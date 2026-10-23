# DR26-07-25-HUB-01-2126 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR26-07-25-HUB-01-2126 — Размещение рутин в персональном AI-флоте (6 машин)

| Поле | Значение |
|------|----------|
| **ID** | DR26-07-25-HUB-01-2126 |
| **Дата** | 2026-07-31 |
| **Тема** | Placement doctrine для scheduled tasks / cron / фоновых роботов в гетерогенном личном флоте |
| **Контекст** | (A) always-on VPS-якорь 16 vCPU / 32 GB, Linux headless, без браузера/GPU, публичный IP, idle ~96%; (B) десктоп-хаб 2×GPU, Chrome с живыми сессиями, резидентный IP, ~24/7, Windows; (C) ноутбуки/Mac операторов, эпизодический uptime |
| **Рельсы** | Syncthing (файлы) + Telegram-чат (живая шина сообщений) |
| **Метод** | Веб-ресёрч (документация оркестраторов, Google SRE, Syncthing, Prefect/Temporal/Nomad/K8s CronJob, healthchecks) |
| **Потребитель** | Решение: куда переселять ~89 рутин с перегруженного хаба |

**Маркировка уверенности:** `[established]` — в документации/каноне; `[emerging]` — практика/индустриальный паттерн без единого стандарта; `[speculative]` — перенос на 6-узловой домашний флот / синтез; `[no source found]` — утверждение не подтверждено вебом.

---

## 0. Executive summary

1. **Узел выбирают не «где свободно», а по hard constraints:** железо (GPU/браузер) → IP/куки (резидентный профиль) → выживание при офлайне соседа (VPS-якорь) → близость к человеку (latency UI) → стоимость перезапуска. Всё остальное — soft affinity. `[emerging]` + синтез из placement-моделей Nomad/Prefect.
2. **Главные анти-паттерны домашнего флота** — два писателя в один master-файл (Syncthing даёт `.sync-conflict-*`, не merge), близнецы-таймеры на двух узлах, «тихие» падения без dead-man's switch, зомби-задачи. Индустрия лечит это single-writer + DB-lock (Airflow), prohibit_overlap / concurrencyPolicy:Forbid (Nomad/K8s), durable execution (Temporal), freshness SLI (Google SRE). Переносимое без K8s: **один canonical scheduler на VPS + capability workers + file-level single-writer + healthchecks по возрасту выхода**. `[established]` по каждому механизму отдельно; связка — `[speculative]`.
3. **Миграция ~89 рутин:** волны по «дешёвым и безопасным» (pure compute/cron без браузера/GPU → VPS), never-migrate (browser/IP-sensitive, GPU, интерактив), success = freshness SLO + 0 dual-writers + hub load drop, не «задача зарегистрирована на VPS».
4. **Метрики:** age of last successful *output* (не heartbeat), schedule drift, idle fraction, dual-writer incidents, miss rate. Пороги — tiered (P0 nightly ≤1.5× period, browser jobs — hub-only liveness).
5. **OSS «личный гетерогенный флот»:** близких «из коробки» мало; наиболее переносимые модели — **Prefect work pools + hybrid workers**, **Rundeck/Salt** (agent or SSH), **Healthchecks** (сторож), **Nomad** (constraints, но heavy), **Temporal** (durable, heavy). Полный аналог «6 машин + Syncthing + TG bus» в open source как продукт **не найден** → `[no source found]` на «готовый personal AI fleet scheduler».

---

## 1. Доктрина размещения: объективные признаки → узел

### 1.1 Признаки (capability tags)

