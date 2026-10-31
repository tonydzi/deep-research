# How DR26-07-14-ZB-01 was run

- topic: Agent-bus архитектура: Telegram+Syncthing vs выделенный транспорт (Matrix/NATS/MQTT) при росте флота (Закрыт замером, а не отчётом: гипотеза «нужен выделенный транспорт (Matrix/NATS/MQTT), потому что Telegram медленный» ОПРОВЕРГНУТА. Провод TG 176 мс RTT, Syncthing p50 1.35 с; сквозное p50 658 с целиком объяснялось нашим опросом PT20M и отсутствием квитанции у робота. Применено 29.07 вместо новой рельсы: быстрая полоса квитанций inbox-ack-fast (2 мин, 0 LLM), гейт формата ACK в bus_send + PreToolUse-хук, один #id на обеих рельсах (раньше TG-копия шла без id и спарить квитанцию было нечем). Живой прогон: 38 с против 11 мин. Пакет ack-fastlane-20260729 раскатан tier-1 на 6 узлов. Строить SSE/NATS-рельс основания нет; вернуться к вопросу только если после раската покрытие квитанций не поднимется.)
- started: 2026-07-14
- models asked: 1 · reports delivered: 3 · used in synthesis: not recorded

| model | report arrived |
|---|---|
| chatgpt | unknown |
| gemini | unknown |
| grok | unknown |

- prompt: not preserved (this research ran before we kept a prompt registry)

Reports below are published as delivered, after an automatic privacy scrub.

- [chatgpt](./chatgpt.md)
- [gemini](./gemini.md)
- [grok](./grok.md)
