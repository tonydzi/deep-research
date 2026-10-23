# DR26-07-10-MACB-01 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!-- source: Grok (SuperGrok Expert) Deep Research · DR26-07-10-MACB-01 · 2026-07-11 · origin: grok -->

Thought for 2m 42s

Рекомендуемая целевая архитектура (гибридная, 3 инструмента под 3 нагрузки)

SKLAD (Obsidian-волт, ~180k мелких файлов, 6.1 ГБ):
Seafile (self-hosted на хабе-Windows) + клиенты на ноуте и macOS.
Почему: Seafile лучше справляется с большим количеством мелких файлов (дедупликация, блочное хранилище, быстрее initial sync и обновлений по отчётам 150k+ файлов). Централизованная модель (хаб = сервер) снижает P2P-проблемы (NAT/VPN/offline), даёт web-UI, versioning, libraries. Cross-platform клиенты отличные. Риск потери данных ниже при правильной настройке, чем у чистого P2P с receive-only. Для не-инженера — приемлемо (GUI + скрипты). Бесплатно self-host.⁠Forum.seafile +1

Альтернативы: tuned Syncthing (если очень хотите P2P) или Obsidian Sync (официальный, E2E, минимальные конфликты, очень простой, но платный ~$4–16/мес и облачный). Resilio — быстрее P2P, но proprietary.

КОНФИГ AI-агента («внутренний путь лаборатории», skills, CLAUDE.md, MEMORY.md и т.д.):
chezmoi (или bare git + claude-sync-like инструмент) с хаба как источника + pull/apply на followers через cron/hook.
Почему: Избегает «локальная правка молча умерла» и silent empty (как было с «внутренний путь лаборатории»). Версионирование, шаблоны под Win+Mac, секреты, скрипты apply. Стандарт для dotfiles/config-флотов. chezmoi современнее bare git (лучше multi-OS). claude-sync (GitHub-based с slash-командами в Claude Code) — готовое решение именно под Claude context sync.⁠GitHub +1

Syncthing receive-only — только как временный костыль с жёсткой дисциплиной.

РАЦИЯ («внутренний архив лаборатории»: consensus, heartbeat, задачи, approvals):
MQTT (Mosquitto брокер на хабе) или NATS (с JetStream) + Tailscale для связности.
Почему: Файловая «шина» — антипаттерн (eventual consistency, гонки на shared файлах типа Task-Backlog/heartbeat, «файл не доехал» неотличимо от «нет файла», нет ordering/acks, offline-узлы теряют события). MQTT/NATS — лёгкие, pub/sub + persistence, topics (/heartbeat/, /tasks/, /approvals/), QoS/retain/durable. Для 4 машин без DevOps-штата — AK-47 (Mosquitto + простые скрипты/питон-клиенты). Tailscale решает LAN/VPN/NAT/P2P-проблемы Syncthing. Хаб как центральный брокер.⁠Nats

Гибридная архитектура в проде: распространена (файлы — один инструмент, конфиг — git/chezmoi, события — MQTT/NATS/Redis Streams). Снижает нагрузку на каждый компонент, избегает слабых мест Syncthing. Хаб (всегда-включённый Windows + GPU) становится «мозгом» (Seafile-сервер + брокер + источник конфига). Followers — лёгкие клиенты/pull-узлы. Минимальный риск потери данных при single-writer + versioning + мониторинг.

Почему не один инструмент: один инструмент (даже tuned Syncthing) тянет все три нагрузки плохо (perf деградация, конфликты на координационных файлах, receive-only ловушки).

(б) «Чиним Syncthing» — конкретный чеклист настроек (вечер на 3–4 машины)

