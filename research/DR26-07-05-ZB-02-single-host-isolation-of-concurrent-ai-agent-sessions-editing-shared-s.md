---
dr_id: DR26-07-05-ZB-02
title: "Single-host isolation of concurrent AI-agent sessions editing shared sensitive files"
date: 2026-07-05
lang: mixed
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR26-07-05-ZB-02): изоляция параллельных сессий на ОДНОМ компе

> Постановка: как безопасно разграничить несколько одновременных AI-agent/IDE-сессий на ОДНОЙ машине с общей ФС и общим конфигом при правке одних и тех же sensitive-файлов (CLAUDE.md, MEMORY.md, ~/.claude, SQLite, secrets).
> ⚠️ Провенанс: авто-синтез хаба 05.07 вышел ПУСТЫМ (транскрипт содержал только запрос, отчёт тогда не был собран). Антон принёс полный отчёт 2026-07-14 → original восстановлен на ожидаемый реестром путь, эта версия написана вручную по полному тексту и supersedes пустую.

## Ключевые выводы

1. **Не «сильнее локи на всё», а два потока (two-lane).** Mergeable-материал (код, заметки под git) → изоляция worktree-на-сессию, слияние через git. Синглтоны (always-loaded конфиг, правила, секреты) → single-writer: читают все, пишет один через крошечный helper. Это и академическая логика (single-writer principle, lost-update), и направление всех вендоров.
2. **Индустрия сошлась на изоляции, не на ко-редактировании:** Anthropic рекомендует worktrees для параллельных Claude Code сессий; Cursor multi-agent = worktrees/remote-VM (+ shadow workspaces); Windsurf прямо предупреждает о гонке двух Cascades на одном файле; Devin = изолированные VM; OpenHands = Docker-sandbox по умолчанию. Никто не пытается сделать «безопасное одновременное редактирование одного живого файла».
3. **OS-локи на Unix/macOS только advisory** (работают, лишь пока ВСЕ кооперируются); mandatory-locking в Linux мёртв (deprecated, выпилен из 5.15+). **Windows сильнее:** `CreateFile` share-modes + `CREATE_NEW` (атомарный «создать-если-нет» = правильный lockfile без TOCTOU-гонки) + `ReplaceFile` для атомарного коммита. Но byte-range локи игнорируются memory-mapped I/O → «у нас есть лок» ≠ «никто не изменит»; брокер-хелпер надёжнее надежды, что все инструменты чтят один лок.
4. **TTL-lease без fencing-токена НЕбезопасен** (Kleppmann): зависшая сессия просыпается ПОСЛЕ истечения своего lease и пишет поверх нового держателя (split-brain-by-timeout). TTL допустим только если коммит валидирует generation/версию lease и отвергает устаревший. Без fencing простой человекочитаемый lockfile + ручной break-glass честнее «самоистекающего».
5. **Атомарная запись = temp-write + rename (POSIX) / ReplaceFile (Win):** лечит рваные полузаписи при крэше и даёт безопасный commit-примитив; от lost-update сама не спасает (нужен ownership поверх).
6. **SQLite остаётся** (не мигрировать преждевременно): WAL + `busy_timeout` + короткие транзакции + `BEGIN IMMEDIATE` на пишущих путях (берёт write-slot сразу, без mid-transaction upgrade-гонки). Postgres — только когда очередь писателей стала нормой. Наш карв-аут «у БД свой примитив, session-lease не нужен» подтверждён дословно.
7. **Редакторы (VS Code/JetBrains/vim/Emacs) не лочат жёстко** — предупреждают, перечитывают, держат recovery-артефакты (swap-файлы, local history). Копировать стоит модель «presence + предупреждение + восстановление», не kernel-enforced exclusion.
8. **Sandbox/контейнеры — только для unattended-фона** (add-if-pain), не для ежедневного интерактива. Фоновым агентам: общие корпуса read-only + свой writable scratch; прямую правку живых синглтонов фону ЗАПРЕТИТЬ — только предложения (patch/candidate), публикует один.
9. **Negative evidence:** трекеры Claude Code/LangGraph/AutoGen/CrewAI полны реальных same-host коллизий (state по workdir, гонки на `.git/config.lock`, `INVALID_CONCURRENT_GRAPH_UPDATE`, «not thread-safe», фантомные правки в чужой worktree). Если рантайм сам говорит «параллельная мутация требует структуры» — верить.

## Рекомендации / решения (проекция на наш флот)

- **Zero-infra СЕЙЧАС (усиливает реглумент):** (а) автономные сессии НЕ правят always-loaded синглтоны in-place по ходу работы — сессионные находки в per-session scratch/append-only, в синглтон — одним осознанным публикационным шагом; (б) видимый presence-реестр сессий в одной папке — наш ON AIR + `_active-sessions/` = ровно этот паттерн, DR подтверждает; (в) фоновые рутины к живым синглтонам не прикасаются — только предложения.
- **Add-if-pain (следующий этаж, когда коллизии реально укусят):** один локальный `publish`-helper для синглтонов: lease атомарно (`CREATE_NEW`/`O_EXCL`) с метаданными владельца (PID/host/время) → валидация generation (fencing!) → temp-write → `ReplaceFile` → бэкап; + ручная команда «сломать протухший lease». Наш план «O_EXCL lockfile после S3» верен, НО к TTL~15м добавить fencing-проверку на коммите — без неё TTL опасен (вывод 4).
- **Оверкилл для нас (не делать):** mandatory locking, overlay/FUSE на Windows, VM-на-каждую интерактивную сессию, тотальная миграция синглтон-сторов в server-DB.

## Сущности
- **Люди:** Anton; Martin Kleppmann (fencing tokens)
- **Инструменты:** Claude Code, Cursor (shadow workspaces), Windsurf, Devin, OpenHands, LangGraph, AutoGen, CrewAI, git worktree, SQLite (WAL/`BEGIN IMMEDIATE`), PostgreSQL (advisory locks), Win32 (`CreateFile CREATE_NEW`, `LockFileEx`, `ReplaceFile`), VS Code/JetBrains/vim/Emacs (recovery-модели), Docker, Syncthing, Obsidian

## Открытые вопросы
- Критерий «боль пришла» для publish-helper: первый реальный тихий clobber синглтона ИЛИ ≥2 коллизии/неделю на скане — что считаем триггером?
- Fencing в нашем лизинге: generation-счётчик в lease-файле vs версия/mtime целевого файла — что проще Антону чинить самому (АК-47)?
- «CrewDD» из промпта не существует — имелся в виду CrewAI (отчёт это сам отловил).

## Источник
- DR-ID `DR26-07-05-ZB-02` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-05-ZB-02-ai-agent-session-isolation-6a4add7f-chatgpt.md` (восстановлен 2026-07-14)

## Связано
- [[reglament-koordinatsiya-sessiy-pered-pravkoy-sensitive-failov]] — правило, ради которого делался DR: two-tier скан подтверждён; уточнить fencing к TTL
- [[decision-2026-07-05-onair-work-declaration-board]] — presence-реестр = подтверждённый индустрией паттерн
- [[coordinate-sessions-before-sensitive-edit]] · [[consensus-active-session-marking]] · [[one-system-propagate]] · [[verify-existing-before-proposing]] · [[ak47-simplicity]] · [[machine-bus-telegram-rail]]
