# DR26-07-15-HUB-01 — raw report from heavy

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: "grok.com web UI, режим Heavy (SuperGrok Heavy, аккаунт @Tony_Stef_ / Tony0x.eth)"
chat_url: "https://приватный чат"
dr_id: DR26-07-15-HUB-01
topic: "Надёжный ночной экспорт диалогов Grok и Gemini в Obsidian-волт"
date_collected: 2026-07-21



type: dr-report-original
verbatim: true
thinking_time: "1m 5s"
sources_count: 581
---

# DR26-07-15-HUB-01 — Reliable Nightly Export of Grok.com and Gemini.google.com Chat Histories into Local Obsidian Vault (as of mid-July 2026)

This dossier consolidates all viable rails for programmatic/semi-programmatic extraction of one's own conversation history from grok.com (SuperGrok / X SSO) and gemini.google.com, prioritising official/stable paths, then reverse-engineered backend JSON, then UI automation. It emphasises incremental, idempotent pipelines keyed by conversation ID, markdown + YAML frontmatter output, breakage detection, and ToS realities at tiny personal scale. All non-obvious claims carry sources, confidence tiers, and single-source/low-authority flags where applicable. Research draws from official pages, 2025–2026 community reports, GitHub, GreasyFork, X (last ~30 days sparse on technical depth), and cross-checked blogs.

## 1. GROK (grok.com / xAI) — Rails Comparison

| Method | Effort | Fragility | Incremental? | ToS Risk | Notes / Evidence |
|---|---|---|---|---|---|
| Official bulk data export (Settings → Data Controls → Export account data, или accounts.x.ai/data) | Low (manual or semi-auto request) | Low | No (full archive each time) | None (sanctioned) | ZIP под `ttl/30d/export_data/<uuid>/` содержит `prod-grok-backend.json` (основные диалоги), auth/billing JSON и `prod-mc-asset-server/` для медиа. Email/download link; минуты–часы (дольше для больших историй). **[established]** Sources: x.ai/legal/faq (May 2025, still referenced 2026), suprmind.ai (Jul 12 2026), gethonestpdf.com (Apr 2026), portable-ai-memory.org (observed Feb 2026), joindeleteme.com, Reddit r/grok. Отдельный «Export to PDF» на диалог тоже есть (Premium+). |
| X data archive (для Grok-on-X взаимодействий) | Low | Low–Medium | No | None | Отдельный ZIP 24–48 ч; включает часть Grok-данных в JSON. Неполон для standalone grok.com. |
| Backend REST (`/rest/app-chat/...`) через cookies | Medium–High (cookie harvest + client) | Medium–High (schema/auth drift) | Yes (по conversation ID / modify_time) | High (явный запрет в AUP) | List: `GET /rest/app-chat/conversations?limit=100`. Full: `/conversations/{id}/response-node`, `POST load-responses`. Auth: браузерные cookies от X SSO (`sso`, `sso-rw`, `x-anonuserid`, challenge/signature headers). Используется GreasyFork-юзерскриптами и неофициальными клиентами. Документированных rate-limit для личного использования нет; вежливые задержки рекомендуются. **[emerging для точного list-эндпоинта; single-source/script-derived для части путей]** |
| DOM / userscript / browser extension | Low–Medium | High (CSS/selector changes) | Partial | Medium–High | Только клиентская сторона или перехват. |
| xAI / Grok API (console.x.ai) | N/A | N/A | No | N/A | Строго для новых инференсов; **доступа к истории grok.com нет**. Отдельный продукт. **[established]** |

**Структура официального экспорта** (portable-ai-memory.org, observed Feb 2026):
`prod-grok-backend.json` содержит массив обёрток `{ "conversation": {id, title, create_time (ISO), modify_time (ISO), user_id, starred, system_prompt_name, ...}, "responses": [ { "response": {_id, parent_response_id, sender ("human"/model/"assistant"), message, create_time (BSON {"$date":{"$numberLong":"..."}}), model, thinking_trace, agent_thinking_traces, steps[], generated_image_urls, file_attachments, cited_web_search_results, ...}, "share_link": {...} } ] }`.
DAG через `parent_response_id`. Ассеты — в отдельных UUID-папках. Смешанные форматы времени требуют конверсии.

**Auth-механика:** X SSO → cookies на grok.com / accounts.x.ai. TTL сессии практически дни–недели до ре-авторизации; формально не документирован. Харвест через persistent browser profile (Playwright/Chrome с `user-data-dir`) или экспорт cookies. В части export-JSON встречаются поля bot-score (напр. `"botScore":90`). **[emerging / low-authority для точного TTL]**

