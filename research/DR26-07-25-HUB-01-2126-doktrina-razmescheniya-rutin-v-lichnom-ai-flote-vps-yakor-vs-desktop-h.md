---
dr_id: DR26-07-25-HUB-01-2126
title: "Доктрина размещения рутин в личном AI-флоте: VPS-якорь vs десктоп-хаб vs ноуты"
date: 2026-07-25
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-25-HUB-01-2126): Доктрина размещения рутин в личном AI-флоте: VPS-якорь vs десктоп-хаб vs ноуты

> Deep research отвечает, какой узел (always-on VPS без GPU/браузера, десктоп-хаб с GPU/Chrome/резидентным IP, или ноуты операторов) должен исполнять каждую из ~89 рутин, какими анти-паттернами грозит гетерогенный флот и как индустрия (Google SRE, K8s, Nomad, Temporal, Prefect, Airflow, Syncthing, Healthchecks) их лечит без развёртывания Kubernetes.

## Ключевые выводы
- Узел выбирается по hard constraints в строгом порядке: железо (GPU/браузер) → IP/куки (резидентный профиль) → выживание при офлайне соседа (VPS-якорь) → близость к человеку (latency UI) → стоимость перезапуска; всё остальное — soft affinity
- 10 объективных признаков (H1-H10) детерминированно определяют узел: GPU→hub(B); живой браузер/cookies/residential IP→hub(B), никогда VPS(A); shared master-файл→ровно один writer-owner; must-survive-offline→VPS(A); публичный webhook→VPS(A); low-latency-to-human→laptop(C); Windows-only API→hub(B); pure compute→VPS(A) предпочтительно; non-idempotent→single-writer+forbid-overlap; ephemeral→C+persistent timer
- Главные анти-паттерны домашнего флота: два писателя в один master-файл (Syncthing даёт .sync-conflict-*, не merge — не CRDT), таймеры-близнецы на двух узлах (двойной запуск), тихие падения без dead-man's switch, sync-конфликты как норма, задачи-зомби без inventory, thundering herd в полночь, watchdog на том же узле что и объект наблюдения
- Индустрия лечит это: single-writer + DB row locks (Airflow SELECT FOR UPDATE), concurrencyPolicy:Forbid (K8s CronJob) / prohibit_overlap (Nomad periodic), durable execution (Temporal), freshness SLI вместо heartbeat (Google SRE Workbook), Healthchecks.io dead-man pattern (ping после записи артефакта, не после запуска процесса)
- Переносимая без Kubernetes архитектура для 6 узлов: один canonical scheduler на VPS(A) + capability workers по узлам (A=CPU/API/webhooks, B=GPU+browser+Windows, C=opportunistic/human-local) + Syncthing-артефакты с single-writer per path + freshness-мониторы на A, независимые от продюсера
- Готового open-source «personal AI fleet scheduler» (6 машин + Syncthing + Telegram bus + browser hub + VPS anchor) не найдено ([no source found]); ближайшая переносимая mental model — Prefect work pools + hybrid workers, либо самописный thin registry + systemd(A)/Task Scheduler(B) + Healthchecks
- Волновой план миграции ~89 рутин: W0-Foundation (registry+single-writer map+healthchecks+conflict scanner) → W1-Safe pure (~25-40%, nightly digests/reindex/API pollers, без browser/gpu/windows) → W2-Always-on coordination (~15-25%, bus emitters/peer health/DR collect) → W3-Stateful single-writer (~10%, master registries) → W4-Conditional (~5-10%, нужны secrets только на hub) → NEVER-list (browser automation, GPU, residential-IP-dependent, interactive/UAC, Windows-only COM) — остаются на B/C навсегда
- Cutover-паттерн одной волны против double-run: Shadow (3-7 дней, A пишет в artifact.shadow, B остаётся production) → Compare → Flip (включить A prod, отключить B timer в том же change window) → Bake (7 дней freshness SLO) → Retire (удалить, не просто disable, B-таск — иначе зомби)
- Метрики здоровья размещения: age of last successful OUTPUT (не heartbeat/PID), schedule drift, success ratio, overlap/concurrency violations, dual-writer/conflict rate, idle fraction узла (A на 96% idle — норма) и idle fraction задачи (exit 0 но артефакт не изменился = false green), node coverage, watchdog independence
- Tiered SLO-пороги: P0 (деньги, публичный постинг, security, master registry) — freshness ≤1.5× периода, drift max 2× периода, alert немедленно после grace; P1 (daily digests, CRM sync) — ≤1 период + 6ч grace; P2 (weekly, best-effort) — ≤2× периода; browser/GPU jobs — отдельный hub-uptime SLO вместо переноса на A

