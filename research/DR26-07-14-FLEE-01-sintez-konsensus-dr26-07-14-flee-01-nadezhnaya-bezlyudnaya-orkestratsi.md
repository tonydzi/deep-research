---
dr_id: DR26-07-14-FLEE-01
title: "Синтез-консенсус DR26-07-14-FLEE-01 — надёжная безлюдная оркестрация DR-fanout"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Синтез: безлюдный DR-fanout (ChatGPT + Grok; Gemini добьётся ночью)

> Гетеро-консенсус двух независимых вендоров (OpenAI-модель + xAI-модель). Расхождения НЕ сглажены. Gemini добавится ночным коллектором — усилит, но не переворачивает выводы (оба сошлись сильно).

## Таблица консенсуса

| Тема | ✅ Сошлись (оба) | ⚖️ Разошлись | 🔹 Уникальное |
|---|---|---|---|
| **Ядро оркестрации** | Durable state-machine + job-ledger, НЕ «долгоживущий автономный браузер-агент». Состояния: created→preflight→prompt_staged→mode_verified→submit_committed→started→waiting→ready→collected→normalized→delivered + needs_reauth/drift_suspected/quota_burn/aborted | — | ChatGPT называет движки поimённо (Temporal / Inngest / LangGraph / **Local Ledger FSM**) с таблицей и рекомендует Local FSM для одно-хозяйного хаба → апгрейд на Inngest при росте. Grok: SQLite/Postgres ledger + cron/systemd/APScheduler/Celery |
| **Probe-before-burn** | Три гейта до необратимого submit: (a) композер держит ВЕСЬ промпт (ре-чтение+hash), (b) DR-режим реально включён, (c) ран реально стартовал. Иначе — abort, 0 ретраев | — | ChatGPT: для ChatGPT «доказательство режима» = видимость lifecycle плана, не «кнопка выглядела нажатой». Grok: canary = малые не-сабмитящие проверки |
| **UI-drift устойчивость** | role/ARIA-локаторы первыми; LLM/vision — только фолбэк; семантический поиск в **dry-run/repair-режиме**, НЕ на квота-сабмите | — | ChatGPT: Playwright `locator.normalize()`, Stagehand `observe()` discovery-only. Grok: browser-use, Playwright MCP, Computer Use (CUA), browserops, self-healing |
| **Браузерный слой** | Firefox + `launch_persistent_context`, выделенный профиль на вендора, человек логинится 1 раз, residential static IP (совпадает с нашим HUB-01) | — | Grok глубже про анти-бот: Firefox vs Chromium stealth (Patchright, SeleniumBase UC), human-like задержки, low cadence, keepalive-визиты |
| **Извлечение отчёта** | Лестница: официальный export/share → backend JSON → DOM-scrape → эскалация человеку. DOM — низший приоритет | — | ChatGPT: backend `/backend-api/conversation/<id>` + `widget_state`/`report_message` = лучший escape-hatch, но [single-source] → держать contract-тест + repair-lane (**мы им и собрали ✅**). Grok: iframe-sandbox блокирует синтет-клики, blob-проблемы |
| **Gemini = лучший официальный выход** | Оба: **Gemini Deep Research Agent / Interactions API** (background async + polling, collaborative_planning) — production-ready, почти отменяет браузер-хак для Gemini | — | ChatGPT добавляет consumer Scheduled Actions + Gemini Spark. Grok: `deep-research-preview-04-2026`, `background=True`, `interactions.get()` |
| **⚠️ Grok = policy-hostile** | Оба: xAI AUP (26.06.2026) ЯВНО запрещает «automated or non-human means». Оба рекомендуют **НЕ строить безлюдную Grok-consumer автоматизацию** / избегать / high-risk | **⚠️ Прямо противоречит решению Антона 16.07 «Grok просто продолжаем использовать в обычной ротации»** — см. развилку ниже | ChatGPT: Reuters 15.07 — xAI уже забанил >52k аккаунтов в 2026 (за абьюз, не именно автоматизацию, но enforcement-at-scale реален). Grok сам про себя: «highest barriers, avoid or minimal scope» |
| **Session longevity** | Не публикуется ни у кого → forced re-auth считать неизбежным, `needs_reauth` = first-class состояние + out-of-band пинг человеку | — | ChatGPT: Google привязывает сессию к cookies/trusted-device, Workspace-админ может укоротить; OpenAI добавил session-controls |
| **Хрупкость хардкода** | UI-специфичные репо мрут быстро: `chatgpt-automation-mcp` (архив 27.04.2026), `browserbase/gemini-browser` (архив 20.05.2026). Держи вендор-логику тонкой, оркестрацию — общей | — | — |
| **2-вендорный старт** | Оба: сначала **ChatGPT + Gemini** безлюдно; Grok — отдельно/осторожно | — | — |

