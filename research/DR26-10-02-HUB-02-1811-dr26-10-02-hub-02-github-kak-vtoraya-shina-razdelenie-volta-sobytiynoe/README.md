# How DR26-10-02-HUB-02-1811 was run

- topic: GitHub как вторая шина синхронизации флота + разделение Vault на ядро-репо и мега-склад + событийное пробуждение сессий (5 сценариев S1-S5 внутри промпта) (dr_reconcile 2026-10-03: на диске 5/5 ног, в реестре стояло 'issued' — статус выправлен по факту. Тело ног: E:\Obsidian\Anton-Knowledge\_machine-bus\_dr\queue\DR26-10-02-HUB-02-1811\results) (⚠️ 7 файлов отчётов, ЖИВЫХ ВСЕГО ДВА — худшая мусорность за заход: grok 87KB ПОВРЕЖДЁН (текст перемешан двумя потоками, таблица вердиктов нечитаема), glm 44KB эхо промпта без ответа, chatgpt 21KB сайдбар 'New chat', claudeai 16KB и claudeai2 16KB сайдбар с cookie-баннером; живые — mistral 32KB и gemini 16KB (gemini-agy.md побайтно идентичен gemini.md, то есть одна нога посчитана дважды). Обе живые ноги говорят ОДНО: S2 (оживить git_bus как есть) убить, S4 (Issues как шина) слабо, брать гибрид S5 + ядро-репо из S3 без messages/; S5 = NATS JetStream на VPS-якоре в Tailscale, субъекты fleet.<machine>.*; ядро (канон, реестры, OnAir) в приватный GitHub-репо, Syncthing остаётся только мега-складом; mistral даёт план на 7 шагов (день 1 NATS+JetStream + шим dual-send, неделя 2 wake-демон только для urgent), gemini добавляет leaf nodes для офлайна. ДЕФЕКТ ФИДЕЛИТИ: заметка insight-DR-DR26-10-02-HUB-02-github-bus-vault-split-wake.md (02.10 10:26) честно пишет '2/6 собрано, добор glm+mistral заказан', НО mistral (12:21) и glm (11:55) приехали ПОЗЖЕ неё и пересинтеза не было — а mistral ставит S5 ПЕРВЫМ, тогда как заметка паркует S5 из-за SPOF Маяка и рекомендует S3-минус+S1. То есть единственная живая добранная нога ПРОТИВОРЕЧИТ действующему выводу, и этого никто не увидел. Заметка СИРОТА (0 входящих по имени; её ID цитируют bb-scout-and-harness-recall-2026-10-03 и insight-vault-github-dr-continuation-20261003, но без wikilink). РЕШЕНИЕ ИЗМЕНИТ: пересинтез по mistral+gemini, затем 2-недельный пилот ядро-репо со счётчиком чтений пирами (ноль чтений = убить) и замер лага доставки — именно он разводит S3-минус+S1 против S5.)
- started: 2026-10-02 18:11
- models asked: 5 · reports delivered: 7 · used in synthesis: 5 of 5

| model | report arrived |
|---|---|
| chatgpt | 2026-10-02 19:59 |
| claudeai | 2026-10-02 19:51 |
| gemini | 2026-10-02 18:24 |
| glm | 2026-10-02 19:55 |
| grok | 2026-10-02 18:21 |
| mistral | 2026-10-02 20:21 |

- [prompt](./prompt.md): the exact question every model got
- [sources](./sources.md): 97 unique links cited by the models

Reports below are published as delivered, after an automatic privacy scrub.

- [chatgpt](./chatgpt.md)
- [claudeai](./claudeai.md)
- [gemini](./gemini.md)
- [glm](./glm.md)
- [grok](./grok.md)
- [mistral](./mistral.md)
