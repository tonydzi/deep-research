# DR26-07-25-HUB-01-2126 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: OpenAI Codex CLI (local headless on Mac16, ChatGPT subscription — NOT paid API), model gpt-5.6-sol
topic: Размещение рутин в персональном AI-флоте: куда что селить между always-on VPS-якорем (16vCPU/32GB, 96% простой) и десктоп-хабом с GPU/Chrome/резидентным IP — доктрина размещения, в
dr_id: DR26-07-25-HUB-01-2126
date: 2026-07-31

rail: codex exec --skip-git-repo-check, live web search, exit 0
sources: 4 unique URLs cited · 2 web-search calls in stderr

note: headless CLI rail — LOCAL session, no chatgpt.com/c/ URL exists for this run
---

# Decision memo

Переносить на VPS следует не «всё, что запускается без GPU», а весь устойчивый control plane и большинство headless CPU/API‑рутин. Десктоп должен остаться capability‑worker’ом для задач, которым действительно нужны GPU, интерактивный Chrome, локальные данные, аппаратные ключи или резидентный IP.

Целевая схема:

```text
                    ALWAYS-ON VPS
       расписания · очереди · состояние · API · watchdog
       CPU/IO jobs · retries · журнал · метрики · секреты
                           │
                 lease + heartbeat + TTL
                           │
                    DESKTOP WORKER
         GPU · Chrome sessions · resident IP · local files
         human-in-the-loop · hardware-bound credentials
```

Главный результат миграции — не максимальная загрузка VPS, а исчезновение зависимости автоматики от того, включён ли десктоп.

В доступной папке самого реестра 89 рутин нет. Поэтому числа ниже — квоты волн, а не выдуманное построчное распределение. После добавления реестра эту схему можно применить к каждой строке без изменения доктрины.

## 1. Доктрина размещения

### Селить на VPS

Рутина по умолчанию принадлежит VPS, если она:

- должна стартовать по расписанию или входящему событию;
- принимает webhook, почту, очередь либо API-запрос;
- использует SaaS/API, SSH, Git, базы, RSS, HTTP;
- выполняет CPU‑парсинг, преобразование файлов, индексацию, embeddings через внешний API;
- следит за состоянием других машин;
- должна переживать перезагрузку, сон или отсутствие человека;
- хранит authoritative state: очередь, ledger запусков, locks, audit log;
- способна работать headless и не зависит от домашней сетевой идентичности.