## 🍴 Развилка для Антона (гетеро-принцип: не сглаживаю)
**Оба DR-вендора независимо советуют НЕ автоматизировать Grok безлюдно** (AUP явно запрещает, enforcement реален). Это **противоречит** твоему решению 16.07 «Grok просто продолжаем использовать, темп крошечный, мы не мишень» ([[ip-sensitive-actions-hub-only]] сосед). Твой аргумент (микро-темп, охота на масс-скрейп $15k/1M) не опровергнут — но обе внешние модели ставят Grok в «high-risk/avoid». Варианты:
- **A. Держать твоё решение** (Grok в ротации, живой Chrome-MCP человеческим темпом) — риск осознан, DR его лишь подтвердил как реальный, не новый.
- **B. Понизить Grok** до «по запросу, не в безлюдном расписании» — ровно то, что советуют оба DR; Gemini+ChatGPT несут основную нагрузку.
- Рекомендация синтеза: **гибрид** — Grok остаётся, но НЕ в безлюдном демоне (только живой Chrome-MCP как сейчас); безлюдное расписание = ChatGPT+Gemini. Это и твоё «продолжаем», и DR-осторожность одновременно. Confidence [emerging]. Решаешь ты.

## Что это меняет в skill dr-fanout v2 (для Маяка)
1. **submit→probe→collect как явный конечный автомат с ledger** (у нас уже есть леджер-файл `_drafts/DR-FANOUT-*` — формализовать в состояния + идемпотентный resume по DR-ID).
2. **Три probe-гейта перед Send** — уже реализованы в v2-костяке (проверял режим+длину+first/last-60 перед каждым Send этого прогона ✅). Добавить: hash полного промпта, aria-snapshot baseline композера для drift-детекта.
3. **Извлечение** — лестница подтверждена практикой прогона: ChatGPT backend-JSON widget_state ✅ (сработал с первого раза, 36K); Grok main-innerText с обрезкой эха ✅ (но blob тихо не долетел → Bash-обрезка эха); Gemini — самый медленный (>20 мин), immersive-канвас, добирается ночным коллектором.
4. **Gemini официальный API** — оценить перенос Gemini-канала с браузера на Interactions API (background+polling): это ⚠️ УСЛОЖНЕНИЕ (платный ли? проверить квоту vs подписка) но снимает самый хрупкий/медленный браузерный канал. Ключевой открытый вопрос: даёт ли Interactions API DR по подписке или это отдельный биллинг (ChatGPT-отчёт: «НЕ то же free-with-subscription»). **Перед стройкой — проверить биллинг** ([[prefer-included-limits-before-paid-api]]).
5. **needs_reauth + drift_suspected** — единственные два человеко-гейта, всё прочее безлюдно.

## Живая фактура этого прогона (мы = подопытные своего же DR)
- ChatGPT: баннер «Suspicious activity detected» при старте DR из автоматизации (не заблокировал) = ровно failure-mode «account flags on automation-like patterns» из обоих отчётов.
- Gemini plan-фаза медленная (~3-4 мин), report-write ещё дольше (>20 мин суммарно) — самый медленный канал, кандидат №1 на офиц. API.
- Grok blob-download тихо не долетел (задокументированная грабля) → Bash-фолбэк; экстракт сперва зацепил эхо промпта (якорь на «Thought for…» надёжнее первого «Executive Summary»).
- TrustedHTML CSP у Gemini → `replaceChildren`, не `innerHTML`.
