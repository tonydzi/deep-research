---
dr_id: DR26-11-17-MAYAK-04
title: "Каталоги MCP-серверов и скиллов: типы, бизнес-модели, трафик, входы (ноябрь 2026)"
date: 2026-11-17
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-11-17-MAYAK-04): Каталоги MCP-серверов и скиллов: типы, бизнес-модели, трафик, входы (ноябрь 2026)

> Какие каталоги MCP-серверов и скиллов для агентов существуют в ноябре 2026, как они зарабатывают, какой у них реальный трафик и как бесплатному OSS-дистрибьютору попасть в них с наибольшей отдачей.

## Ключевые выводы
- Официальный MCP Registry бесплатен и без ревью (CLI mcp-publisher, проверенный namespace, server.json). Из него тянут данные Glama, PulseMCP, mcp.directory, а GitHub MCP Registry подхватывает после ручного онбординга. Список в нём ≠ видимость: записей 11,8k (март 2026) → 58 089 (7 ноября) → 62 678 (15 ноября), около 23–25% удалённых эндпоинтов нерабочие.
- Эра прокси закончилась: Smithery куплен Arcade (5 авг 2026) и 31 окт 2026 закрыл прокси, CLI и аккаунты, остался как курируемый каталог без прокси. Arcade привлёк $60M Series A (июнь 2026), всего $72M. Всё, что завязано на smithery-URL, надо снять.
- PulseMCP приостановил ручные подачи (страница обновлена 3 сент 2026) на время переделки пайплайна. Единственный путь туда: опубликоваться в официальном реестре, подхватят автоматически. Каталог держится на рассылке и консалтинге, около 22 тыс. серверов.
- Бизнес-модели делятся на 4 класса: (а) бесплатная инфраструктура (официальный реестр, GitHub, VS Code, Docker, Cline); (б) VC-платформы, зарабатывающие на хостинге, шлюзе и управлении (Glama, Arcade, Composio $29M); (в) SEO-каталоги с платным ускорением и dofollow (mcp.so $39, MCP Market $29); (г) вендорские in-app каталоги (Claude, ChatGPT, Cursor), бесплатные, но с ревью.
- Каталог Anthropic (Claude Marketplace, открыт 23 сент 2026, 2000+ листингов) самый ценный закрытый канал: нужен платный план Claude, автосканирование даёт статус Community, ревьюер даёт Verified, лимит 10 подач на организацию в сутки, одна подача на репо или папку, у подавших есть аналитика использования.
- Скиллы консолидировались вокруг формата SKILL.md (Anthropic, открытая спека в декабре 2025, передан в Agentic AI Foundation при Linux Foundation 22 окт 2026) и Vercel skills.sh / npx skills (старт 20 янв 2026). skills.sh индексирует скилл только после реальной установки (без формы и краулинга), поэтому tonydzi/join-the-fleet там пока даёт 404. Топ-скилл find-skills имеет 4,4M установок.
- Данные о трафике слабые и в основном самоотчётные: Glama заявляет 50k+ разработчиков и 1M+ вызовов в месяц, mcp.so заявляет 2,2M уникальных посетителей за год и DR 74. Независимых SimilarWeb-цифр и публичных кейсов «каталог X привёл Y установок» нет. Glama насчитал минимум половину из 81 отслеживаемых MCP-проектов мёртвыми или свернувшими бизнес.
- Для бесплатного OSS лучшая отдача примерно от 6 бесплатных площадок: официальный реестр → Glama (claim) → онбординг в GitHub MCP Registry → skills.sh (через установки) → плагин-каталог Anthropic (3–5 флагманских плагинов) → awesome-списки. Платные ускорители пропускать.
- Защита от спама везде ужесточается: лимит Anthropic 10 подач за сутки, ручной отбор GitHub и Glama, пауза PulseMCP. Единственная санкционированная автоматизация: путь 🤖🤖🤖 в PR для punkpeye/awesome-mcp-servers (~96,7k звёзд).
- Состояние артефактов Антона на момент проверки: sqlite-graph-memory есть в официальном реестре (io.github.tonydzi) и на Glama, но не на Smithery. join-the-fleet не проиндексирован на skills.sh.

