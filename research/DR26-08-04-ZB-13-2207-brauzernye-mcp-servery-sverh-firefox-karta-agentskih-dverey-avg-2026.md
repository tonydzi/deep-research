---
dr_id: DR26-08-04-ZB-13-2207
title: "Браузерные MCP-серверы сверх Firefox: карта агентских дверей (авг. 2026)"
date: 2026-08-04
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-08-04-ZB-13-2207): Браузерные MCP-серверы сверх Firefox: карта агентских дверей (авг. 2026)

> Исследование выясняет, какие браузеры кроме Firefox имеют родной/официальный MCP-сервер, пригодный для attach к живому браузеру с сохранением логинов, и нужен ли второй движок сверх Firefox + @mozilla/firefox-devtools-mcp.

## Ключевые выводы
- Прямой ответ: Firefox + @mozilla/firefox-devtools-mcp достаточно для подавляющего большинства задач; единственный технически равноценный второй рельс — Google chrome-devtools-mcp через --browser-url (официальный, v1.6.0 от 2026-07-14).
- Полную связку attach-to-live + on-disk профиль с логинами + survive-disconnect + re-attach + headless дают только два движка: Firefox (--connect-existing) и Chrome (chrome-devtools-mcp --browser-url).
- Apple выпустил официальный safaridriver --mcp (STP 247 и Safari 27 beta, 01.07.2026, 16 инструментов), но сессия изолированная, БЕЗ cookies/логинов — бесполезен для действий под своим аккаунтом; реальную сессию даёт только неофициальный community safari-mcp (macOS-only).
- Microsoft @playwright/mcp (офиц., v0.0.78) attach делает только через браузерное расширение (Chrome/Edge, ручной выбор вкладки) или --cdp-endpoint — не headless-friendly, добавляет human-in-the-loop.
- Puppeteer-MCP (@modelcontextprotocol/server-puppeteer) перемещён в servers-archived, не поддерживается — не ставить.
- У Edge, Brave, Vivaldi, Arc/Dia, Zen/LibreWolf/Waterfox, Ladybird, Servo нет родного/официального MCP: Edge наследует chrome-devtools-mcp через --executablePath; Brave и Vivaldi принципиально без MCP (Vivaldi CEO публично против AI-агентов); Firefox-форки покрываются тем же Mozilla MCP по BiDi/Marionette.
- Opera Neon имеет официальный MCP Connector (анонс 2026-03-31), но это агентный браузер по подписке, отдающий live-контекст внешним клиентам — экзотика вне текущего кейса.
- Риск класса CometJacking (indirect prompt injection через контент страницы, раскрыт LayerX/Brave Security в отношении Comet, авг–окт 2025) — архитектурная проблема любого MCP с доступом к залогиненным аккаунтам, а не баг конкретного вендора; опасны tools evaluate_script, install_extension, unrestricted file paths.
- Mozilla прямо предупреждает не держать Marionette включённым при обычном сёрфинге — она выставляет navigator.webdriver=true и меняет fingerprint, триггеря Cloudflare/Akamai.

## Рекомендации / решения
- Этап 0 (до 2026-08-18): ничего не менять, продовый рельс Firefox заморожен на время замера.
- После 2026-08-18 провести 48-часовой 'pretend first' тест chrome-devtools-mcp на Windows-хабе: отдельный user-data-dir, --browser-url к внешнему Chrome, проверка survive-disconnect/re-attach и работы на резидентном IP без бот-детекта.
- Ставить chrome-devtools-mcp только при подтверждённом наличии строго-Chromium сайта в рабочей корзине; если таких сайтов нет — не ставить вообще ('не на всякий случай' инструмент).
- safari-mcp (community) добавлять только при появлении Safari-only задачи под своим логином на Mac; официальный Apple safaridriver --mcp не подходит из-за отсутствия логинов.
- Playwright-MCP использовать только как dev-инструмент для генерации тестов/кросс-движковой проверки на ноутбуке, не как продовый резидентный рельс.
- Выбросить из стека: Puppeteer-MCP (архивирован), любые community-обёртки под Firefox-форки (избыточны, покрыты Mozilla MCP), облачные Browserbase/Steel (foreign IP убивает смысл резидентного IP).

## Сущности
- **Люди:** Jon von Tetzchner
- **Компании:** Mozilla, Google, Microsoft, Apple, Opera, Brave, Vivaldi, The Browser Company, Atlassian, Perplexity, LayerX
- **Продукты/инструменты:** Firefox, @mozilla/firefox-devtools-mcp, chrome-devtools-mcp, @playwright/mcp, @modelcontextprotocol/server-puppeteer, safaridriver --mcp, safari-mcp, Opera Neon MCP Connector, Chrome, Edge, Windows 365 Agents MCP, Safari Technology Preview, Ladybird, Servo, Zen Browser, LibreWolf, Waterfox, WebDriver BiDi, Marionette, Comet, Browserbase, Steel

## Открытые вопросы
- Точная версия @playwright/mcp на момент решения не финализирована — рекомендовано проверить npm view перед принятием решения.
- Гарантия survive-disconnect/re-attach у chrome-devtools-mcp архитектурно достоверна, но не подтверждена однострочно в README; есть открытые issues по надёжности reconnect (#1094, #2033) и невозможности рантайм-смены browser-url (#2154).
- Наличие флага --mcp у safaridriver в конкретных релизных сборках Safari не гарантировано — помечено как [contested], требуется проверка на месте; после обновлений STP macOS отзывает Automation-грант.
- Загрузка расширений в attach-режиме chrome-devtools-mcp не работает до Chrome 149 — актуальность для кейсов с расширениями на живой сессии нужно перепроверять по версии Chrome.

## Источник
- DR-ID `DR26-08-04-ZB-13-2207` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- concept-ai-agent-orchestration
- concept-autonomous-ai-agents
- concept-agent-experience-ax
- model-context-protocol
- browser-automation
- prompt-injection