Бэкапы и безопасность: Полный бэкап всех папок + включить Versioning (Trash Can или Staggered) на всех папках Syncthing. Включить Tailscale (или WireGuard) на всех — решает P2P/NAT/VPN/offline.
.stignore (агрессивно на всех узлах, особенно Obsidian):
text
Copy
.git/
.obsidian/workspace*
.obsidian/cache/
.stversions/
*.tmp
.DS_Store
Thumbs.db
$RECYCLE.BIN/
desktop.ini
(плюс кэши плагинов и медиа, если не нужны везде)
Снижает сканируемые файлы/нагрузку в разы (исключает динамику, которая вызывает конфликты и лишние события).⁠Notes.davidkopp
Тюнинг папок (Advanced в GUI Syncthing):
fsWatcherEnabled: true
fsWatcherDelayS: 2–5 (вместо дефолт 10; быстрее детект, но чуть больше CPU — из 2025 практики)⁠Evoleinik
rescanIntervalS: 3600 (1 час) или выше на хабе (меньше изменений); ниже на активных узлах.
scanProgressIntervalS: -1 (экономит RAM/CPU).
copiers: 8 (или по ядрам/I-O тесту).
hashers: 8.
maxConcurrentWrites: 16.
pullerMaxPendingKiB: увеличить при хорошем канале.
GOMEMLIMIT: 4–8GiB (или 50% RAM хаба; env-переменная, снижает GC).⁠Docs.syncthing
GOGC: 100–200.
disableFsync: false (безопасность > perf).
Index DB — на быстрый SSD. На Windows — исключить папки Syncthing + vault из Windows Defender.
Receive-only правильно:
На followers — Receive Only. На хабе — Send & Receive или Send Only.
Включить «Ignore permissions» (кросс-ОС).
Скрипт-автоматизация (PowerShell/Bash + Syncthing REST API localhost:8384): проверять «out of sync» / local changes → auto «Revert local changes» или алерт в Telegram-шину. Никогда не редактировать вручную на receive-only (риск silent loss, как было с «внутренний путь лаборатории»). Для MEMORY.md — сделать read-only на followers или ACL.
Анти-конфликт стратегии:
Single-writer discipline + directory-per-writer (shared файлы редактирует только хаб; mutable state — в поддиректориях _bus/<machine-id>/).
Append-only shards где возможно (логи, outbox).
Авто-очистка: скрипт find ... -name "*.sync-conflict*" -exec mv {} _review/ \; + уведомление. Раз в неделю review.
Версионирование включено.
v2.x caution (2025–2026): Миграция на SQLite — частые проблемы (дни/недели, OOM на VACUUM, corruption, stuck, malformed DB). Много отчётов на форуме. Бэкап DB перед апгрейдом, мониторьте логи, тестыруйте на копии. Некоторые фиксы в поздних 2.0.x, но риски остаются. Если возможно — оставайтесь на стабильной v1.x или переходите на альтернативу.⁠Forum.syncthing +1
Мониторинг без самописных watchdog:
Syncthing REST API (/rest/system/status, /rest/db/status per folder) — polling скриптом (cron) на расхождения, offline-узлы, errors.
лог-мониторинг (grep ошибок) + Telegram-алерты.
Внешние: healthchecks или простой HTTP-пинг. Syncthing exporter для Prometheus при желании.

Чеклист за вечер: бэкап + Tailscale + .stignore + базовый тюнинг GUI + 1–2 скрипта реверта/очистки + тесты на копии. Откат — restore backup или pause sync.

(в) Таблица альтернатив по нагрузкам (вердикты)

SKLAD (180k мелких .md):

Seafile (self-host на хабе): Лучший баланс (perf + self-host + versioning). Высокий потолок файлов, хорошая скорость по отчётам, средний риск, средняя простота.
Tuned Syncthing: Приемлемо после тюнинга, но риски (конфликты, perf, receive-only).
Obsidian Sync: Самый простой + надёжный для Obsidian (низкий риск конфликтов), но платный + облако.
Resilio: Быстрее P2P, но proprietary.
Nextcloud: Тяжелее/медленнее для мелких файлов.
Cloud (Drive/Dropbox/iCloud): Максимальная простота для не-инженера, реал-тайм, но приватность/стоимость.
Вердикт: Seafile или Obsidian Sync.

КОНФИГ агента:

chezmoi / bare git / claude-sync: Лучше всего (версионирование, кросс-ОС, нет silent loss).
Syncthing receive-only: Рискованно (как в вашем случае).
Вердикт: chezmoi/git-based.

РАЦИЯ:

MQTT (Mosquitto) / NATS: Правильная семантика (pub/sub, persistence, acks). Легко для 4 машин.
Файловая папка: Плохо (гонки, ambiguity, eventual only).
Вердикт: MQTT/NATS + Tailscale.

(г) Топ-10 граблей коллег (реальный опыт 2024–2026)

