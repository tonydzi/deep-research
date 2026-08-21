---
dr_id: DR26-07-10-MACB-01
title: "Флот Syncthing: чинить тюнингом или дробить на 3 слоя (данные/конфиг/шина)"
date: 2026-07-10
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-10-MACB-01): Флот Syncthing: чинить тюнингом или дробить на 3 слоя (данные/конфиг/шина)

> Отчёт отвечает, стоит ли лечить текущий Syncthing-флот (168k+ md-файлов, Win/Mac/Linux) тюнингом или заменять/дробить его на специализированные инструменты для данных, конфига агентов и шины сообщений.

## Ключевые выводы
- In-house вердикт (Claude, 103 агента, адверсариальная верификация 3-линзовая): чинить и тюнить Syncthing, не менять — ни одна проверенная альтернатива не лучше для профиля (168k мелких md, кросс-ОС, 45 дней unattended, чинит не-инженер)
- Архитектура подтверждена: Syncthing = data plane (байты), TG-03/машинная шина = control plane (координация/ACK) — тот же паттерн, что у Tailscale и NATS
- rclone bisync и Iroh отклонены голосованием 3-0: bisync лочится при ошибке до ручного --resync (смерть для unattended), Iroh — библиотека для программистов, а не готовый продукт
- Git-based синк (по образцу LiveSync→GitHub) признан жизнеспособным резервом, не заменой; .git уже вынесен из общей папки
- Конкретный тюнинг-пакет: fsWatcherDelayS 10→1 на отправителях (детекция ~11с→~3с), пересмотр maxConcurrentWrites, апгрейд для фикса шторма конфликтов (баг v1.22), страх апгрейда на SQLite (v2.x) признан завышенным — у флота ~168k блоков против 30M в фейл-репортах на форумах
- Открыт и НЕ закрыт этим DR: лаг 20-60 минут при флапе (обрыв/сон) пира — это проблема re-establishment, а не детекции; текущие watchdog'и остаются главным лекарством, кандидат на отдельный точечный DR
- Внешние вендоры (Gemini, Grok) дают ДРУГОЙ вердикт — расщепить на 3 инструмента: Syncthing (или Seafile self-host по Grok, для >100k файлов лучше по дедупликации) для склада, chezmoi+Git для дистрибуции ~/.claude (устраняет 'тихую смерть' локальных правок в receive-only), NATS/MQTT для _machine-bus (файловая система не гарантирует FIFO-порядок, гонки на heartbeat/approvals)
- Документированный баг: миграция Syncthing v2.0.x на SQLite вызывала OOM до 3.9 ГБ при VACUUM и повреждение БД (malformed), пофикшено в v2.0.6, но риск остаётся по мнению Grok
- Gemini даёт числовой тюнинг-чеклист: GOMEMLIMIT=2GiB, databaseTuning=large, maxFolderConcurrency=1-2, maxConcurrentWrites=2, caseSensitiveFS обязателен false (даже ценой замедления скана с 3 до 45 минут) — иначе фатальная потеря данных при смене регистра на кросс-ОС флоте
- Общий анти-конфликтный паттерн у всех вендоров: single-writer discipline + append-only shards (каждый узел пишет в свой файл вместо общего MEMORY.md) + directory-per-writer — устраняет гонки записи почти полностью

## Рекомендации / решения
- Оставить Syncthing как data plane волта; внедрить пакет лечения (fsWatcherDelayS=1 на отправителях, ревизия версий флота, план апгрейда на 2.1.x после проверки, watchdog'и как были) — цена ~1 час, 0 новой инфры
- НЕ менять файловую рельсу на брокер (NATS/TG) целиком — сообщения несут указатели, байты остаются на файловой рельсе (metadata-content separation, лимит NATS payload ~1MB)
- Расщепление на 3 инструмента (chezmoi+Git для конфига, NATS/MQTT для шины), предложенное Gemini/Grok — считать кандидатом на будущее (вариант B/гибрид+), а не принятым решением; пересматривать только если флап-лаг не победить тюнингом или начнётся потеря данных несмотря на versioning
- Следующий точечный DR — исследовать конкретно лаг re-establishment 20-60 минут после сна/разрыва пира (самый ценный открытый вопрос)
- Seafile/Resilio/Obsidian Sync как замена склада — не принимать без отдельной проверки: в in-house отчёте отклонены/не протестированы, у внешних вендоров получили противоречивые вердикты

## Сущности
- **Люди:** calmh (Syncthing core dev)
- **Компании:** —
- **Продукты/инструменты:** Syncthing, rclone bisync, Iroh, Git/LiveSync, chezmoi, NATS, MQTT/Mosquitto, Seafile, Resilio Sync, Obsidian Sync, Tailscale, Prometheus/Grafana, SQLite, Redis Streams, Ansible, agentcookie, GOMEMLIMIT/databaseTuning (Syncthing config)

## Открытые вопросы
- Лаг 20-60 минут при флапе пира (re-establishment после сна/разрыва) — не решён, самый ценный кандидат на следующий DR
- Resilio/Unison/mutagen непроверены in-house — отсутствие клеймов ≠ слабость
- Поведение Syncthing 2.1.x в стабильном режиме на мелкофайловом профиле не протестировано
- Расхождение вердиктов не примирено: in-house говорит 'тюнить, не менять', внешние вендоры (Gemini/Grok) советуют дробить на 3 инструмента — какой подход даст материально лучший результат, не проверено экспериментально
- Рост флота >8 узлов как триггер пересмотра архитектуры — условие задано, но не проверено

## Источник
- DR-ID `DR26-07-10-MACB-01` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\Anton-Knowledge\05-Resources\Deep-Research\DR26-07-10-MACB-01-syncthing-alternatives-decision-memo.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DL-2026-07-14--DR26-07-10-MACB-01-gemini.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DL-2026-07-14--DR26-07-10-MACB-01-grok.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-10-MACB-01-chatgpt.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-10-MACB-01-gemini.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-10-MACB-01-grok.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-10-MACB-01-inhouse-A-fix-syncthing.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-10-MACB-01-inhouse-B-alternatives.md`
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-10-MACB-01-inhouse-C-peer-rakes.md`

## Связано
- [[machine-bus-telegram-rail]]
- [[decision-vault-sync-architecture]]
- [[sync-via-telegram-03-mandatory]]
- [[one-system-propagate]]
- [[ak47-simplicity]]
- [[deterministic-script-gotchas]]
- [[own-fleet-peer-equality]]
