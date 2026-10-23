# How DR26-06-24-HUB-01 was run

- topic: Claude Desktop Code Recents на Windows: где живёт список сессий (claude-code-sessions + IndexedDB), как чистить безопасно (вне реестра; найден в Downloads, ретро-регистрация 2026-07-06) (ЗАКРЫТО 01.08 суждением: модель хранения из отчёта (%APPDATA%\Claude\claude-code-sessions\<account>\<org>\*.json как локальный реестр сессий + claude.ai/epitaxy как рендер + IndexedDB как кэш) реально внедрена — по ней читают session_slot.py и visible_rail_guard.py (glob claude-code-sessions/*/*/scheduled-tasks.json), и на ней стоит память [[claude-desktop-sessions-per-account]] на 2 узлах. Практический вывод отчёта тоже принят: per-session permanent delete в десктопе нет, чистка = только claude.ai/code, поэтому боремся не удалением, а профилактикой имён (auto_session_name.py) и запретом чёрных запусков (CLAUDE.md §9.2))
- started: 2026-06-24
- models asked: 1 · reports delivered: 1 · used in synthesis: not recorded

| model | report arrived |
|---|---|
| chatgpt | unknown |

Reports below are published as delivered, after an automatic privacy scrub.

- [chatgpt](./chatgpt.md)
