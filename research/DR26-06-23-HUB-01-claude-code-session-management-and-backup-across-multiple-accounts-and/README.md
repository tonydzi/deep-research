# How DR26-06-23-HUB-01 was run

- topic: Claude Code session management across multiple accounts and machines (Выводы реализованы в текущей архитектуре флота (причинность не доказана — фиксирую совпадение с фактом, не влияние): (1) 'наивный файл-синк = репликация, а не транзакционное состояние, плодит sync-conflict' -> конфиг едет ОТДЕЛЬНЫМ git-каналом, не Syncthing (память git-channel-for-config-not-file-sync, two-channels-one-file-race: два канала на один файл = затирание); (2) 'сессии ~/.claude/projects = живое операционное состояние с retention 30д' -> бэкап конфига на каждой машине + сверка счётчиков при миграции (CLAUDE.md §7.11); (3) 'официального синка сессий между машинами/аккаунтами нет' -> кросс-машинная связь идёт текстом по шине machine_bus + TG-03, а не переносом сессий (§7.3); (4) страховка на клоббер = .stversions прежде вывода 'файла нет' (память stversions-rescue-before-declaring-missing).)
- started: 2026-06-23
- models asked: 1 · reports delivered: 0 · used in synthesis: not recorded

| model | report arrived |
|---|---|


- prompt: not preserved (this research ran before we kept a prompt registry)

Reports below are published as delivered, after an automatic privacy scrub.

- [multipl](./multipl.md)
