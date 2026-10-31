# How DR26-09-26-HUB-01-2230 was run

- topic: Jev (TypeSafe AI System One) — переносить ли решающие точки флота на типизированную decision-модель (dr_reconcile 2026-09-27: на диске 3/5 ног, в реестре стояло 'issued' — статус выправлен по факту. Тело ног: E:\Obsidian\Anton-Knowledge\_machine-bus\_dr\queue\DR26-09-26-HUB-01-2230\results) (добор 28.09 Mac16: gemini ⛔ (AutoFF разлогинен в Google, Deep Research в меню нет), chatgpt браузер ⛔ (webdriver read-timeout x2), codex CLI ⛔ (refresh token revoked), claude CLI ⛔ (политический бан 22.09). Кворум остался 3/6 — рельсовый потолок узла, добора нет.) (Решение жило и до разбора: 02-Decisions/decision-jev-typesafe-rail-2026-09-26.md (01.10) + дверь jev_ask.py; 4 живых ноги (grok 70KB, glm 44KB, mistral 39KB, gemini 25KB), консенсус 'внедрять точечно, shadow, confidence-gated routing'. Внесено В САМ ФАЙЛ решения (§2-квинт): метка ПЕРЕСОБРАТЬ — решение от 01.10 собрано по 3 ногам от 26.09, gemini приехал опоздуном 03.10 и даёт ПРОТИВОПОЛОЖНЫЙ вердикт по точке 'гейт отказа ДР' (оставить LLM vs брать в тень первой); плюс цифры grok не в пользу Jev (фишинг 62.6% против Haiku 81.3%, ECE 0.154 vs 0.097; на своём JevBench 86.6%/0.032 — то есть на чужих бенчах хуже) и независимой публикации ECE от вендора нет = калибровка остаётся claim вендора. ДЕФЕКТ: авто-заметка insight-DR-...-2230 врёт 'синтез не включён, есть только промпт' при 4 живых ногах — в ремонт.)
- started: 2026-09-26 22:30
- models asked: 5 · reports delivered: 4 · used in synthesis: 3 of 5

| model | report arrived |
|---|---|
| gemini | 2026-10-03 23:20 |
| glm | 2026-09-26 22:48 |
| grok | 2026-09-26 22:38 |
| mistral | 2026-09-26 22:48 |

- [prompt](./prompt.md): the exact question every model got
- [sources](./sources.md): 51 unique links cited by the models

Reports below are published as delivered, after an automatic privacy scrub.

- [gemini](./gemini.md)
- [glm](./glm.md)
- [grok](./grok.md)
- [mistral](./mistral.md)