При 96% простоя VPS выглядит естественным местом для таких работ, но это не лицензия на overcommit. На старте разумно отдать воркерам максимум около 10–12 vCPU и 18–22 GB RAM, оставив запас control plane, файловому кэшу и аварийному всплеску. Контейнеры без лимитов по умолчанию могут потреблять все доступные ресурсы, поэтому CPU/RAM limits здесь обязательны. [Docker: Resource constraints](https://docs.docker.com/engine/containers/resource_constraints/)

### Оставить на десктопе

Только рутины, для которых существует проверяемая capability‑причина:

- CUDA, локальная GPU‑инференция, OCR/STT/video с реальным преимуществом GPU;
- залогиненный Chrome и сайт без надёжного API;
- WebAuthn, passkey, аппаратный токен или ручное подтверждение;
- сервис чувствителен к облачным IP или требует домашнего/резидентного адреса;
- большие локальные файлы, перенос которых дороже вычисления;
- интерактивная визуальная проверка;
- лицензия или приложение аппаратно привязаны к десктопу.

«Так исторически работает» и «скрипт лежит на десктопе» capability‑причинами не являются.

Chrome‑автоматизацию нужно изолировать от личного профиля. Google прямо связывает remote debugging с кражей cookies; с Chrome 136 debugging стандартного профиля дополнительно ограничен, а для автоматизации рекомендуется отдельный профиль или Chrome for Testing. [Chrome Developers](https://developer.chrome.com/blog/remote-debugging-port)

### Разделять между узлами

Гибридная рутина должна разрезаться по границе способности:

1. VPS принимает событие и создаёт job.
2. Десктоп арендует job с TTL.
3. Выполняет только GPU/browser/residential‑фрагмент.
4. Возвращает артефакт и структурированный результат.
5. VPS валидирует, сохраняет состояние, уведомляет и решает retry.

Если десктоп недоступен, job остаётся в очереди, а не исчезает и не считается успешно завершённым.

### Не мигрировать до переделки

Карантин нужен для рутин, которые:

- неидемпотентны и могут дважды отправить письмо, пост или платёж;
- используют общий Chrome‑профиль;
- хранят секреты в коде, shell history или `.env` общего доступа;
- имеют одновременно scheduler на обоих узлах;
- полагаются на локальное абсолютное имя файла;
- не имеют machine-readable результата и exit status;
- требуют destructive action без preview/approval;
- неизвестно кем используются либо зачем существуют.

## 2. Формальный placement gate

Для каждой рутины в реестре нужны поля:

```yaml
id:
owner:
trigger: schedule | event | manual
capabilities: [cpu, gpu, chrome, resident_ip, local_data, human]
authoritative_state:
side_effects:
idempotency_key:
secrets:
input_location:
output_location:
p95_runtime:
cpu_peak:
ram_peak:
vram_peak:
network_egress:
retry_policy:
max_staleness:
fallback:
current_host:
target_host:
rollback:
```

Последовательность решения:

1. `GPU/WebAuthn/local-only data` → desktop worker.
2. `Resident IP` → desktop либо отдельно доказанный residential egress.
3. `Browser`, но без личной сессии → сначала проверить headless на VPS.
4. `Always-on/event/state/watchdog` → VPS.
5. Смешанный набор → split.
6. Неизвестные side effects или секреты → quarantine.

Для приоритета миграции можно использовать:

```text
priority =
  3 × availability_gain
+ 2 × idle_capacity_fit
+ 2 × operational_simplicity
+ 1 × expected_frequency
- 3 × side_effect_risk
- 2 × desktop_capability_strength
- 2 × migration_effort
```

Каждый фактор оценивается от 0 до 3. Hard fences имеют приоритет над итоговым score.

## 3. Волновой план для 89 кандидатов

| Волна | Количество | Что входит | Условие выхода |
|---|---:|---|---|
| 0. Инвентаризация | 89 | Все кандидаты: owner, capability, state, side effects, baseline | 89/89 классифицированы; неизвестные отправлены в quarantine |
| 1. Канарейки | 15 | Read-only, API, мониторинг, sync, отчёты, CPU jobs | 14 дней; ≥99% успешных запусков; ноль потерянных/двойных effects |
| 2. Основной VPS‑пул | 30 | Schedulers, ingestion, ETL, индексация, уведомления, backups | p95 не хуже baseline >20%; rollback проверен |
| 3. Split‑рутины | 24 | Оркестрация на VPS, Chrome/GPU/IP‑шаг на десктопе | Удаление десктопа из сети не теряет job; lease recovery проходит |
| 4. Высокий риск | 20 | Authenticated browser, публикации, локальные данные, legacy | Human approval для необратимых effects; отдельные профили и секреты |

Итого: `15 + 30 + 24 + 20 = 89`.

Это не означает, что все 89 должны сменить хост. Ожидаемый здоровый исход:

- 50–65% — VPS;
- 20–30% — split;
- 10–20% — desktop-only;
- часть — удалить, объединить или оставить в карантине.

### Режим каждой миграции

Для каждой рутины:

1. Записать baseline за 7–14 дней.
2. Запустить shadow без внешних side effects.
3. Сравнить результат со старым исполнением.
4. Перевести 10% запусков на новый путь.
5. Затем 50%, после этого 100%.
6. Старый путь оставить выключенным, но доступным для rollback на 7 дней.
7. Только потом удалить старый scheduler.

Для редких месячных задач календарных 14 дней недостаточно: требуется минимум 3 успешных эквивалентных прогона или синтетический replay.

## 4. Протокол между VPS и десктопом

Минимальная модель job:

```text
queued → leased → running → succeeded
             │         │
             └─TTL─────┴→ queued / dead-letter
```

Обязательные свойства:

- уникальный `job_id`;
- отдельный `idempotency_key` для внешнего эффекта;
- lease с TTL, а не вечный флаг `running`;
- heartbeat десктопа;
- ограниченное число retry с exponential backoff и jitter;
- dead-letter queue;
- checksum входов и выходов;
- stdout/stderr плюс структурированный result;
- артефакты записываются атомарно: temp → validate → rename;
- время хранится в UTC, UI показывает локальную зону;
- часы обоих узлов синхронизированы.

Сетевой доступ — deny-by-default: VPS может выдавать десктопу jobs, но десктоп не должен получать широкий root-доступ к VPS. В overlay-сети машины следует идентифицировать как сервисные узлы и разрешать доступ по назначению/порту; Tailscale, например, поддерживает device tags и декларативные grants для такого ограничения. [Device tags](https://tailscale.com/docs/features/tags), [Grants syntax](https://tailscale.com/kb/1538/grants-syntax)

## 5. Анти‑паттерны

- **VPS как удалённый рабочий стол.** Перенос личного Chrome‑профиля в облако увеличивает поверхность атаки.
- **Десктоп как control plane.** Сон, reboot, update или уход пользователя останавливают весь флот.
- **Два активных cron для надёжности.** Это создаёт двойные side effects, а не HA.
- **Распределённая общая папка как очередь.** Нет корректных leases, fencing и ясной семантики ошибок.
- **SSH-команда с VPS и ожидание до завершения.** Обрыв соединения превращает статус исполнения в загадку.
- **Retry всей цепочки.** Повторять следует безопасный шаг, а не уже выполненную публикацию.
- **Один Chrome‑профиль человеку и агентам.** Ошибка агента получает весь набор cookies и сессий.
- **Миграция ради загрузки железа.** Утилизация — диагностическая метрика, не цель.
- **GPU для любой AI‑задачи.** API‑вызов, routing или небольшой CPU‑классификатор зачастую дешевле эксплуатационно.
- **Resident IP как универсальная отмычка.** Он должен быть capability с allowlist доменов, а не общим outbound proxy.
- **Секреты, доставляемые вместе с job payload.** Worker должен получать только минимальный scoped credential.
- **“Success = exit 0”.** Для значимой рутины нужен проверяемый постусловный эффект.

## 6. Метрики и SLO

### Надёжность

- `successful_runs / eligible_runs`;
- missed schedule rate;
- duplicate side-effect rate — целевой hard SLO: `0`;
- stuck leases и recovered leases;
- queue age p50/p95/max;
- mean time to detect и mean time to recover;
- desktop-unavailable jobs, пережившие outage без потери.

### Производительность

- queue wait, execution time и end-to-end latency отдельно;
- CPU/RAM/IO p95 на VPS;
- GPU utilization, VRAM peak и cold-start на десктопе;
- network bytes и transfer time на job;
- параллелизм и throttled jobs.

Не следует делать целью «поднять VPS с 4% до 90%». Здоровый ориентир — отсутствие устойчивой saturation: CPU p95 ниже примерно 70%, RAM без swap thrashing, достаточный disk headroom и предсказуемая очередь.

### Качество и экономика

- результат, совпадающий с golden/postcondition;
- human correction rate;
- стоимость успешного результата, включая API и электричество;
- operator minutes на 100 запусков;
- число инцидентов и ручных рестартов;
- миграционные часы / ежемесячно сэкономленные operator-hours.

### Показатели правильности архитектуры

Через 30 дней после миграции:

- 100% расписаний и authoritative state находятся на VPS;
- выключение десктопа на 24 часа не теряет задания;
- desktop-only jobs автоматически догоняют очередь после включения;
- ни одна рутина не имеет двух владельцев расписания;
- 100% внешних side effects имеют idempotency либо approval fence;
- 100% Chrome‑автоматизаций используют выделенные профили;
- ≥95% запусков имеют коррелируемые logs и конечный статус;
- rollback одной рутины занимает менее 15 минут.

## Итоговое решение

VPS следует превратить из простаивающего сервера в **якорь управления и исполнения обычных рутин**. Десктоп — не второй равноправный scheduler, а **редкий capability‑остров**, берущий jobs по lease. Такой дизайн использует свободные 16 vCPU/32 GB, не переносит в облако домашнюю identity‑поверхность и сохраняет Chrome/GPU/residential‑преимущества там, где они действительно необходимы.

Единственное, чего нельзя честно сделать без исходного реестра, — назвать конкретные 89 `id → host`. Для этого достаточно добавить таблицу/CSV: приведённые поля, hard fences и квоты волн уже образуют готовый placement pass.