.obsidian/workspace.json и cache вызывают шторм конфликтов/зависаний (Obsidian модифицирует при любом изменении файлов). Цитаты из r/ObsidianMD и forum.syncthing.net 2024–2025. Закрыли: .stignore на .obsidian/workspace* / cache.⁠Forum.syncthing
v2 SQLite-миграция: дни/недели, OOM, corruption, stuck (много отчётов на forum.syncthing.net 2025–2026, Unraid/TrueNAS/Docker). Закрыли: бэкап DB, мониторинг логов, тесты на копии, поздние патчи частично помогли.⁠Forum.syncthing
Receive-only: локальные правки → «out of sync» + ручной Revert; легко unnoticed loss (ваш кейс с «внутренний путь лаборатории»). Закрыли: дисциплина + авто-скрипты реверта/алертов.⁠Forum.syncthing
>100k–380k мелких файлов: очень медленные сканы/синк, высокое потребление RAM/CPU (forum + GitHub issues). Закрыли: агрессивный .stignore + тюнинг (copiers/hashers/fsWatcher/GOMEMLIMIT) + index на SSD.⁠Forum.syncthing
sync-conflict explosion на координационных файлах (реестры, backlog, heartbeat) при multi-writer. Закрыли: single-writer + directory-per-writer + append-only + авто-очистка.
P2P connectivity (LAN только, VPN/NAT глушит, offline → шина встала). Закрыли: Tailscale/WireGuard mesh или central server (Seafile).
fsWatcher delays («файл не доехал» = «файла нет»). Закрыли: fsWatcherDelayS=2–5 + периодический rescan + мониторинг.
Silent data loss (полное опустошение папки конфига). Закрыли: versioning + file count/size мониторинг + backups.
Hardcoded C:/E: пути в скриптах на кросс-ОС флоте. Закрыли: relative paths / env / chezmoi.
DB corruption / slow deletes в больших сетапах (особенно v2). Закрыли: тюнинг + бэкапы + альтернативы при необходимости.

(д) Поэтапный план миграции (минимальный риск, откат)

Фаза 0 (1–2 дня): Полные бэкапы + versioning + Tailscale + базовый тюнинг Syncthing + .stignore + мониторинг-скрипты + тесты на копиях. Откат: restore.

Фаза 1 (1 неделя): Стабилизация текущего Syncthing + strict receive-only дисциплина + авто-revert скрипты. Параллельный запуск.

Фаза 2 (1–2 недели): Перевод КОНФИГА на chezmoi/git/claude-sync (хаб = источник, followers pull). Верификация. Откат: вернуть Syncthing receive-only.

Фаза 3 (1–2 недели): Внедрение MQTT/NATS шины параллельно файловой (обновить скрипты). Переход задач/heartbeat. Откат: старая файловая шина.

Фаза 4 (тест + cutover): Пилот Seafile (или Resilio/Obsidian Sync) на подмножестве vault или параллельно. Сравнение perf/конфликтов. Полный cutover при успехе. Откат: вернуть tuned Syncthing.

Общий откат: держать старые конфиги/бэкапы 1 месяц, параллельные запуски где возможно, документировать. Минимальный риск — поэтапно, с верификацией каждого компонента.

(е) Источники (2025–2026 где возможно, уверенность)

Syncthing docs (tuning.html) — текущие рекомендации GOMEMLIMIT, fsWatcher, concurrency.
forum.syncthing.net: perf many small files (2023–2024, ongoing), receive-only (2019–2025), v2 SQLite migration stuck/OOM/corruption (много постов Sep 2025 – Feb 2026) — подтверждено многими.⁠Forum.syncthing
Reddit r/ObsidianMD + forum.obsidian.md: .obsidian conflicts, large vault issues (2024–2025) — подтверждено.
GitHub syncthing issues: perf, v2 DB (2025).
Блог evoleinik.com (Dec 2025): fsWatcherDelayS tuning.
Reddit r/selfhosted + comparisons: Seafile vs Syncthing/Nextcloud perf (2024–2026).
GitHub shaike1/claude-sync: инструмент для Claude context (2025–2026).
Разные сравнения 2025–2026 (Seafile/Resilio/Nextcloud).

Уверенность: v2 migration, .obsidian issues, perf many files, receive-only проблемы — подтверждено многими (десятки отчётов). Отдельные кейсы silent loss / конкретные миграции — единичные, но паттерн повторяется. Нет единого «убийцы» для всех граблей — гибрид + дисциплина решает лучше всего.

Это реальный engineering-ответ на основе живых кейсов, а не теории. Начинайте с бэкапов и Tailscale — сразу закроете часть болей. При необходимости могу углубить любой пункт (конкретные скрипты, Seafile setup на Windows и т.д.).