**xAI API:** истории потребителя не даёт. **[established]**

## 2. GEMINI (gemini.google.com) — Rails Comparison

| Method | Effort | Fragility | Incremental? | ToS Risk | Notes / Evidence |
|---|---|---|---|---|---|
| Google Takeout (My Activity → Gemini Apps) | Low | Low | No (полный; можно расписание раз в 2 месяца на год) | None | Deselect all → My Activity → «All activity data included» → deselect all → отметить только **«Gemini Apps»** (не верхнеуровневые «Gemini»/Gems). JSON или HTML. Часы–дни обработки. Доставка email/Drive. **[established]** Sources: support.google.com threads (Feb–Jun 2026), xtrace.ai (Apr 2026), univik.com (Mar 2026), aifreeapi.com. Грабля: неверный продукт → пустой или metadata-only архив. |
| My Activity (myactivity.google.com/product/gemini) | Low | Medium (для скрейпа) | Partial | Low–Medium если скрейпить | Хронологический лог активности; годен для верификации или инкрементального HTML-харвеста. |
| Backend / reverse-engineered (cookie-based) | Medium | Medium–High | Yes (по chat ID / cid) | Medium–High | `HanaokaYuzu/Gemini-API` (Python, ~3.3k звёзд, активен до Apr 2026): `list_chats()`, `read_chat(cid)`. Cookies: `__Secure-1PSID` + `__Secure-1PSIDTS` (один авто-обновляется). **[emerging]** |
| UI / extension / userscript | Low–Medium | High | Partial | Medium | DOM-извлечение или перехват. |
| Официальный Gemini API / AI Studio | N/A | N/A | No | N/A | Продукт для разработчиков; истории gemini.google.com не отдаёт. **[established]** |

**Структура Takeout:** обычно `Takeout/My Activity/Gemini Apps/MyActivity.json` (массив событий). Поля: `header`, `title` («Used Gemini Apps»), `titleUrl` (содержит `gemini.google.com/app/c/<id>`), `time`, `products`, `details`/`userInteractions` с текстом запроса/ответа. Треды восстанавливаются группировкой по conversation ID из `titleUrl` + хронологической сортировкой. Мульти-тёрн-верность не всегда идеальна (есть сообщения о неполных промптах). Есть HTML-альтернатива. **[emerging / single-source разброс по полноте]**

**Расписание:** официальная опция «Export every 2 months for 1 year» существует; ежедневного/бессрочного нет. **[established]**

## 3. Reliability & Anti-Fragility (2025–2026 Community Evidence)

- **Официальные рельсы:** самые стабильные. Изредка задержки обработки или (редко) неполные/битые архивы (одна анекдотическая жалоба Jun 2026 по business-аккаунту в X **[single-source]**). Схема `prod-grok-backend.json` достаточно стабильна для конвертеров как минимум с начала 2026. Выбор продукта в Gemini Takeout — регулярная пользовательская грабля.
- **Backend / reverse:** ломается на смене auth, дрейфе схемы или обновлениях UI/API. Юзерскриптам (напр. `enhanced-grok-export`) требовались правки селекторов (документирована миграция на Tailwind в Jan 2025). У Gemini-API были проблемы с токенами/cookie, со временем починены. **Массовых банов аккаунтов за вежливые персональные экспортеры своих же данных в 2025–2026 в поиске нет.** Rate-limit и бот-детект существуют (Grok наследует агрессивность X; анонимный скрейп блокируется быстро), но персональные cookie-сессии с человеческим темпом на практике выглядят низкорисково. **[emerging для частоты поломок; speculative для объёма энфорсмента]**
- **UI/DOM:** максимальная хрупкость (изменения CSS). Требует частых правок.

**ToS-безопасность:**
- Официальный экспорт / Takeout: полностью санкционированы для своих данных.
- Backend/скриптовая автоматизация: xAI AUP (в силе с 26 Jun 2026) прямо запрещает «Accessing the Services through automated or non-human means, whether through a bot, script, or otherwise», реверс-инжиниринг, ботов, скрейпинг Input/Output. Политики Google аналогично ограничивают автоматический доступ. Энфорсмент на крошечном персональном нераспространяемом масштабе по доступным свидетельствам околонулевой, но текст однозначен. **[established для текста AUP; speculative/low-authority для реального энфорсмента персональных own-data инструментов]**
- Риск аккаунта: низкий для официального; низкий-средний для реверса при rate-limit и отсутствии абьюза. Режимы Private Chat / выключенной активности влияют на то, что вообще доступно.

