# How DR26-09-29-HUB-01-0839 was run

- topic: bb (get-bb) как оркестратор: кто как использует, LLM поверх bb, интерфейсы (dr_reconcile 2026-09-29: на диске 2/5 ног, в реестре стояло 'issued' — статус выправлен по факту. Тело ног: E:\Obsidian\Anton-Knowledge\_machine-bus\_dr\queue\DR26-09-29-HUB-01-0839\results) (3 ноги: grok 42KB живой и сильнейший (несёт changelog 0.39-0.44), glm 20KB живой но короче, gemini 17KB наполовину сайдбар. ДЕФЕКТ ФИДЕЛИТИ (третья такая заметка за заход): insight-DR-...-0839 ВРЁТ 'фактические ответы вендоров в этом фрагменте отсутствуют, прочитать тело результатов' — то есть пересказывает ПРОМПТ при 42KB живого grok с разделами A-I, вердиктом, цифрами и целевой схемой. Корень тот же: dr_synthesize.files_for не видит queue/results. Что отчёты реально говорят: один bb-сервер на Linux-VPS под systemd, UI ТОЛЬКО через Tailscale (критично: API bb без аутентификации, если bind не на loopback), поднять 0.41.0 -> 0.44.0; 100 Windows-рутин в Automations bb НЕ переносить — Task Scheduler и systemd timers остаются хозяевами расписания; роли агентам Claude=план и ревью, Codex=реализация в worktree; Workflows и часть API 0.43-0.44 помечены experimental; кейса 'перенесли 100 cron в bb' в открытых источниках НЕТ. Все три рельсы согласны по tailnet-only и 'рутины не переносить'. Паркуем, потому что потребителя нет: два входящих на заметку (insight-2026-09-05-bb-dva-proekta-odno-imya, insight-DR-DR26-09-04-HUB-02-2326) СТАРШЕ этого ДР и ссылаются на прежнее bb-расследование. РЕШЕНИЕ ИЗМЕНИТ: пересинтез из grok.md разделы F-H, затем решение Антона поднимать ли bb 0.41.0->0.44.0 на VPS-якоре.)
- started: 2026-09-29 08:39
- models asked: 5 · reports delivered: 3 · used in synthesis: 2 of 5

| model | report arrived |
|---|---|
| gemini | 2026-10-04 06:06 |
| glm | 2026-09-29 16:27 |
| grok | 2026-09-29 08:44 |

Reports below are published as delivered, after an automatic privacy scrub.

- [gemini](./gemini.md)
- [glm](./glm.md)
- [grok](./grok.md)