## Рекомендации / решения
- На этой неделе (около 3–4 ч): обновить io.github.tonydzi/sqlite-graph-memory в официальном реестре (схема 2025-12-11); подать запрос на онбординг в GitHub MCP Registry (discussion #1257); заклеймить листинг на Glama (DNS TXT или /.well-known/glama.json) и добавить glama.json в корень репо.
- Довести join-the-fleet до чистой установки и провести ~5 реальных пользователей через npx skills add tonydzi/join-the-fleet, чтобы появилась запись и строки в лидерборде skills.sh.
- Открыть PR в awesome-mcp-servers (сервер) и awesome-claude-skills (флот), заявку в Cline marketplace (логотип 400×400) и добавить server-card и /.well-known/ai-catalog.json (SEP-2127).
- В течение месяца: упаковать 3–5 флагманских бандлов как плагины Claude Code и подать через claude.ai/directory/manage (≤10 за 24 ч, одна подача на папку), оформить .claude-plugin/marketplace.json в репо флота; подать PR в Docker MCP Catalog для сервера; следить за возобновлением PulseMCP.
- Держать один бандл-репо с флотом, а не 232 репо; не подавать 232 скилла в каталог Anthropic.
- Пропустить: прокси Smithery (мёртв), mcp.so за $39 и MCP Market за $29, листинги с обязательным бэклинком, подачу в ChatGPT для локального stdio-сервера (нужен хостинг, OAuth и верификация бизнеса).
- Мерить без телеметрии в продукте: GitHub Insights → Traffic (рефереры), счётчики skills.sh, скачивания npm/PyPI, Docker Hub pulls, аналитика портала Claude; в списках указывать прямой URL вендора, а не прокси. Раз в неделю 15 минут; звёзды считать запаздывающим индикатором.

## Сущности
- **Люди:** Tadas Antanavicius, Mike Coughlin, Anirudh Kamath, Henry Mao
- **Компании:** Anthropic, Vercel, Arcade.dev, Smithery, Glama, PulseMCP, mcp.so, Composio, Docker, GitHub, Cline, Cursor, OpenAI, Microsoft, Google, Meta, South Park Commons, SYN Ventures, Lightspeed, Agentic AI Foundation (Linux Foundation), TrueFoundry, Pipedream
- **Продукты/инструменты:** MCP Registry, mcp-publisher, server.json, GitHub MCP Registry, Glama, Smithery, PulseMCP, mcp.so, mcpservers.org, MCP.Directory, MCP Market, Docker MCP Catalog, Cline MCP Marketplace, Claude Marketplace / directory, skills.sh, npx skills, SKILL.md, agentskills.io, ChatGPT apps/plugins, Codex skills, Cursor Marketplace, awesome-mcp-servers, awesome-claude-skills, awesome-claude-code, npm, PyPI, sqlite-graph-memory, join-the-fleet, SEP-2127 server cards

## Открытые вопросы
- Точный независимый трафик каталогов (уровня SimilarWeb) не публичен; цифры Glama, mcp.so и Smithery самоотчётные.
- Выручка Glama и Smithery не раскрыта; публичного раунда у Glama не найдено.
- Нет публичных кейсов с цифрами рефералов от каталогов: какая отдача у каждой площадки, неизвестно.
- Семантика счётчика установок skills.sh неоднозначна (HN критикует непрозрачную атрибуцию установок).
- Критерии и сроки ревью для ChatGPT/Codex skills, а также время ревью бесплатного уровня mcp.so не опубликованы.
- Когда PulseMCP возобновит приём и подтянет ли он записи из официального реестра автоматически, пока не проверено.
- Начнут ли клиенты использовать server cards (SEP-2127) для обнаружения серверов: пока ни один клиент не использует.
- Часть цифр помечена как REPORTED (по сниппетам поиска): суммы финансирования, даты запусков, число звёзд; в отчёте за этой ногой обрезан хвост (прочитано 60000 из 62885 знаков).

## Источник
- DR-ID `DR26-11-17-MAYAK-04` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- MCP Registry и server.json
- распространение OSS-скиллов Claude Code
- SKILL.md и Agent Skills
- плагин-маркетплейс Claude Code
- install-driven индексация
- pay-to-play SEO-каталоги
- консолидация MCP-экосистемы
- awesome-lists как канал дистрибуции

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-11-19 через gh: 1 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 1 · 🤷 0.