**X (последние 30 дней):** технической дискуссии мало. Представительное: @lalopenguin (21 Jul 2026) — «go to settings for grok, gemini, chatgpt, and claude .. export your data .. once it's finished you'll get a bunch of zip files…» (низкий engagement). Кластера постов «экспорт сломался» или «забанили за экспортер» нет. Старая утечка расшаренных Grok-ссылок 2025 к приватному экспорту отношения не имеет. **[только-X для цитаты; существование фичи независимо подтверждено официальными доками]**

## 4. Recommended Architecture (Nightly, Incremental, Idempotent)

**Общие принципы**
- Ключ заметки — conversation ID (имя файла или `id:` во frontmatter).
- YAML frontmatter: `id`, `title`, `created`, `modified`, `model`, `source: grok|gemini`, `url` (если шарится), `tags`.
- Тело: чередование user/assistant; опционально сворачиваемые thinking traces / steps / цитаты / медиа-ссылки.
- Идемпотентность: перезапись только если `modified` новее или хэш содержимого отличается.
- Запуск на always-on Windows-хабе (или macOS); cookies/волт — на шифрованном томе.
- **Детект тихой поломки:** счётчики (сколько диалогов увидено, сколько новых/обновлённых заметок записано), канарейка (известный диалог — сверка содержимого/хэша), прогресс max `modify_time`, алерт при нулевом прогрессе. Валидация схемы при парсинге.
- **Обновление cookie/токенов:** persistent browser profile (Playwright `launch_persistent_context` или Chrome `user-data-dir`). Детект 401/403 → триггер ре-логина (ручной или полуавто). Сессии живут дни–недели.

**Grok pipeline (лестница фолбэков)**
1. **Primary** (ночной инкремент, если AUP-риск принят): харвест cookies → `GET` список (`/rest/app-chat/conversations?limit=...` + пагинация/курсор) → для новых/обновлённых ID тянуть полное содержимое → парс в MD → запись/обновление волта. Вежливые задержки 1–5 с, браузероподобные UA/headers.
2. **Canary / source-of-truth** (еженедельно или по требованию): триггер официального экспорта (вручную или скриптом на accounts.x.ai/data) → скачать ZIP → парсить `prod-grok-backend.json` (переиспользовать логику `Owlock/easy-grok-chat-exporter` или конвертера portable-ai-memory) → полная сверка.
3. **Last resort:** Playwright UI-автоматизация (открыть историю, скроллить/догружать, извлечь DOM) или юзерскрипт-кнопка.

**Бутстрап:** (1) один полный официальный экспорт → базовое наполнение волта + валидация парсера; (2) поднять persistent browser profile, залогиниться один раз через X SSO, экспортировать/протестировать cookies; (3) реализовать list+fetch клиент, добавить канарейку; (4) поставить в расписание (cron / Task Scheduler) с логами и алертом на аномалию; (5) опционально — сложить медиа Imagine во вложения волта.

**Gemini pipeline (лестница фолбэков)**
1. **Primary по частоте:** cookie-based через `HanaokaYuzu/Gemini-API` — `list_chats()`, затем `read_chat(cid)` для дельт.
2. **Canary / bulk:** Takeout (My Activity → Gemini Apps, предпочтительно JSON) → парс/группировка по conversation ID из `titleUrl` → MD (переиспользовать `Coryrichter94/gemini-to-obsidian`).
3. **Last:** UI/расширение или скрейп My Activity HTML.

Бутстрап тот же. Важно: сохранение активности должно быть включено, иначе история дальше будет неполной.

**Мнение (уверенность высокая):** на чисто персональном масштабе второго мозга на приватном хабе реверс/cookie-рельсы дают нужную ночную инкрементальность при приемлемой практической надёжности и околонулевом наблюдаемом риске энфорсмента — при условии rate-limiting и отсутствия перераспространения. Официальные рельсы остаются базой комплаенса и канарейкой. Не распространять сам инструментарий автоматизации. Предпочитать официальное там, где терпима задержка; реверс трактовать как «восстановление своих данных» с задокументированным риском.

## 5. Prior Art (GitHub / GreasyFork / Extensions, 2025–2026)

