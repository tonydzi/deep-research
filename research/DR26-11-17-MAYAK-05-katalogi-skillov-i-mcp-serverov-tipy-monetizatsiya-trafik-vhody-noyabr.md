---
dr_id: DR26-11-17-MAYAK-05
title: "Каталоги скиллов и MCP-серверов: типы, монетизация, трафик, входы (ноябрь 2026)"
date: 2026-11-17
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-11-17-MAYAK-05): Каталоги скиллов и MCP-серверов: типы, монетизация, трафик, входы (ноябрь 2026)

> Какие каталоги для бесплатных Agent Skills и MCP-серверов существуют в конце 2026, как они зарабатывают, сколько реального трафика даёт листинг, как в них попасть и куда небольшой free-only OSS-лаборатории вкладывать ограниченные часы.

## Ключевые выводы
- Официальный MCP Registry (запуск 8 сен 2025, CLI mcp-publisher, без ревью, бесплатно) автоматически копируется в Glama, PulseMCP, mcp.directory и GitHub MCP Registry: одна публикация server.json даёт максимальный рычаг для любого MCP-сервера. Пример: sqlite-graph-memory уже попал в Glama и ClaudePluginHub без доп. действий.
- Smithery ушёл из хостинга: Arcade купил его 5 авг 2026, прокси и managed identity закрыты 31 окт 2026, остался только публичный непроксируемый каталог. Arcade (по прессе, REPORTED) привлёк Series A $60M (SYN Ventures, Morgan Stanley, Wipro), всего ~$72M. Порядок приёма новых листингов после поглощения не опубликован.
- PulseMCP приостановил ручные заявки примерно с середины авг 2026 на время переработки ингеста и обещает импорт из официального реестра после возобновления: сейчас листинг там недоступен как действие.
- skills.sh (Vercel, запуск 20 янв 2026): ~1 млн скиллов и ~280 млн установок за 7 месяцев, ранжирование только по анонимной телеметрии CLI, формы подачи нет (достаточно SKILL.md в публичном репо и npx skills add). Концентрация экстремальная: 375 скиллов (0,04%) дали 62% установок, топ-1,2% дали 94%, почти половина установлена ровно один раз; листинг без внешнего импульса невидим.
- Anthropic открыл каталог для всех 25 сен 2026 через claude.ai/directory/manage: нужен платный план Claude, GitHub-репо (публичный на старте), claude plugin validate, политика приватности, аннотации инструментов, тест-аккаунт; автоскан безопасности плюс ручная проверка без фиксированного SLA, обновления через Git, аналитика (установки, просмотры, поисковые запросы). Старый поток marketplace.json сворачивается, PR в claude-plugins-official не мониторятся.
- Основная выручка независимых каталогов это платные fast-lane: mcp.so $39 разово (мгновенно, dofollow, featured), mcpservers.org $39 (24 ч), MCP Market $29 (24 ч) против бесплатной очереди 4-6 недель. По сути это SEO-бизнес продажи ссылок и размещения. Глама монетизируется хостингом и гейтвеем (freemium), MCPize обещает 80% rev share, ToolHive (Stacklok) продаёт enterprise-governance; Vercel и официальный реестр денег не берут (ecosystem play).
- Трафик каталогов мал, самоотчёты завышены: независимый аудит (DataForSEO, 27 окт 2026) показал, что только 3 из 21 каталога имеют >10 000 визитов/мес из поиска; крупнейший MCP Market ~81k/мес против заявленных им «1M+». Полевые эксперименты: из 30 сайтов реально листинг появился лишь на 3 (официальный реестр, Glama, LobeHub); у Linkly одна заявка 6 недель ушла в никуда, другая требовала write-доступ ко всем репо за листинг с 11 установками.
- Каталоги сильно дублируют друг друга и забиты мёртвым: 17 320 строк реестра схлопываются в 11 447 уникальных GitHub-серверов, 32,8% без коммитов 180+ дней, 405 «зомби»-серверов; в одном сводном списке скиллов 94 записи на несуществующие репо. Поэтому курируемые площадки (Claude directory, Cursor, GitHub registry, awesome-списки) весят непропорционально больше.
- Встроенные в клиенты каталоги (Claude, ChatGPT/Codex, Cursor Marketplace, Cline, Docker MCP Catalog, VS Code через GitHub registry) дают реальных пользователей, но самые жёсткие входы: верификация личности, тест-аккаунты, 5 позитивных и 3 негативных тест-кейса и видео (OpenAI), ручная отборка и бесплатные MIT/BSD/Apache-плагины (Cursor), PR и сборка образа Docker'ом (~24 ч). Cline берёт заявки issue с логотипом 400x400.
- Для 232 скиллов не ставить на один бандл: спрос на skills.sh и вход в Anthropic идут по отдельным скиллам/плагинам; рекомендуется 10-20 флагманских скиллов с индивидуальной полировкой, join-the-fleet как монорепо для длинного хвоста, массовая подача всех 232 нигде не нужна и вредна. Свой маркетплейс плагинов Claude Code строить не стоит.

