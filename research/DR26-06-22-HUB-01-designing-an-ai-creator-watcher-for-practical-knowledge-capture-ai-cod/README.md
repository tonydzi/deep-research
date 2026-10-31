# How DR26-06-22-HUB-01 was run

- topic: Designing an AI Creator Watcher for Practical Knowledge Capture (Ядро реализовано, часть — нет. ✅ Взяли и живёт: двухпроходный конвейер 'дешёвый детерминированный детектор -> дорогой LLM-судья только по выжившим' = скилл /mine-channel + /alpha-judge + /watch-channel (та же формула поднята в CLAUDE.md §6.1 как лестница дешёвых инструментов); курируемый тир-A/B/C watchlist; политика 'только публичные каналы, Telethon, минимум сырого хранения'. ⛔ НЕ реализовано и осталось на будущее: knowledge-units как первичный объект вместо заметок, экспоненциальный распад свежести (half_life_days 30-730, effective_utility), числовая формула alpha_score с порогами 0.40/0.60/0.75, трёхслойный дедуп (exact + MinHash + семантика e5 0.89-0.93), SponsorBlock-очистка рекламы — проверено grep по скиллам 01.08.2026: маркеров alpha_score/half_life/freshness в коде нет. Вернуться к этому, когда майнинг каналов пойдёт потоком и начнёт плодить дубли-репосты через площадки.)
- started: 2026-06-22
- models asked: 1 · reports delivered: 0 · used in synthesis: not recorded

| model | report arrived |
|---|---|


- prompt: not preserved (this research ran before we kept a prompt registry)

Reports below are published as delivered, after an automatic privacy scrub.

- [practical](./practical.md)
