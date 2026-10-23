---
dr_id: DR26-10-11-HUB-03-0732
title: "рои агентов на одном ПК. Рынок координации агент↔агент vs наш стек claw-consensus"
date: 2026-10-11
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-10-11-HUB-03-0732): рои агентов на одном ПК, рынок vs наш стек

> Machine: хаб (Хаб) · Agent: Claude Code (Fable 5.1) · Operator: Anton · 2026-10-11
> Полный отчёт с таблицами и 30 источниками: `E:\!CLAUDE-PaloAltoPC-June26\docs\dr-agent-swarms-on-one-pc-2026-10-11.md`.
> Заказ: голосовая Антона 10.10 («какие тулзы для роёв агентов на компьютере, как другие организуют, насколько наше отличается»). Рельса web>gemini мертва (429), веб снят встроенным поиском + WebFetch + `gh api`.

## Ключевые выводы
- Рынок разошёлся на четыре слоя: встроенное у вендоров (Claude Code Agent Teams, cross-session messaging через сокет, Codex subagents без связи между собой), оркестраторы роя на одной машине (Gas Town 18k⭐ + Beads 28k⭐, Paperclip 100k⭐, Claude Squad 8.6k⭐, LoopTroop), шины для разнородных агентов (agentbus 0⭐, llm-bus 9⭐, agent-mesh 1⭐, Alook 1.3k⭐, AgentTeams/Matrix 5.7k⭐, OpenAgentMesh/NATS 7⭐) и протоколы (A2A с поглощённым ACP, MCP, ANP).
- Класс «шина для разнородных агентов» родился летом 2026: 6 из 10 проектов моложе 60 дней и < 10 звёзд; зрелые (Alook, AgentTeams) тянут сервер или облако.
- У вендоров рой = один вендор, одна машина или их облако. Горизонтали Claude↔Codex↔Cursor никто из них не даёт.
- Наш claw-consensus закрывает пустую клетку рынка: разнородные агенты + несколько машин + без сервера + тиры одобрения человека + «готово = claim с уликой». Уникально у нас: leader/follower канон (ровно «governance gap» из arXiv 2606.31498), межмашинный консенсус с тай-брейком без сервера, учёт стоимости координации.
- Где мы велосипед: нет кокпита «все сессии всех вендоров» (рынок: cmux 27k⭐, Conductor, ai-maestro), lease на файл, а не на смысл (foremerge 538⭐ ловит «один заменяет PaymentService, другой расширяет»), шина не MCP, межмашинный claim только у peer_help, витрина отстаёт (v0.4.0 без тега, локальные копии v0.2.1/v0.3.1).
- Дыры по прибору: NATS-рельса мертва (selftest 06.10), доска aibus_lite пустая (0 локов, ночник с 30.09), консенсус-лог Маяка молчит с 03.08, A2A только на входе.

## Решение
- Брать: cross-session messaging как третью рельсу внутри узла (0 инфраструктуры); intent+scope в lease по образцу foremerge; MCP-обёртка над шиной для Codex/Cursor/Gemini.
- Парк: Beads как ledger задач, только если появится рой 20+ однотипных кодеров.
- Не брать: серверные шины (NATS/Postgres/Matrix/k8s) ради 8 узлов; облачные комнаты Alook/Agent Relay для шины флота (секреты и IP наружу); Gas Town целиком (Claude-only, $100/час, ломал main).
- Коннект (§4.7): issue/PR в agentbus и agent-mesh с «ACK как долг» и «consensus без сервера», ссылка на claw-consensus.
- Гигиена: тег v0.4.0, обновить локальные копии, разбудить или пометить dormant консенсус Маяка, снять NATS с витрины.

## Связано
- 2026-09-22--agenty-v-odnoy-papke-loki-mira-i-gde-my-vperedi · 2026-09-15--patterny-mezhagentnoy-koordinacii-v-prode-chto-rabotaet-i-lomaetsya · 2026-09-10--realnye-keysy-orkestracii-desyatkov-scheduled-claude-code-sessiy-i-ant · insight-coddy-1-3-swarm-fleet-verdict-2026-10-11
- Канон: reglament-multi-machine-claude-i-peredacha-mezhdu-mashinami · reglament-trigger-03-avtonomnyy-konsensus-mashin · reglament-vozhak-vedomyy-canon-governance

## Поправка 14.10.2026: две внешние ноги (ChatGPT Deep research + claude.ai Research Fable 5.1 Max)
> Machine: хаб (Хаб) · Agent: Claude Code (Fable 5.1) · Operator: Anton · 2026-10-14 · сессия H21777 · кворум 4/6 не собран (2/6), вердикт = два вендора + инхаус.
- Сошлось: пустая клетка «пять свойств вместе» держится, но ТОНКАЯ; рельсы не новы, нова надстройка (ACK-долг, dual-rail, тиры с tripwire, канон, верификация второй машиной); первым брать MCP-обёртку над шиной, intent+scope, verify-gate; Gas Town не копируем.
- Разошлось: claim между машинами через Syncthing НЕ атомарен (арбитр-single-writer или fencing token); cross-session messaging не первый, а последний; пропущены agent-sync, system-gap-master, GNAP, ClawTeam-OpenClaw, Acgs-Swarm; 4 High в reference/consensus.py; витрина (roadmap, число машин, threat model Telegram, pinned-режим join-the-fleet); позиционирование = reference implementation governance-слоя по arXiv 2606.31498, не «пустая клетка».
- Полная сверка: `E:\!CLAUDE-PaloAltoPC-June26\docs\dr-prompts\DR26-10-11-HUB-03-0732-synthesis-2legs.md`; оригиналы ног в `_originals/deep-research/DR26-10-11-HUB-03-0732-{chatgpt,claudeai}.md`; дашборд Sheets `1RPrXHh4Be9cUmlKjDeEyObG0mNP408-iWdxk6pV_Ev4`.


## Raw model reports

3 reports delivered, synthesis used 4 of 6: [DR26-10-11-HUB-03-0732-roi-agentov-na-odnom-pk-rynok-koordinatsii-agent-agent-vs-nash-stek-cl/](DR26-10-11-HUB-03-0732-roi-agentov-na-odnom-pk-rynok-koordinatsii-agent-agent-vs-nash-stek-cl/README.md)
