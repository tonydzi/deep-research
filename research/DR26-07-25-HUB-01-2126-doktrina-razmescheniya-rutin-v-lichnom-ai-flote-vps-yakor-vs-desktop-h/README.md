# How DR26-07-25-HUB-01-2126 was run

- topic: Размещение рутин в персональном AI-флоте: куда что селить между always-on VPS-якорем (16vCPU/32GB, 96% простой) и десктоп-хабом с GPU/Chrome/резидентным IP — доктрина размещения, волновой план миграции 89 кандидатов, анти-паттерны и метрики (втащен в очередь dr_start --adopt; потребитель: доктрина узлов v2 — куда селить 89 рутин (§7.1 CLAUDE.md)) (Собран ЛОКАЛЬНО на Mac16 через Grok Build CLI (подписка Heavy) в обход блокера трат хаба: 35 КБ, 400 строк, 19 уникальных URL, метки established/emerging + 6 честных [no source found]) (Доктрина узлов v2 (CLAUDE.md 7.1) подтверждена внешним синтезом — не меняем; взят новый элемент: карта один-master-один-владелец + ночной фейл при *.sync-conflict-*. Отвергнут K8s/Nomad/Temporal (цена>выгода на 6 узлах). Синтез: 02-Decisions/decision-2026-07-31-sintez-9-dr-lokalnogo-veera.md) (chatgpt rail collected headless via codex CLI (4 uniq URLs, 2 web searches); gemini rail closed by vendor)
- started: 2026-07-31 06:25
- models asked: 3 · reports delivered: 2 · used in synthesis: not recorded

| model | report arrived |
|---|---|
| chatgpt | 2026-08-01 07:15 |
| grok | 2026-07-31 17:55 |

- [prompt](./prompt.md): the exact question every model got
- [sources](./sources.md): 22 unique links cited by the models

Reports below are published as delivered, after an automatic privacy scrub.

- [chatgpt](./chatgpt.md)
- [grok](./grok.md)
