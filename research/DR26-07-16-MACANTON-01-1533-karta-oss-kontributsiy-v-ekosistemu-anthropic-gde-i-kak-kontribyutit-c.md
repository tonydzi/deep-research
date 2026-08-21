---
dr_id: DR26-07-16-MACANTON-01-1533
title: "Карта OSS-контрибуций в экосистему Anthropic — где и как контрибьютить, чтобы заметили"
date: 2026-07-21
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Карта OSS-контрибуций в экосистему Anthropic (синтез 3 вендоров DR)

Контекст: у Антона уже есть PR #784 в `anthropics/claude-cookbooks` (multi-agent consensus & verification). Цель №2 — оффер от Anthropic/LLM-компании в DevRel. Вопрос: куда и как контрибьютить дальше, чтобы заметили.

## TL;DR

Все три вендора сходятся на одном ядре: **не «россыпь мелких PR по всем репо», а узкая лестница из 1-2 флагманских контрибуций** в официальные обучающие поверхности Anthropic (`claude-cookbooks` → `skills` → `claude-agent-sdk-demos`), которые превращают уже существующие production-ассеты Антона (консенсус, adversarial verification, tier-gating к человеку, память, evals) в именованные, переиспользуемые артефакты. Anthropic сама официально пишет на careers-странице: independent research + OSS-контрибуции «ставьте в самый верх резюме». Следующий ход: довести #784 до мержа, затем второй cookbook под **уже открытый issue** (кандидаты: #682 citation-faithfulness evals, #619 authority-routing/tier-gating, #715/#716 memory), потом один скилл или Python-демо в Agent SDK на той же архитектуре. AI-код не проблема — проблема невалидированный слоп; в MCP-репо дисклоз AI-помощи с июня 2026 **обязателен** (AI_POLICY.md). Ещё один конкретный крюк: программа **Claude for Open Source** (расширена 07.07.2026) — официальный сигнал, что Anthropic трекает builder-impact по GitHub.

## Ключевые выводы