| # | Признак | Что означает объективно | Типичный узел | Уверенность |
|---|---------|-------------------------|---------------|-------------|
| H1 | **Hardware: GPU** | CUDA/embeddings/Whisper/local LLM | **B (hub)** | `[established]` — placement по device/GPU — стандарт Nomad/device plugins ([NVIDIA + Nomad](https://developer.nvidia.com/blog/hashicorp-nomad-gpu-scheduling/), [Nomad placement](https://developer.hashicorp.com/nomad/docs/concepts/scheduling/placement)) |
| H2 | **Hardware: browser + interactive profile** | Живой Chrome/Firefox, cookies, OAuth, human-like fingerprint | **B (hub)**; fallback **C** если оператор online | `[emerging]` — датацентровые/VPS IP и headless часто триггерят anti-bot; residential/home IP предпочтительнее для consumer-сайтов ([браузерная автоматизация / ban-risk](https://www.browserstack.com/guide/how-to-avoid-bot-detection); proxy-индустрия разделяет datacenter vs residential — `[emerging]`) |
| H3 | **Always-on / survive peer offline** | Задача должна идти, даже если hub/ноут выключены | **A (VPS)** | `[established]` — single-machine cron failure domain (Google SRE Cron ch.24) |
| H4 | **Public ingress / webhook** | Нужен стабильный публичный endpoint | **A (VPS)** | `[established]` — VPS + публичный IP — канон self-host |
| H5 | **Low latency to human** | UI, UAC, «нажми на экране», hot inbox | **C (operator machine)** или **B** если человек у хаба | `[emerging]` |
| H6 | **Windows-only API** | Task Scheduler COM, Win-specific paths, Outlook/local COM | **B** или Windows-ноут **C** | `[established]` — платформенный constraint |
| H7 | **Linux/systemd friendly pure compute** | Python/bash, SQLite read, reindex, digests, pure API | **A (VPS)** предпочтительно | `[established]` |
| H8 | **Restart cost / non-idempotent** | Двойной запуск дорог (рассылка, платёж, публичный пост) | Узел с **single-writer + forbid overlap**; не дублировать timer | `[established]` — Google SRE: prefer skip over double-launch ([SRE Book ch.24](https://sre.google/sre-book/distributed-periodic-scheduling/)); K8s `concurrencyPolicy: Forbid` ([K8s CronJob](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)); Nomad `prohibit_overlap` ([Nomad periodic](https://developer.hashicorp.com/nomad/docs/job-specification/periodic)) |
| H9 | **Shared file master (registry, ledger)** | Один canonical writer | **Ровно один узел** + остальные read-only consumers | `[established]` — Syncthing conflict model (не CRDT): `.sync-conflict-*` ([Syncthing syncing](https://docs.syncthing.net/users/syncing.html)) |
| H10 | **Ephemeral / opportunistic** | «Когда ноут включён» | **C** + anacron-like / Persistent timer | `[established]` — anacron / systemd `Persistent=` ([systemd.timer](https://www.freedesktop.org/software/systemd/man/latest/systemd.timer.html)) |

### 1.2 Решающее дерево (decision tree)

```
START: задача T
│
├─[1] Нужен GPU / local heavy ML?
│     YES → B (hub). STOP.  [H1]
│
├─[2] Нужен живой браузер / cookies / consumer-site session / ban-sensitive IP?
│     YES → B (hub).  Если B down и задача P0 human-facing → C (тот оператор, у кого сессия).
│           NEVER A (VPS без браузера + datacenter IP). STOP.  [H2]
│
├─[3] Пишет в shared master-файл (registry, ledger, inventory) И другой узел тоже может писать?
│     YES → назначить РОВНО ОДНОГО writer-owner (обычно A для always-on masters,
│           B только если master локален к Windows/Chrome).
│           Остальные: read-only или append-only outbox + merge job на owner. STOP. [H9]
│
├─[4] Должна выжить при offline B и offline C?
│     YES → schedule + execute на A (VPS). STOP.  [H3]
│
├─[5] Нужен публичный webhook / ingress с интернета?
│     YES → A. STOP.  [H4]
│
├─[6] Требует low-latency к конкретному человеку (UAC, «на экране», hot reply)?
│     YES → C (машина этого человека). Schedule: local. Не dual-cron с A. STOP. [H5]
│
├─[7] Windows-only API / путь / COM?
│     YES → B (или Windows C). STOP.  [H6]
│
├─[8] Pure compute / API / file processing / nightly digests / reindex / digests?
│     YES → A (разгрузить B).  [H7]
│
├─[9] Non-idempotent / дорого при double-run?
│     YES → forbid-overlap + single schedule owner + dead-man on OUTPUT.  [H8]
│           Предпочтительно A как scheduler-of-record.
│
└─[10] Default
      → A, если always-on полезен; иначе B.
      → C только если задача имеет смысл только при живом операторе.
```

### 1.3 Матрица «класс задачи → узел» (прикладная)

| Класс | Примеры | Узел | Почему |
|-------|---------|------|--------|
| Pure batch / nightly ETL | reindex, digests, backups файлов, SQLite maintenance | **A** | idle capacity + survive hub sleep |
| Watchdog / health of fleet | output_freshness, peer liveness, sync parity | **A** (сторож **не** на том, что сторожит) | Google SRE monitoring separation + dead-man pattern |
| Browser automation | FB/X/Grok/Chrome login flows | **B** only | browser + residential IP + sessions |
| GPU jobs | embeddings, Whisper, local models | **B** | 2×GPU |
| Operator-local | screen confirm, OTP physical, Mac Contacts export | **C** | requires human machine |
| Dual-rail messaging | post to TG bus | **any online**; prefer **A** for always-on emitters | A always can post |
| Master registry writers | DR registry, approvals DB if file-backed, fleet audit masters | **single owner**, prefer **A** | avoid Syncthing dual-write |

### 1.4 Soft vs hard

- **Hard (нарушение = broken):** H1, H2, H9 (single-writer), non-idempotent without forbid-overlap.
- **Soft (предпочтение):** «A idle 96% → клади compute на A», «ночь 23–06 Lisbon для heavy», «C не должен нести 24/7».
- **Cost of restart:** если перезапуск дёшев и идемпотентен — можно aggressive retry на любом online node; если дорог — только single schedule + skip-on-overlap (SRE: skip > double).

`[speculative]` как цельное дерево для AI-флота; компоненты placement constraints — `[established]` в Nomad/Prefect.

---

## 2. Анти-паттерны и как индустрия лечит (переносимость без K8s)

### 2.1 Каталог анти-паттернов домашнего флота

| Анти-паттерн | Симптом | Корень | Индустриальное лечение | Перенос на 6 узлов без K8s |
|--------------|---------|--------|------------------------|----------------------------|
| **Два писателя в один master-файл** | `.sync-conflict-DATE-TIME-DEVICE.ext`, «тихая» потеря строк | Syncthing — file sync, не multi-master DB; conflict = rename loser ([docs](https://docs.syncthing.net/users/syncing.html)) | Single-writer; DB row locks (Airflow HA scheduler uses `SELECT … FOR UPDATE` ([Airflow scheduler HA](https://airflow.apache.org/docs/apache-airflow/stable/administration-and-deployment/scheduler.html))); outbox pattern | **Назначить owner-node per master path**; consumers read-only; writers → append `outbox/<node>/` → owner merges. `[established]` conflict model + `[emerging]` outbox |
| **Таймеры-близнецы** | Job runs 2× (2 email, 2 post) | cron на A **и** на B с одним смыслом | K8s `concurrencyPolicy: Forbid/Replace` ([CronJob](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)); Nomad `prohibit_overlap = true` ([periodic](https://developer.hashicorp.com/nomad/docs/job-specification/periodic)); Google cron: one leader via Paxos ([SRE ch.24](https://sre.google/sre-book/distributed-periodic-scheduling/)) | **Один schedule-of-record** (обычно A systemd timer / Task on A). Hub/C могут быть *workers*, не *schedulers*. Inventory: unique job_id → exactly one enabled timer |
| **Тихие падения** | exit≠0 проглочен; cron «жил», output мёртв | Monitoring launch ≠ monitoring effect | Dead man's switch / Healthchecks.io ([self-hosted](https://healthchecks.io/docs/self_hosted/), [GitHub](https://github.com/healthchecks/healthchecks)); SRE **freshness** SLI for pipelines ([SRE Workbook Implementing SLOs](https://sre.google/workbook/implementing-slos/)) | Ping **после** записи артефакта; alert on **age(output)** > threshold, not on process alive. Watchdog на **другом** узле (A сторожит B) |
| **Sync-конфликты как «норма»** | Конфликт-копии копятся, master «раздваивается» | Simultaneous edit while both online or reconnect after dual offline edit | Syncthing: older mtime loses; conflicts propagate as normal files ([docs](https://docs.syncthing.net/users/syncing.html)); `maxConflicts` config | **Design for zero concurrent writers**; CI/guard: fail if any `*.sync-conflict-*` under masters. Version masters as append-only logs where possible |
| **Задачи-зомби** | Disabled half, orphan Task Scheduler entries, dead paths after migrate | No inventory; schedule ≠ code | Inventory + suspend flag (K8s `.spec.suspend`); history limits | **Registry of all timers** (id, node, schedule, owner, artifact path, last_ok). Nightly audit: enabled-but-no-output-30d → quarantine |
| **Thundering herd в :00** | 30 daily jobs at midnight | Human crontab habits | Google cron `?` hash-spread ([SRE ch.24](https://sre.google/sre-book/distributed-periodic-scheduling/)) | Jitter: `H(job_id) % 60` minutes; night window spread 23:00–04:30 |
| **Scheduler on the watched system** | Watchdog dies with hub | Fate-sharing | Independent monitoring plane | Freshness probe on A for B’s outputs; B probes A |
| **Idempotency ignored** | Double launch corrupts | Fire-and-forget cron | Prefer skip over double (Google); jobs should be idempotent where possible (K8s docs) | Classify each job: **idempotent / at-most-once / at-least-once**; encode in registry |

### 2.2 Что переносимо из «больших» систем (без Kubernetes)

| Система | Ключевая идея | Перенос (AK-47) | Не переносить |
|---------|---------------|-----------------|---------------|
| **systemd timers** | `OnCalendar`, `Persistent=`, `RandomizedDelaySec`, unit isolation | A (Linux VPS): primary timer plane | Не тащить на Windows без WSL; Persistent ≠ «всегда поймал пропуск» для sub-minute |
| **Windows Task Scheduler** | Local always-on when hub up | B-only for Windows/browser jobs | Не дублировать те же job id на A |
| **Nomad periodic** | constraints (OS, meta, GPU), `prohibit_overlap`, batch/sysbatch | **Идея** constraints + node meta (`role=vps|hub|laptop`); full Nomad optional (heavy for 6 nodes) | Полный Nomad cluster ради 89 cron — overkill unless already ops-ready |
| **K8s CronJob** | concurrencyPolicy, startingDeadlineSeconds, history limits | Перенести **политику**: Forbid default; deadline; keep N last logs | Не ставить K8s «чтобы было» |
| **Airflow** | Metadata DB as source of truth; HA via DB locks | SQLite/Postgres **job registry** with lease/lock for «who runs now» | Full Airflow UI/DAG overhead for home fleet — often overkill ([community critiques](https://medium.com/@ApacheDolphinScheduler/airflow-is-overkill-for-most-teams-heres-a-better-option-eb4bf7c2df7e) — `[emerging]` opinion) |
| **Temporal** | Durable execution; cron/schedules; task queues → workers | Task queue per capability: `q-gpu`, `q-browser`, `q-cpu`; workers on B/A | Self-host Temporal heavy; Cron Jobs deprecated in favor of Schedules ([Temporal cron](https://docs.temporal.io/cron-job)) |
| **Prefect** | Work pools + hybrid workers poll orchestration | **Лучший mental model для гетерогенного флота**: orchestrator on A, workers on A/B/C ([work pools](https://docs.prefect.io/v3/concepts/work-pools)) | Cloud lock-in if not self-host; Windows worker caveats |
| **Rundeck / Salt** | Multi-node runbooks, schedules, inventory | Homelab-proven remote execution ([Rundeck homelab](https://www.reddit.com/r/homelab/comments/gkxut6/using_rundeck_to_automate_tasks_on_my_homelan/)); Salt `schedule` + reactors ([Salt schedule](https://docs.saltproject.io/en/latest/topics/jobs/schedule.html)) | Another daemon to babysit; auth surface |
| **Healthchecks** | Dead-man pings | Self-host on A; every P0/P1 job pings on success | Не заменять domain checks «curl ok» без артефакта |

### 2.3 Минимальная «индустриальная» архитектура для 6 узлов (рекомендуемый каркас)

```
[Schedule-of-record: A (VPS)]
        │  decides WHEN
        ▼
[Dispatch: file bus / TG / local queue]
        │  job_id + params + not_before
        ▼
[Workers by capability]
   A-worker: pure CPU, API, file ETL, webhooks
   B-worker: GPU + browser + Windows
   C-worker: opportunistic / human-local
        │
        ▼
[Artifacts on Syncthing paths]  ← single-writer per path
        │
        ▼
[Freshness monitors on A]  ← age(output), not PID
        │
        ▼
[Alert rail: TG chat]
```

`[speculative]` как сборка; каждый блок имеет прецедент выше.

---

## 3. Волновой план миграции ~89 рутин (hub → VPS)

### 3.1 Инвентаризация до любой волны (обязательный gate)

Для каждой рутины зафиксировать (таблица/registry):

| Поле | Зачем |
|------|--------|
| `job_id` | unique |
| `current_node` | сейчас почти всё B |
| `schedule` | cron expression + timezone |
| `capabilities[]` | gpu, browser, windows, always_on, human_local, public_ingress, writes_master |
| `idempotency` | yes / no / unknown |
| `artifact_path` | где «выход» для freshness |
| `max_runtime` | для forbid-overlap |
| `blast_radius` | P0/P1/P2 (double-run cost, user-visible) |
| `dependencies` | other jobs / human |

Без inventory миграция = перенос хаоса. `[emerging]` ops practice; Google tracks launch state for distributed cron (`[established]` need for state).

### 3.2 Приоритизация волн

**Скоринг (чем выше — раньше на A):**

```
score = always_on_value
      + pure_compute
      + hub_cpu_relief
      - browser_need
      - gpu_need
      - windows_only
      - dual_write_risk_if_moved_wrong
      - non_idempotent_without_lock
```

| Волна | Что | Доля (оценка) | Критерий входа | DoD волны |
|-------|-----|---------------|----------------|-----------|
| **W0 — Foundation** | Registry + single-writer map + healthchecks self-host on A + conflict scanner | — | — | 0 unknown owners; `*.sync-conflict-*` alert |
| **W1 — Safe pure** | Nightly digests, reindex, pure API pollers, log rotation, report builders, dead-man itself | ~25–40% | no browser/gpu/windows; idempotent or easy retry | freshness OK 7d; hub timer **disabled** not dual |
| **W2 — Always-on coordination** | bus emitters, peer health, inventory audits, DR collect, lightweight watchers | ~15–25% | need survive hub reboot | hub offline 2h drill: jobs still fire |
| **W3 — Stateful single-writer** | master registries that currently thrash on hub | ~10% | rewrite to owner-A or outbox | 0 conflicts 14d |
| **W4 — Conditional** | jobs that *can* run on A but need secret/API only on hub | ~5–10% | move secrets or add remote worker call B | documented path |
| **NEVER** | see §3.3 | rest | — | stay on B/C |

Проценты — **оценка порядка**, не замер вашего инвентаря → `[speculative]`.

### 3.3 Что мигрировать НИКОГДА (или только с явной сменой capability)

| Категория | Почему never → A | Узел |
|-----------|------------------|------|
| Browser automation (FB, X, Grok via Chrome, OAuth UI) | Нет браузера на A; datacenter IP / headless risk | **B** |
| GPU workloads | Нет GPU на A | **B** |
| Jobs requiring residential IP reputation | Public VPS IP ≠ home | **B** |
| Interactive / screen / UAC / physical OTP | Нужен человек/локальный UI | **C** (или B) |
| Windows-only COM/paths without port | Linux VPS | **B** |
| Dual-writer «оставим на всякий случай на двух» | Близнецы | **запрет** |
| «Перенесём scheduler, оставим старый enabled» | Double launch | **запрет** — cutover checklist |

### 3.4 Паттерн cutover одной волны (zero double-run)

1. **Shadow (3–7 дней):** job runs on A, writes to `artifact.shadow` or dry-run log; B still production.
2. **Compare:** shadow output freshness + diff sample vs prod (where deterministic).
3. **Flip:** enable A prod path; **disable B timer in same change window**; registry status=`migrated`.
4. **Bake:** 7 days freshness SLO; no dual pings.
5. **Retire:** remove B task definition (not just disable — zombie prevention).

Аналоги: progressive delivery / dual-run then cutover — стандарт миграций batch; точной «89 home fleet» статьи **не найдено** → процесс `[emerging]` из release engineering.

### 3.5 Доказательство успеха волны — цифры

| Метрика | Как считать | Цель волны |
|---------|-------------|------------|
| **Jobs migrated** | count in registry `node=A` | per wave target |
| **Hub schedule count** | enabled timers on B | ↓ (доказать drop) |
| **Dual-schedule incidents** | same job_id enabled on >1 node | **0** |
| **Output freshness P0** | % jobs with age(output) ≤ SLO | ≥ 99% over 7d |
| **Conflict files** | count `*.sync-conflict-*` under masters | **0** new |
| **Hub CPU/RAM at night** | before/after window | measurable relief |
| **Missed runs** | expected_runs − successful_outputs | → 0 for P0 |
| **Idle waste** | jobs that succeed but artifact unchanged / no consumer | flag, don’t count as «healthy» |

**Не считать успехом:** «crontab line exists on VPS», «process started», «heartbeat file touched by wrapper without business output».

---

## 4. Метрики здоровья размещения

### 4.1 Что мерить (и почему)

| Метрика | Определение | Почему не «запуск» |
|---------|-------------|-------------------|
| **Output age / freshness** | `now - mtime(or internal stamp) of consumer-visible artifact` | Google pipeline freshness SLI: proportion of data newer than threshold ([SRE Workbook](https://sre.google/workbook/implementing-slos/)); dead-man after success ([healthchecks](https://github.com/healthchecks/healthchecks)) |
| **Schedule drift** | `actual_start - scheduled_start` | Catch overload, sleep, thundering herd; K8s tracks missed starts / startingDeadlineSeconds |
| **Success ratio** | successful_outputs / expected_slots | Expected from cron math |
| **Overlap / concurrency violations** | second instance while first alive | Forbid policy health |
| **Dual-writer / conflict rate** | new sync-conflict files / day | Placement integrity |
| **Idle fraction (node)** | 1 − busy_time/window | A at 96% idle is OK; B overloaded is signal to migrate |
| **Idle fraction (job)** | runs with no delta / no consumer | «Вхолостую» — job should be paused or fixed |
| **Node coverage** | % time required node was up when job needed | C machines: opportunistic |
| **Watchdog independence** | freshness checker runs on different node than producer | Fate-sharing avoidance |

### 4.2 Пороги (практические, tiered)

| Tier | Пример | Freshness SLO (age) | Drift max | Miss alert |
|------|--------|---------------------|-----------|------------|
| **P0** | money, public post pipeline, security, master registry | ≤ 1.5 × period (e.g. hourly → 90 min) | 2× period | page (TG) immediately after grace |
| **P1** | daily digests, CRM sync | ≤ 1 period + 6h grace for daily | 6h | same day |
| **P2** | weekly, best-effort, laptop-local | ≤ 2 × period or next human session | n/a | weekly digest |
| **Browser/GPU** | hub-only | if hub down > X h → degraded mode alert, not «run on A» | — | separate hub-up SLO |

Grace periods — суть dead-man: period + grace ([healthchecks model](https://healthchecks.io/docs/self_hosted/)). Конкретные минуты подгонять под period.

### 4.3 «Вхолостую» (idle success)

- **Node idle high on A:** норма; цель миграции — занять A полезным, не 100% load.
- **Job idle:** exit 0, but artifact stamp unchanged while inputs changed → **false green** → count as fail for freshness-of-effect.
- **Empty consumer:** output exists, nobody reads → product fail, not infra fail; track separately (connect rule).

### 4.4 Каноническая проверка (псевдо)

```
for job in registry:
  expected = schedule.next_windows(lookback)
  age = now - artifact.success_stamp(job)
  if age > job.slo_age: ALERT(job, age)
  if enabled_on(nodes) > 1: ALERT(dual_schedule)
  if master_path and writers(job)>1: ALERT(dual_writer)
  if sync_conflict_glob(master_roots): ALERT(conflict)
```

Сторож на **A** для jobs на **B**, и отдельный probe с **B** для «A alive» (симметрия).

---

## 5. Open-source: кто решает «личный флот разнородных машин»

| Проект | Модель | Плюсы для 6 узлов | Ограничения | URL |
|--------|--------|-------------------|-------------|-----|
| **Prefect** (OSS server + workers) | Orchestrator + **work pools**; hybrid workers poll | Явное разделение schedule vs capability workers; process pool for local | Нужен always-on API (A); ops surface | https://docs.prefect.io/v3/concepts/work-pools |
| **Healthchecks** | Dead-man switch SaaS/self-host | Идеальный слой «возраст пинга/выхода»; лёгкий | Не scheduler jobs; только мониторинг | https://github.com/healthchecks/healthchecks |
| **Rundeck** | Central schedule + node inventory + remote exec | Homelab multi-node jobs | Java stack; другой UI/mental model | https://docs.rundeck.com/ |
| **Salt** | Minions + `schedule` + reactors | Flexible remote; event-driven | Agent footprint; learning curve | https://docs.saltproject.io/en/latest/topics/jobs/schedule.html |
| **Nomad** | Bin packing + constraints + periodic batch | GPU/OS constraints native; prohibit_overlap | 3-server consensus heavy for home | https://developer.hashicorp.com/nomad/docs/job-specification/periodic |
| **Temporal** | Workflows + task queues + Schedules | Durable, retries, exactly-once-ish patterns | Heavy self-host; overkill for simple cron | https://docs.temporal.io/cron-job |
| **Airflow** | DAG scheduler + executors | Mature; HA via DB | Heavy; multi-scheduler needs DB locks | https://airflow.apache.org/docs/apache-airflow/stable/administration-and-deployment/scheduler.html |
| **systemd timers** | Per-host | Zero extra deps on A | No cross-node placement | https://www.freedesktop.org/software/systemd/man/latest/systemd.timer.html |
| **Syncthing** | File sync only | Уже у вас; delivery plane | **Not** a job scheduler; conflicts on multi-write | https://docs.syncthing.net/users/syncing.html |
| **Google SRE Cron (описан, не OSS продукт)** | Paxos leader, fail-closed | Theory of skip vs double | Не open-source product | https://sre.google/sre-book/distributed-periodic-scheduling/ |

**Вывод:** готового OSS «personal AI fleet of 6 with Syncthing + Telegram bus + browser hub + VPS anchor» **не обнаружено** → `[no source found]`. Ближайшая **переносимая модель** — Prefect-like work pools **или** самописный thin registry (AK-47) + systemd on A + Task Scheduler on B + Healthchecks + single-writer discipline.

---

## 6. Источники (ключевые)

1. Google SRE Book — Distributed Periodic Scheduling with Cron: https://sre.google/sre-book/distributed-periodic-scheduling/
2. Google SRE Workbook — Implementing SLOs (freshness): https://sre.google/workbook/implementing-slos/
3. Kubernetes CronJob (concurrencyPolicy, limitations, idempotency): https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/
4. HashiCorp Nomad — periodic jobs: https://developer.hashicorp.com/nomad/docs/job-specification/periodic
5. HashiCorp Nomad — placement / constraints: https://developer.hashicorp.com/nomad/docs/concepts/scheduling/placement
6. NVIDIA — Nomad GPU scheduling: https://developer.nvidia.com/blog/hashicorp-nomad-gpu-scheduling/
7. Apache Airflow — scheduler HA / locks: https://airflow.apache.org/docs/apache-airflow/stable/administration-and-deployment/scheduler.html
8. Temporal — Cron Job / Schedules: https://docs.temporal.io/cron-job
9. Prefect — Work pools: https://docs.prefect.io/v3/concepts/work-pools
10. Syncthing — conflict handling: https://docs.syncthing.net/users/syncing.html
11. systemd.timer: https://www.freedesktop.org/software/systemd/man/latest/systemd.timer.html
12. Healthchecks (OSS): https://github.com/healthchecks/healthchecks · self-host: https://healthchecks.io/docs/self_hosted/
13. Dead-man switch pattern (practice): https://dev.to/krissv/how-to-add-dead-mans-switch-monitoring-to-any-cron-job-in-2-minutes-3ebj
14. Salt schedule: https://docs.saltproject.io/en/latest/topics/jobs/schedule.html
15. Browser bot-detection / automation risk (emerging): https://www.browserstack.com/guide/how-to-avoid-bot-detection

---

## 7. Что делать нам (5–7 конкретных рекомендаций)

1. **Ввести Job Registry (один master, writer = VPS A).**  
   Поля: id, schedule, node, capabilities, artifact_path, slo_age, idempotency, enabled. Запрет: два `enabled=true` на одном id. Потребитель: audit + migration waves.  
   *Доказательство:* SQL/JSON count dual-enabled = 0.

2. **Schedule-of-record = A; B/C = capability workers, не вторые cron-демоны.**  
   Pure compute/API/nightly (~W1–W2) — systemd timers на A. Browser/GPU — только B Task Scheduler.  
   *Доказательство:* после W1 число enabled tasks на hub ↓, freshness на A ↑.

3. **Single-writer map для всех Syncthing masters.**  
   Каждый master path → один owner node; остальные read-only или outbox. Nightly fail if `*.sync-conflict-*` appears under masters.  
   *Источник:* Syncthing conflict semantics.

4. **Freshness > heartbeat: self-host Healthchecks (or thin clone) on A.**  
   Ping only after artifact write; P0 grace = 1.5× period; watchdog for B’s browser jobs runs on A.  
   *Источник:* SRE freshness + healthchecks dead-man.

5. **Миграция волнами W0→W1→W2, never-list жёсткий.**  
   W0 registry+conflicts+health; W1 pure; W2 always-on coordination. Browser/GPU/IP-sensitive **никогда** на A. Cutover = shadow → flip → disable old → bake 7d.  
   *Доказательство волны:* dual=0, freshness 7d, hub load drop.

6. **Forbid-overlap + classify idempotency.**  
   Для non-idempotent (posts, outreach, money-adjacent): one lock file/DB lease or skip if previous running; prefer skip over double (Google SRE).  
   *Доказательство:* zero double-send incidents post-policy.

7. **Не ставить K8s/Nomad/Temporal «ради красоты» (AK-47).**  
   Если понадобится общий оркестратор — **Prefect self-host work pools** или thin custom dispatcher; иначе registry + systemd + Task Scheduler + TG alerts достаточно.  
   *Исключение:* если уже есть ops-готовность к Nomad — constraints meta `role=vps|hub` полезны; иначе цена > выгода на 6 узлах. `[speculative]` cost judgment.

---

## 8. Открытые пробелы / [no source found]

| Утверждение | Статус |
|-------------|--------|
| Готовый OSS «personal AI agent fleet scheduler for Syncthing+Telegram» | **[no source found]** |
| Универсальные числовые SLO «1.5× period» как industry standard | **[emerging]** (practice synthesis, not single RFC) |
| Точный % из 89 jobs мигрируемых на VPS без inventory | **[no source found]** — нужен ваш registry dump |
| «Residential IP всегда безопаснее datacenter для всех сайтов» | **[emerging]** — зависит от сайта/антибота; не абсолют |

---

## 9. Карта решений (one-screen)

```
           нужен GPU? ──yes──► HUB (B)
                │ no
           нужен browser/cookies/residential? ──yes──► HUB (B)
                │ no
           shared master write? ──yes──► ONE owner (prefer VPS A)
                │ no
           must survive hub/laptop off? ──yes──► VPS (A)
                │ no
           human-local UI/OTP? ──yes──► LAPTOP/MAC (C)
                │ no
           windows-only? ──yes──► HUB (B)
                │ no
           default pure compute ──► VPS (A)
```

---

*Конец отчёта DR26-07-25-HUB-01-2126. Все нетривиальные внешние утверждения снабжены URL; синтез для 6-машинного AI-флота помечен `[speculative]`/`[emerging]` где нет прямого канона.*
