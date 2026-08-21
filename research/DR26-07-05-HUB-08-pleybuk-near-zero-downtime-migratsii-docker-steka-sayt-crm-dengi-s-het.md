---
dr_id: DR26-07-05-HUB-08
title: "Плейбук near-zero-downtime миграции Docker-стека (сайт+CRM, деньги) с Hetzner Dedicated на"
date: 2026-07-05
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-05-HUB-08): Плейбук near-zero-downtime миграции Docker-стека (сайт+CRM, деньги) с Hetzner Dedicated на один Hetzner Cloud CPX62

> Как безопасно и почти без простоя перенести боевой Docker-стек (Postgres/MySQL/Redis/RabbitMQ/n8n/CRM) с выделенного сервера Hetzner на один облачный VPS CPX62, с бэкапом и быстрым откатом.

## Ключевые выводы
- Миграция на один CPX62 технически реалистична только при заведомо низкой нагрузке: shared vCPU, отсутствие bandwidth guarantee (~300-500 Mbit/s против выделенного 1 Gbit на dedicated), единая blast-radius зона — весь стек падает вместе.
- БД/очереди/кэш лучше держать на локальном NVMe CPX62, а не на Hetzner Volumes (networked block storage) — важна latency/jitter; Volumes годятся для снепшотов и бэкапов.
- PostgreSQL: лучший путь near-zero-downtime — logical replication (publish/subscribe), но она НЕ переносит DDL, sequence state и large objects — схему синкать отдельно, sequence поднимать вручную (`setval`) перед switchover, иначе duplicate key при первой записи.
- MySQL: near-zero-downtime через source/replica репликацию (`mysqldump --single-transaction` + `CHANGE REPLICATION SOURCE TO` + `START REPLICA`); events на replica обязательно выключить до switch, иначе повторное выполнение/дубли; для больших БД — MySQL Shell (`mysqlsh util.dumpInstance/loadDump`) с parallelism и resume.
- RabbitMQ нельзя переносить как volume/live-rsync datadir — restore поддерживается только на node с тем же именем (не работает с quorum queues/streams при renaming). Правильный путь — export/import definitions + blue-green deployment, federation/shovel для дренажа backlog (но это может нарушить порядок сообщений).
- Redis: если это просто кэш — не мигрировать данные, cold start; если stateful (sessions/dedupe/rate-limit) — через replica (REPLICAOF) или AOF backup, при этом копировать AOF во время rewrite опасно (нужно проверять `INFO persistence` и временно отключать auto-rewrite).
- n8n: главная ловушка — не workflows, а `N8N_ENCRYPTION_KEY` (генерируется при первом запуске, без переноса ключа credentials не расшифруются); в queue mode ключ должен совпадать на всех worker'ах; лучше держать n8n на PostgreSQL, а не SQLite, для прод с вебхуками.
- Cutover через Cloudflare: proxied (orange-cloud) записи имеют TTL Auto=300s (не меняется вручную); режим TLS — Full (strict); rollback делается за минуты просто возвратом old origin IP в те же A/AAAA записи, старый сервер держать hot 7-14 дней.
- Безопасность единого сервера: наружу открыты только 22/80/443, все БД/очереди — только internal Docker network или bind на 127.0.0.1; секреты через Docker Compose `secrets` (не в env/образах/Git); UFW (сначала allow SSH, потом enable) + Fail2ban с `banaction=ufw`.
- Backup-стратегия — 3 слоя (логические дампы БД + файловые бэкапы конфигов/uploads/RabbitMQ-definitions/n8n + offsite encrypted snapshots через restic/borg на Hetzner Storage Box или Backblaze B2), обязательно 3-2-1 и тестовый restore (`restic check --read-data-subset`).

## Рекомендации / решения
- Переносить на один сервер можно, только если CRM/site/bots/n8n используют заметно меньше половины CPU без тяжёлых write-spikes; иначе минимально разделить на `prod-core` и `n8n/dev`.
- Держать steady-state RAM не выше 22-24 GB из 32 GB, оставляя запас под page cache и всплески.
- Делать cutover по единому плейбуку: инвентарь → зелёный стек без трафика → initial sync (repl./dump) → прогрев и проверка → freeze writes в 'час X' → catch-up → переключение DNS/origin → снятие read-only → мониторинг 30-60 минут.
- Перед switchover прогонять проверку целостности: row counts, MAX(id)/MAX(updated_at), денежные агрегаты (SUM(amount) по payments), RabbitMQ backlog, sha256 файлов.
- Не удалять старый сервер минимум 7-14 дней после миграции, держать в hot standby для мгновенного отката.
- Настроить минимальный мониторинг после миграции: Uptime Kuma (внешние health-check), Netdata (host/container telemetry), Healthchecks.io (dead man's switch для бэкапов/cron), n8n `/metrics`.
- Использовать Traefik ACME DNS-01 challenge через Cloudflare API для выпуска сертификатов до cutover (не TLS-ALPN-01, требующий доступности 443).

## Сущности
- **Люди:** —
- **Компании:** Hetzner, Cloudflare, Backblaze
- **Продукты/инструменты:** Docker, Docker Compose, Traefik, PostgreSQL, MySQL, Redis, RabbitMQ, n8n, UFW, Fail2ban, restic, BorgBackup, Hetzner Storage Box, Backblaze B2, Uptime Kuma, Netdata, Healthchecks.io, MySQL Shell (mysqlsh), Let's Encrypt, Cloudflare Origin CA

## Открытые вопросы
- Точные версии PostgreSQL/MySQL/RabbitMQ/Redis/n8n не указаны — влияют на выбор пути миграции
- Включен ли GTID в MySQL (влияет на способ запуска репликации)
- Какие типы очередей в RabbitMQ используются (classic/quorum/streams) — критично для выбора blue-green vs restore
- Redis используется как чистый кэш или как stateful-хранилище (sessions/dedupe/rate-limit)
- n8n сейчас на SQLite или уже на PostgreSQL
- Насколько допустим короткий maintenance-window на write-путях бизнеса
- Какой план Cloudflare и какой тип TLS challenge используется в Traefik

## Источник
- DR-ID `DR26-07-05-HUB-08` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\2026-07-05-DR26-07-05-HUB-08-hetzner-migration-playbook.md`

## Связано
- [[hetzner-migration-playbook]]
- [[docker-compose-production]]
- [[zero-downtime-deployment]]
- [[postgresql-logical-replication]]
- [[rabbitmq-blue-green]]
- [[n8n-encryption-key]]
- [[cloudflare-origin-cutover]]
- [[backup-3-2-1-strategy]]