**Grok**
- `Owlock/easy-grok-chat-exporter` — парсит официальный `prod-grok-backend.json` → MD/TXT/JSONL с thinking traces. Zero-deps Python CLI. **Переиспользовать парсер.**
- `iikoshteruu/enhanced-grok-export` (GreasyFork + GitHub) — юзерскрипт, мульти-формат (MD/JSON/PDF), детект спикера, авто-скролл. Обновлялся под изменения UI 2025.
- GreasyFork «Grok - Export all your conversations (JSON, Markdown)» (≈Mar 2026) — использует backend list/fetch.
- `revivalstack/ai-chat-exporter` (Tampermonkey, v3.1 ≈2026) — мульти-платформенный включая Grok, богатый MD + YAML.
- `portable-ai-memory.org` конвертеры / схема (раздел Grok) — лучше всего задокументированная структура + маппинги полей.
- `rohernan76/grok-export-decoder`, `numq/GrokViewer` — работа с сырыми/бинарными артефактами экспорта (особенно Imagine).
- Chrome: YourAIScroll, AI Toolbox, «Grok to Obsidian and Html» и др.

**Gemini**
- `HanaokaYuzu/Gemini-API` — зрелый реверс-клиент на Python (list/read chats через cookies). Активен до Apr 2026. **Главный кандидат для инкремента.**
- `Coryrichter94/gemini-to-obsidian` — Takeout → чистый Obsidian MD + frontmatter. **Напрямую переиспользуемо.**
- Прочее: `rashidazarang/chatgpt-chat-exporter` (поддержка Gemini), Chrome «Export Gemini Chats to Obsidian», Simple Exporter for Gemini, AI Toolbox, YourAIScroll.
- `obsivault` (PyPI) — мульти-провайдер (вкл. Grok + Gemini) в раскладку Obsidian-волта.

**Избегать / осторожно:** неподдерживаемые инструменты эпохи 2024; любые, что заливают данные на сторонние серверы; агрессивные скрейперы, игнорирующие rate-limit.

## Breakage Playbook
- Ноль новых диалогов / застывший max `modify_time` → проверить auth (пере-харвест cookies), здоровье эндпоинта, статус сохранения активности (Gemini).
- Провал валидации схемы / ошибка парсинга → заморозить запись, откатиться к последнему хорошему экспорту или UI, обновить парсер.
- 401/403 или бот-челленджи → ре-логин через persistent profile; увеличить задержки; временно перейти на только-официальное.
- Takeout пустой / не тот продукт → проверить «Gemini Apps» под My Activity.
- Сигналы риска аккаунта (редко) → немедленно остановить реверс, только официальное.
- Мониторинг: ежедневные счётчики + хэши канареек + канал алертов. Держать последние 2–3 полных официальных архива как ground truth.

## Sources (all retrieved/verified in research window)
- x.ai/legal/faq, x.ai/legal/privacy-policy, x.ai/legal/acceptable-use-policy (eff. 26 Jun 2026), x.ai/legal/terms-of-service
- portable-ai-memory.org/providers/grok/ (и связанные schema/import страницы; структуры наблюдались с Feb 2026)
- suprmind.ai/hub/grok/how-to-delete/ (Jul 2026), gethonestpdf.com (Apr 2026), memoryplugin.com, joindeleteme.com
- support.google.com/gemini threads (Feb–Jun 2026), xtrace.ai (Apr 2026), univik.com, aifreeapi.com
- GitHub: Owlock/easy-grok-chat-exporter, iikoshteruu/enhanced-grok-export, HanaokaYuzu/Gemini-API, Coryrichter94/gemini-to-obsidian, revivalstack/ai-chat-exporter, mem0ai/grok3-api (archived) и др.
- GreasyFork Grok scripts (2025–2026)
- Reddit r/grok, r/GoogleGeminiAI (various 2025–2026)
- X posts (напр. @lalopenguin 21 Jul 2026) и вторичные отчёты
- Chrome Web Store listings (YourAIScroll, AI Toolbox, Grok/Gemini-to-Obsidian extensions)

> This architecture matches the existing ChatGPT pipeline philosophy (bearer/cookie + incremental by ID + idempotent MD) while respecting the different official surfaces and ToS realities of Grok and Gemini. Implement official first for baseline and compliance, then layer reverse for true nightly delta. Re-validate endpoints and AUP text periodically (they change).

**Итого прогона:** Heavy, «Thought for 1m 5s», **581 источник**.