## Рекомендации / решения
- Опубликовать каждый MCP-сервер в официальный реестр (mcp-publisher, server.json, верификация namespace через GitHub/DNS), затем заявить (claim) автоимпортированные листинги в Glama и mcp.directory.
- Подать 5-10 флагманских скиллов как плагин-бандлы и sqlite-graph-memory в Claude directory (нужен один платный план Claude); использовать вкладку аналитики как инструмент замера.
- Разложить скиллы по отдельным чистым репо/путям под npx skills add owner/repo, добавить бейдж skills.sh и по одному анонсу (HN/X/Reddit) на флагманский скилл, чтобы получить телеметрию для рейтинга.
- Взять бесплатные PR в awesome-списки: punkpeye/awesome-mcp-servers, ComposioHQ/awesome-claude-skills, hesreallyhim/awesome-claude-code, ccplugins; затем Cline (issue), Docker MCP Registry (PR), mcp.directory и mcpservers.org (бесплатные очереди), agenticskills.io, запрос в GitHub MCP Registry.
- OpenAI plugin directory отложить до появления удалённо хостящегося сервера (нужен публичный HTTPS, верификация, тест-кейсы); платные fast-lane (mcp.so $39, MCP Market $29) использовать только после доказательства реферального трафика.
- Пропустить: PulseMCP (пауза), Smithery (процесс неясен), тонкие скрейперы скиллов (skillsmp, aiskill.market), любые площадки, требующие write-доступ к репо или капчу за листинг; массовую автоподачу не делать.
- Стандартный пакет на артефакт: LICENSE, README с homepage первым, лого, server.json, frontmatter SKILL.md, политика приватности, тест-аккаунт для ревьюеров; UTM-метки в README для атрибуции; команда --check/quickstart для измерения реального первого использования.
- Пересматривать ландшафт ежеквартально: полураспад площадок меньше полугода (Smithery, PulseMCP).

## Сущности
- **Люди:** Tadas Antanavicius, Mike Coughlin, Anirudh Kamath
- **Компании:** Anthropic, OpenAI, Vercel, Arcade, Smithery, PulseMCP, Glama, mcp.so, MCP Market, mcpservers.org, mcp.directory, GitHub, Microsoft, Docker, Cursor, Cline, Stacklok, MCPize, Composio, SYN Ventures, Morgan Stanley, Wipro, Snyk, Trail of Bits, Mistral, DataForSEO, SubmitMCP, AgenticSkills, LobeHub, Skiln, Top MCPs, MentionAgent, Linkly
- **Продукты/инструменты:** Official MCP Registry, mcp-publisher, server.json, GitHub MCP Registry, Claude directory, claude plugin validate, claude-plugins-official, ChatGPT/Codex plugin directory, Cursor Marketplace, cursor.directory, Cline MCP Marketplace, Docker MCP Catalog, VS Code MCP gallery, skills.sh, npx skills add, SKILL.md, Agent Skills spec (agentskills.io), agenticskills.io, ClawHub, awesome-mcp-servers, awesome-claude-skills, awesome-claude-code, ToolHive, MCPB (.mcpb), sqlite-graph-memory, join-the-fleet, ClaudePluginHub, Pulse Score

## Открытые вопросы
- Примет ли Arcade/Smithery общественные заявки после закрытия прокси 31 окт 2026 и на каких условиях.
- Дата и критерии возобновления приёма заявок в PulseMCP (опубликовано только «как будет готово»).
- Реальная атрибуция установок: ни один каталог публично не отдаёт per-listing рефералы или установки; только Claude directory (издателю) и skills.sh (агрегат), нужна собственная инструментация.
- Финансирование и выручка Glama, PulseMCP, mcp.so, MCP Market, mcpservers.org не публичны; данные Arcade Series A взяты из прессы без первичного источника.
- Расширит ли Cursor ручной маркетплейс до сообщества (сейчас сообщество отправляют в cursor.directory).
- Конфликт счётчиков серверов (PulseMCP от ~7k до ~21,5k у разных источников) и самоотчётов трафика (1M+/мес, 2,2M за 12 мес) против независимой оценки ~81k: всё считать порядком величины.

## Источник
- DR-ID `DR26-11-17-MAYAK-05` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- MCP Registry и server.json
- Agent Skills / SKILL.md
- skills.sh и закон концентрации установок
- Pay-to-play SEO-каталоги
- Дистрибуция OSS и заметность в топ-10 экосистемах
- Claude Code плагины и marketplace
- Awesome-lists как источник доверия
- Консолидация и пивоты рынка MCP (Smithery, PulseMCP)

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-11-19 через gh: 0 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 0 · 🤷 0.
