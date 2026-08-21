---
dr_id: DR26-07-16-HUB-02
title: "Split-brain hardening для roaming-leader arbiter поверх Syncthing (4-node personal fleet)"
date: 2026-07-16
lang: mixed
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-16-HUB-02): Split-brain hardening для roaming-leader arbiter поверх Syncthing (4-node personal fleet)

> Исследование ищет минимальный, ремонтопригодный набор механизмов (epoch/fencing + partition detection + safe-mode + опционально Telegram-beacon), который защищает roaming-arbiter Антона от split-brain, когда presence stamps и ledger идут по одному eventually-consistent Syncthing-каналу.

## Ключевые выводы
- Presence stamps, реплицируемые по тому же Syncthing-каналу, что и ledger, НЕ являются независимым failure detector (Chandra-Toueg) — это и есть корневая причина, по которой текущее правило S9 roaming-arbiter может расщепиться (chatgpt, established).
- Лучший детектор partition — локальный Syncthing REST /rest/system/connections: он спрашивает собственный демон узла о состоянии соединений и НЕ проходит через сам partition, поэтому не зависит от того же канала, что и ledger (оба вендора сошлись, established, топ signal/noise).
- Lease+fencing token (epoch, аналог Chubby sequencer) останавливает 'зомби'-действия устаревшего арбитра после rejoin, но НЕ предотвращает одновременное действие двух живых арбитров во время самого partition, если epoch выпускается каждым узлом самостоятельно (established, оба вендора).
- Raft-термины без majority-vote RPC и свежих quorum-чтений — это 'fence labels, а не consensus': полезны для аудита и отклонения устаревших записей, но не дают теоремы safety об уникальном лидере (chatgpt, established).
- Witness/quorum-device паттерн работает только если witness достижим по НЕЗАВИСИМОМУ пути, не через тот же Syncthing/replicated storage — Microsoft прямо предупреждает не размещать file-share witness на реплицируемом хранилище (established принцип, но конкретная формулировка single-source).
- Safe-mode: оба вендора сходятся на комбинации 'freeze auto-arbitration + эскалация человеку' как дефолт при подозрении на partition, epoch-fencing включён всегда, узкий белый список идемпотентных/обратимых действий — опционально (= Option B, рекомендация обоих отчётов).
- Reconciliation: детерминированный total order по ts+machine-id — плохое правило слияния (стирает легитимные конкурентные решения); лучше — хранить все события, детектировать конфликт через vector-clock/`seen_seq` причинный снимок, и отправлять в очередь человеку ТОЛЬКО конфликтующие терминальные COMMIT (chatgpt детально расписал json-схему seen_seq и conflict()-предикат; grok — похожий вывод 'epoch-wins + human queue').
- Telegram как боковой канал — обоснованное усиление ТОЛЬКО как независимый liveness/epoch-beacon с протоколом claim+challenge+wait (signed claim, unchallenged окно 2-5 минут), а не как единственный источник истины; есть технические ловушки: Bot API хранит апдейты не дольше 24ч, боты в группах по умолчанию в privacy mode и не видят обычные сообщения без настройки (chatgpt, established/single-source по Telegram докам).
- Рекомендуемый размер импланта — ~300-500 LOC поверх текущего ~1500-строчного single-file Python-движка; явно НЕ строить на этом масштабе: полный Raft/BFT (нет доверенной сети RPC, спящие узлы ломают динамическое membership), сложный dynamic quorum voting, тяжёлые CRDT-библиотеки — для доверенного личного флота из 4 узлов без Byzantine-акторов это оверинжиниринг (оба вендора).

## Рекомендации / решения
- Внедрять в порядке: (1) монотонный epoch/fencing на каждом arbiter-claim и решении (COMMIT/tie-break), peers отклоняют более старые epoch навсегда после появления более высокого; (2) детекция partition через локальный Syncthing connection-state к always-on якорям (hub, Mayak) + ledger-divergence → вход в safe-mode (freeze + TG-пинг Антону) при подозрении; (3) опционально — Telegram signed claim+challenge beacon как независимый корроборирующий путь.
- Спящие узлы (HP17, Mac16) не считать авторитетными для вывода о доступности флота — их отсутствие в connections не сигнал partition.
- Reconciliation держать через human-review-queue только конфликтующих терминальных COMMIT (обнаруженных по causal seen_seq), не через слепой epoch-wins или ts-ordering по всей истории.
- Не строить полный Raft/BFT, сложный dynamic quorum voting, тяжёлые CRDT-библиотеки на этом масштабе — избыточная сложность и хрупкость относительно 1500-строчного репо, чинимого одним нетехническим владельцем.

## Сущности
- **Люди:** Anton, Martin Kleppmann, Chandra and Toueg
- **Компании:** Google (Chubby), Microsoft (Windows Server quorum witness), HashiCorp (Consul), Red Hat (corosync-qdevice), Gluster, StarWind, Amazon (Dynamo)
- **Продукты/инструменты:** Syncthing, Telegram, Raft, Chubby, ZooKeeper, etcd, Kubernetes Leases, Bayou, Dynamo, CRDT, corosync-qdevice

## Открытые вопросы
- Отчёт ChatGPT обрезан на середине описания Option B (Fence-and-freeze with Telegram witness) — полная оценка LOC и деталей Option B/C не захвачена в этом файле.
- Секция Gemini (собрана 16.07 ~23:00, 34.7K immersive-panel) в этом фрагменте отсутствует — нужна для полного синтеза.
- Точная длина challenge-окна (Y минут) для Telegram claim-протокола — контекстная оценка без источника, не решена.
- Классификатор exclusive_effects() для conflict-предиката при reconciliation — доменно-специфичен, источника нет, нужно спроектировать самим.
- Не решено, нужен ли Telegram-боту отключённый privacy mode/admin-права или полноценный TDLib-клиент для надёжного наблюдения claim-сообщений спящими узлами.
- Финальный выбор (Option A/B/C) и волны внедрения S10 ждут решения Антона ('+').

## Источник
- DR-ID `DR26-07-16-HUB-02` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- roaming-arbiter
- consensus.py
- machine-bus-telegram-rail
- one-system-propagate
- multi-machine-auto-consensus
- own-fleet-peer-equality
- vps-anchor-node
- fencing-tokens
- insight-DR-DR26-07-17-HUB-02-prior-art-защита-4-узлового-консенсуса-от-split-br — twin DR report on the identical split-brain/fencing/epoch topic for the same fleet
