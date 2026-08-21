---
dr_id: DR26-07-07-MACB-03
title: "Альтернативы Syncthing для мульти-машинной AI-флотилии: чинить или менять файловую рельсу"
date: 2026-07-07
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-07-MACB-03): Альтернативы Syncthing для мульти-машинной AI-флотилии: чинить или менять файловую рельсу

> In-house deep-research (103 агента, адверсариальная верификация) отвечает, стоит ли менять Syncthing как файловую рельсу флота Антона на альтернативу, и подтверждает разделение control plane (TG-03/шина) vs data plane (Syncthing).

## Ключевые выводы
- Вердикт: чинить и тюнить Syncthing, не менять — ни одна проверенная альтернатива не лучше для профиля (168k мелких md, Win+Mac+Linux, 45 дней unattended, чинит не-технарь)
- Архитектура подтверждена индустрией: Syncthing = data plane (байты), TG-03/шина = control plane (координация, ACK, указатели) — тот же паттерн, что у Tailscale и NATS
- rclone bisync проигрывает: латентность = крон, lockout при ошибке требует ручного --resync — смерть для unattended-режима
- Iroh — библиотека для программистов, не готовый продукт; синк пришлось бы писать самим — провал принципа АК-47
- git-based подход (LiveSync→GitHub) жизнеспособен, но нишевый; .git у флота уже вынесен из шары — резерв, не замена
- Resilio/Unison/mutagen/Nextcloud/NFS/Tailscale Drive — про-клеймов в пользу них не выжило верификацию (карта неполная, не приговор)
- Страх апгрейда на Syncthing 2.x (SQLite) для флота преувеличен: узкое место — число блоков, у флота ~168k блоков против 30M в фейл-репортах
- Лаг 20-60 минут при флапе (re-establishment после сна/разрыва пира) этим DR НЕ закрыт — отдельная проблема от детекции изменений, лечится watchdog'ами
- Убиты верификацией (0-3): «просто выключи restartOnWakeup» и «SSD+64-bit — главный рычаг»; замена файловой рельсы на брокер NATS/TG признана антипаттерном (лимит payload NATS ~1MB, метаданные и байты должны разделяться)

## Рекомендации / решения
- Вариант A (рекомендован): оставить Syncthing + пакет лечения — fsWatcherDelayS 10→1 на отправителях, апгрейд для решения конфликт-штормов, поднять maxConcurrentWrites (замерить под диск), проверить 2.1.x перед апгрейдом, watchdog'и оставить как есть; цена ~1 час работы, 0 новой инфры
- Вариант B (по триггеру): если флап-лаг не победить тюнингом — точечный DR по re-establishment, затем рассмотреть NATS JetStream на Маяке как control plane
- Вариант C (смена рельсы) — не рекомендован; вернуться к нему только при повторяемой потере данных несмотря на versioning или нерешаемом флапе
- Пересмотреть решение при: повторяемой потере данных при живом versioning, нерешённом флап-лаге после DR №3, доказанном преимуществе Resilio/Unison, росте флота свыше 8 узлов

## Сущности
- **Люди:** Anton
- **Компании:** —
- **Продукты/инструменты:** Syncthing, rclone bisync, Iroh, git/GitHub (Obsidian LiveSync), Resilio, Unison, mutagen, Nextcloud, NFS, Tailscale Drive, NATS JetStream, Telegram-03 (машинная шина)

## Открытые вопросы
- Resilio/Unison/mutagen не проверены живым тестом — отсутствие про-клеймов не равно доказанной слабости
- Steady-state поведение Syncthing 2.1.x на мелкофайловом профиле (168k md) ещё не проверено на практике
- Флап-лаг 20-60 минут (re-establishment после сна/разрыва пира) остаётся нерешённым — самый ценный кандидат на отдельный точечный DR

## Источник
- DR-ID `DR26-07-07-MACB-03` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\Anton-Knowledge\05-Resources\Deep-Research\DR26-07-10-MACB-01-syncthing-alternatives-decision-memo.md`

## Связано
- [[machine-bus-telegram-rail]]
- [[one-system-propagate]]
- [[decision-vault-sync-architecture]]
- [[control-plane-vs-data-plane]]
- [[sync-self-heal-layers]]
- [[ak47-simplicity]]