1. **Cookbooks = поверхность №1 для заметности.** Внешние PR реально мержатся быстро (примеры мая-июля 2026: #677, #696, #754, #771, #772; от same-day до ~4 дней), ревьюят живые люди (`PedramNavid`, `maheshmurag`, `cj-ant`, `rlancemartin`) + Claude AI Review; внешних авторов кредитуют по имени на официальной docs-странице. [established, 3/3 вендора]
2. **Открытые issues кукбука — готовые мишени под ассеты Антона:** #682 (citation-faithfulness evals), #619 (ADVISE/EXECUTE/DEFER/STOP = tier-gating к человеку), #715/#716/#596 (память, компактное состояние, shared context), #721 (pipeline vs barrier в мульти-агентах). Это не абстрактный спрос, а конкретный backlog. [established, source: ChatGPT (самый заземлённый); тематически подтверждено Grok]
3. **Issue-first и селективность в `skills`:** мержатся хирургические фиксы и точечные скиллы; proposal-style «изобрёл новую парадигму» закрывают. Для новых скиллов — сперва issue + шаблон. Близкий фит: issue #1385 (Reasoning Quality Gate Pipeline — почти дословно архитектура Антона), #1329 (compact-memory). [established]
4. **Флагман > объём.** Единый вывод всех трёх: один именованный, production-обоснованный, обучающий артефакт внутри официальной поверхности даёт больше, чем десяток мелких PR. Anthropic careers прямо ценит independent research/OSS как hiring-сигнал; задокументированных кейсов «hired за один PR» нет [no source found — все 3 вендора]. [established]
5. **AI-код — ок, слоп — нет; дисклоз = ownership.** Формула: «Архитектура, production-паттерн и валидация мои; имплементация с помощью Claude, вручную отревьюена и протестирована». ⚠️ В MCP-org с ~июня 2026 действует **AI_POLICY.md — обязательный дисклоз AI-использования** в PR/issue; недисклоз может вести к закрытию. [established для MCP-политики (source: Grok); emerging для формулировки]
6. **MCP: не «ещё один коннектор».** Управление ушло в Agentic AI Foundation (Linux Foundation, донейшн 09.12.2025), но технически рулят прежние мейнтейнеры (David Soria Parra, Justin Spahr-Summers) через SEPs/Working Groups. Публикация сервера — НЕ через PR в registry (только `mcp-publisher` CLI; PR в `data/seed.json` закрывают как invalid). Серверов уже 10 000+ — выделяет только решение экосистемной боли: audit trails/observability, enterprise auth, approval-routing, Python-publishing friction (registry issue #1157). [established]
7. **Issue creation закрыт** в ключевых репо (`claude-cookbooks`, `skills`, `claude-agent-sdk-demos`, `claude-code`) → стратегия = целиться в уже открытые issues или тщательно скоупленные PR под видимый backlog. [established, single-source: ChatGPT]
8. **Claude for Open Source program (расширена 07.07.2026):** бесплатные 6 мес Claude Max 20x мейнтейнерам high-impact репо / 100+ merged PR за 12 мес — официальное подтверждение, что Anthropic системно трекает GitHub-impact. Потенциальный отдельный крюк заметности. [emerging, single-source: Grok]
9. **`claude-agent-sdk-demos` issue #63 просит Python-примеры** (демки TypeScript-heavy) — дешёвая и легитимная точка входа для Python-демо верификационного агента. [established, single-source: ChatGPT]
10. **`claude-code-action` мержит внешние мелкие фиксы пачками** (9 PR за 13-16 июля 2026) — быстрый «кредибилити-филлер», но слабо связан с уникальными ассетами. [established, single-source: ChatGPT]

## Где вендоры расходятся

| Вопрос | ChatGPT (Pro DR, самый заземлённый) | Grok (74 источника) | Gemini (⚠️ DEGRADED: без live web, вес ниже) |
|---|---|---|---|
| Главная рекомендация | «Narrow Ladder»: флагман-cookbook (#682 verification/citation-faithfulness) → Python SDK demo или скилл | «Balanced»: #784 до мержа → 2-3 скилла → один standout MCP server | Cookbook: Human-in-the-Loop tier-gating recipe |
| Telegram/WhatsApp MCP-коннектор | ⛔ Не ранжирует: рынок насыщен (10k+ серверов), выделяет только auditability/approvals | ✅ В топ-4: fleet-scale communications как underserved | ✅ В топ-4 flagship |
| MCP governance | Two-layer: AAIF = зонтик, техрешения у мейнтейнеров | То же + детали (SEPs, WG Skills Over MCP) | Устарело: «Anthropic stewards the spec» |
| Дисклоз AI | Нет формального требования, дисклозить ownership добровольно | **AI_POLICY.md = обязательный дисклоз в MCP-org** | «Стигмы нет, ожидается» |
| Что мерится в cookbooks | Конкретные issue-номера + именованные ревьюеры | Общие категории + CI-детали (uv, conventional commits) | Структурные эвристики без номеров |
| Волюм PR имеет значение? | Нет, только именованный артефакт | Частично да (программа OSS считает 100+ PR) | Нет |

Разрешение конфликта по мессенджер-коннектору: ChatGPT аргументирует данными о насыщении — принимаем его версию как базу; версия Grok/Gemini живёт только если коннектор демонстрирует **качественно новое** (approval-routing, audit, multi-machine ops), т.е. это тот же вывод «решай экосистемную боль». Gemini глубоко не заземлён (сам признал отсутствие live web) — его данные используем только там, где совпадают с двумя другими.

## Приоритетный план действий для Антона

1. **Довести PR #784 до мержа** (`anthropics/claude-cookbooks`) — следить за ревью, быстро отвечать, конформить CI (nbconvert, ruff, registry.yaml). Всё остальное компаундится от этого якоря. [Grok #1; effort low]
2. **Флагман-cookbook №2 под открытый issue #682** — citation-faithfulness evals + adversarial verification harness (прямой запрос мейнтейнеров от 29.05.2026, идеальный фит с консенсус-ассетом). Альтернативный/парный кандидат: **#619** authority-routing ADVISE/EXECUTE/DEFER/STOP = наш tier-gating в чистом виде. [ChatGPT top-1 и top-3; merge prob high]
3. **Python-демо в `claude-agent-sdk-demos` под issue #63** — verification-heavy агент на Python (репо просит именно Python-примеры; низкая конкуренция, высокая легитимность). [ChatGPT #5]
4. **Один скилл в `anthropics/skills`, issue-first:** целиться в #1385 (Reasoning Quality Gate — почти дословно наша архитектура) или #1329 (compact-memory). Не proposal-стиль, а хирургически скоупленный скилл + README. [ChatGPT #6-7, Grok #3]
5. **Memory-cookbook** под #715/#716/#596 (context-reset-safe память, компактное состояние) — вторая флагманская волна, тема сейчас в центре публичного фрейминга Anthropic. [ChatGPT #2]
6. **MCP — только экосистемная боль, не коннектор:** registry issue #1157 (Python/PyPI publishing path) или reference-сервер observability/audit-trail/approval-routing. Публикация серверов — через `mcp-publisher` CLI, никогда PR в seed.json. При любом PR в MCP-org — обязательный AI-дисклоз (AI_POLICY.md). [ChatGPT #9-10]
7. **Фоновые дешёвые ходы:** мелкие фиксы в `claude-code-action` (мержат быстро) + участие в MCP WG «Skills Over MCP» с нашими consensus-паттернами (прямой контакт с core-мейнтейнерами). Проверить eligibility под **Claude for Open Source**. [Grok #7, #9]
8. **Каждый мерж → соцнарратив** («всё = контент»): пост с архитектурным write-up, CTA инженеру-тестеру; код = доказательство, write-up = маркетинг. Дисклоз-формула в каждом PR: «Design, production pattern and validation are mine; implementation drafted with Claude, manually reviewed and tested».

Чего НЕ делать: не PR-ить commodity Telegram/WhatsApp коннектор «как есть»; не лезть в generated-code SDK-репо; не слать proposal-style скиллы без issue; не открывать surprise-PR в MCP registry ради публикации сервера.

## Источник

Синтез трёх Deep Research отчётов (fan-out 16.07.2026, собрано 21.07.2026):

- DR26-07-16-MACANTON-01-1533-anthropic-oss-map-chatgpt — «внутренний путь лаборатории» (ChatGPT Pro DR; самый заземлённый: конкретные PR/issue-номера, ревьюеры)
- DR26-07-16-MACANTON-01-1533-anthropic-oss-map-gemini — «внутренний путь лаборатории» (⚠️ DEGRADED: Flash-Lite без live web; только структурная аналитика)
- DR26-07-16-MACANTON-01-1533-anthropic-oss-map-grok — «внутренний путь лаборатории» (SuperGrok Expert, 74 источника; уникальные находки: AI_POLICY.md, Claude for Open Source program)

Реестр: _DR-Registry

---

## 🧒 Простыми словами

Мы спросили три умных робота: «куда Антону писать код, чтобы его заметили в Anthropic?» Все три сказали одно: не разбрасывай много маленьких кусочков, а построй одну-две большие красивые башни там, где Anthropic сама учит людей (книга рецептов cookbooks). У них уже есть список «хотим вот это» — и там просят ровно то, что у Антона уже готово и работает. Сначала доводим наш первый кирпич (PR #784) до конца, потом ставим второй — про проверку, что агент не врёт. И честно пишем: «придумал человек, печатал Claude, человек всё проверил» — за это хвалят, а за неряшливость выгоняют.

## Связано
- insight-DR-DR26-07-27-MACANTON-01-0018-мультиканальный-холодный-аутрич-к-devrel-исследова — обе согласны: GitHub PR/issue-comment идёт ПЕРЕД холодным касанием DevRel — сирота даёт каденцию после этого шага