## Рекомендации / решения
- Ввести единый Job Registry с writer=VPS(A): поля id/schedule/node/capabilities/artifact_path/slo_age/idempotency/enabled; запретить два enabled=true на одном job_id; доказательство — SQL count dual-enabled = 0
- Сделать VPS(A) schedule-of-record; hub(B) и ноуты(C) — capability workers, не вторые cron-демоны; pure compute/API/nightly переносить на systemd-таймеры A, browser/GPU оставлять только на Task Scheduler B
- Ввести single-writer map для всех Syncthing master-путей: один owner-node на master, остальные read-only или append-only outbox+merge; ночной алерт на любой новый *.sync-conflict-* под masters
- Самостоятельно хостить Healthchecks (или тонкий клон) на A: пинг только после записи артефакта, не после старта процесса; watchdog для browser-джобов B должен жить на A, а не рядом с B
- Мигрировать волнами W0→W1→W2 с жёстким never-list (browser/GPU/IP-sensitive никогда на A); каждый cutover через shadow→flip→disable-old→bake 7 дней, критерий успеха волны — dual-schedule=0, freshness-SLO 7д, зафиксированное падение нагрузки хаба
- Ввести forbid-overlap + классификацию идемпотентности для каждой рутины (idempotent/at-most-once/at-least-once); для non-idempotent (посты, аутрич, деньги) — lock-файл/DB-lease, приоритет skip над double-run
- Не разворачивать Kubernetes/Nomad/Temporal ради красоты (принцип АК-47) — для 6 узлов достаточно registry + systemd + Task Scheduler + TG-алерты; Nomad-constraints (role=vps|hub) оправданы только при уже готовой ops-инфраструктуре

## Сущности
- **Люди:** —
- **Компании:** Google, HashiCorp, NVIDIA, Apache, Kubernetes, Prefect, Rundeck, SaltStack
- **Продукты/инструменты:** Syncthing, systemd timers, Windows Task Scheduler, Nomad, Kubernetes CronJob, Airflow, Temporal, Prefect, Healthchecks.io, Rundeck, Salt

## Открытые вопросы
- Готовый OSS «personal AI agent fleet scheduler for Syncthing+Telegram» не найден — нет прямого прецедента, только компонентные аналоги
- Численный порог SLO «1.5× period» — это синтез практики, а не единый индустриальный стандарт RFC
- Точный процент из 89 рутин, мигрируемых на VPS без построения полного inventory — не определён, требуется дамп реестра рутин
- Утверждение «residential IP всегда безопаснее datacenter IP для всех сайтов» — зависит от конкретного сайта/антибот-системы, не абсолютно

## Источник
- DR-ID `DR26-07-25-HUB-01-2126` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-25-HUB-01-2126-routine-placement-grok.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-25-HUB-01-2126-grok.md`

## Связано
- [[node-doctrine-thin-anchor]]
- [[vps-anchor-node]]
- [[fleet-parity-board]]
- [[one-system-propagate]]
- [[watchdog-must-verify-the-item]]
- [[machine-bus-telegram-rail]]
- [[ak47-simplicity]]
- [[decision-2026-07-31-sintez-9-dr-lokalnogo-veera]] — источник пункта №1 синтеза
