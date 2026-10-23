# How DR26-07-29-MACANTON-01-1302 was run

- topic: Контент-заводы (content factory) на LLM-агентах: как устроены у других — писатель/редактор/дистрибьютор, окна апрува, анти-слоп гейты, хранение в Google Docs/Notion, квоты и метрики (Спека spec-content-factory-architecture правлена в 4 местах: новые §3.8 (тиры автоапрува T0/T1/T2 + плашка EU AI Act ст.50) и §3.9 (апрув привязан к sha256 снимка, после апрува LLM не касается текста); §3.1 таймер стартует от подтверждённой доставки, активность человека -> HOLD; §6 критерий выхода из тени расширен на FPR_owner<=5%, WARN<=25%, гейт до этого в shadow.) (ВТОРАЯ ВОЛНА 31.07: добран третий вендор Gemini Deep Research (Chrome, аккаунт a) — веер стал полным 3/3 независимых. Все трое независимо назвали автопубликацию по молчанию ошибкой №1. Спека дополнена: §3.8 получил замеренный аргумент про rubber-stamping (93% одобрений без проверки у Anthropic, 81% ложноотрицательных в auto-режиме, arXiv 2606.08919 инвертированная U ёмкости надзора) + механизм check-before-execute через revisions.list/lastModifyingUser; §3.9 оговорка что нативные Google Docs не отдают md5Checksum, хэш считаем сами по экспортированному Markdown. Уверенность синтеза поднята medium -> medium-high.) (ПРАВКА УКАЗАТЕЛЕЙ 15.08 (тик dr-runner, Mac16): первый путь ...-content-factories-chatgpt.md на Mac16 НЕ существует — висячий указатель, не искать. Живые тела трёх рельс приложены следом (.txt, md5 19675cae/5c8178c3/ad9e8133), скопированы из мёртвого корня $VAULT/_dr/queue (не путать с живой очередью _machine-bus/_dr/queue). 🤔 не проверено: лежат ли канон-копии .md на хабе — отсюда не видно.)
- started: 2026-07-29
- models asked: 1 · reports delivered: 3 · used in synthesis: not recorded

| model | report arrived |
|---|---|
| chatgpt | unknown |
| gemini | unknown |
| grok | unknown |

Reports below are published as delivered, after an automatic privacy scrub.

- [chatgpt](./chatgpt.md)
- [gaps](./gaps.md)
- [gemini](./gemini.md)
- [grok](./grok.md)
- [chatgpt](./chatgpt.md)
- [chatgpt](./chatgpt.md)
