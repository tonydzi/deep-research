---
dr_id: DR26-07-15-HUB-01
title: "DR-синтез: надёжный ночной экспорт диалогов Grok и Gemini в волт (архитектура импортёров)"
date: 2026-07-21
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# DR26-07-15-HUB-01 — экспорт Grok/Gemini в волт: синтез двух прогонов

Два независимых прогона одного вендора (первый боевой тест подписки SuperGrok Heavy): **Grok web Heavy** («Thought for 1m 5s», 581 источник) и **Grok CLI** (grok-build 0.2.106, ~5 мин, 53 URL). Отчёты сходятся по стержню, расходятся по деталям — расхождения помечены.

## TL;DR

Ни у Grok, ни у Gemini **нет ChatGPT-класса рельсы** «bearer + документированный REST по истории» — API-ключи обоих вендоров к истории потребительских чатов доступа не дают вообще. Рабочая архитектура = **двухслойная**: официальный bulk-экспорт как истина и канарейка (Grok Settings→Export Data; Gemini Takeout «My Activity → Gemini Apps») + опциональный reverse-cookie слой для настоящей ночной инкрементальности (Gemini: зрелый `gemini_webapi`; Grok: живые endpoints `/rest/app-chat/*` найдены, но AUP xAI от 26.06.2026 прямо запрещает автоматизацию). Детекторы тихой поломки (счётчики, канарейки, watermark) — первоклассный код, не afterthought. Главная дельта к решению 14.07: **Grok из «отложить» повышается до дешёвого weekly-актора** (экспорт → письмо → парсер `prod-grok-backend.json` → локальная дельта).

## Ключевые выводы

1. **API ≠ история** (established, оба прогона). xAI API (`api.x.ai`) и Gemini API / AI Studio — отдельные продукты для новых генераций; список/содержимое чатов grok.com и gemini.google.com они не отдают. Покупать API-квоту ради импорта истории бессмысленно.
2. **Grok: официальный экспорт реален, но on-demand** (established для факта; emerging для формата). Grok.com → Settings → Data Controls → Export Data → письмо со ссылкой → ZIP с `prod-grok-backend.json` (диалоги + thinking traces + DAG через `parent_response_id`, BSON-таймстампы) + папки медиа. Готовый парсер — `Owlock/easy-grok-chat-exporter`. Инкремента у источника нет — дельта делается локально (conversation_id + content-hash). ⚠️ Формат НЕ хардкодить: источники расходятся (голый JSON vs ZIP; Settings vs `accounts.x.ai/data`; LLMnesia вообще утверждает, что кнопки bulk-экспорта нет) — детектор обязан принимать все варианты.
3. **Grok: скрытый backend всё же есть** (emerging, только web-Heavy; CLI-прогон его НЕ нашёл и пометил «fringe — not found»). `GET /rest/app-chat/conversations?limit=100` + `/conversations/{id}/response-node`, auth = браузерные cookies X SSO (`sso`, `sso-rw`). Это дало бы настоящий ночной инкремент, НО: AUP xAI (в силе с 26.06.2026) прямо запрещает «accessing the Services through automated or non-human means». Энфорсмент на личном масштабе по свидетельствам ≈ нулевой (speculative), текст однозначен → по умолчанию рельса ВЫКЛ.
4. **Gemini: Takeout = каноничный bulk** (established). Точный путь: Deselect all → **My Activity** → «All activity data included» → только **«Gemini Apps»** → JSON. Грабля №1 у пользователей: выбрать верхнеуровневый продукт «Gemini» → приедут Gems-конфиги, а не чаты. Формат: `MyActivity.json`, треды восстанавливаются группировкой по conversation ID из `titleUrl`. Родное расписание «раз в 2 месяца, год, 6 экспортов» подтверждено; ссылка живёт ~7 дней.
5. **Gemini: ночной инкремент возможен уже сегодня** (emerging). `HanaokaYuzu/Gemini-API` (`pip install gemini_webapi`, 3.3k★, релизы до Apr 2026): `list_chats()` + `read_chat(cid)`, cookies `__Secure-1PSID` + `__Secure-1PSIDTS` с авто-обновлением. Ближайший аналог нашего ChatGPT-bearer-харвеста. Риск: постоянный дрейф эндпоинтов Google (пакет живёт именно потому, что его чинят), серая зона ToS.
6. **Полнота Takeout оспаривается** (speculative, спор источников). Одни источники — «полный архив», другие (`Liyue2341/gemini-exporter`) — «обрезанные пары промпт-ответ». Не усреднять, а **инструментировать**: канарейка сверяет длину N живых диалогов UI vs экспорт; систематический недолив → повысить RE-рельсу до основной по телу текста.
7. **X-архив ≠ история grok.com** (emerging, подтверждено аккаунтом @grok 15.07.2026): standalone Grok и Grok-в-X держат **раздельные** истории, переноса нет. X-архив — только форензик-бэкап, не рельса.
8. **Тихая поломка — главный враг** (консенсус обоих прогонов + весь наш опыт). Обязательный слой: счётчики (`conversations_seen/written/skipped`, `parse_errors`, `auth_ok`), канарейки (известные диалоги, хэши), watermark max `modify_time`, алерт «0 новых 7 дней при живом использовании», non-zero exit при hard fail. Свежие анекдоты потерь истории на стороне Grok (откат чата, Jul 2026) — довод «сервер не истина, локальный бэкап обязателен».
9. **ToS-лестница «безопаснее → серее»**: официальный экспорт/Takeout → ручное копирование → парсинг присланных тебе файлов → браузерная автоматизация своей сессии → RE-API. Кейсов энфорсмента против личных экспортёров своих данных не найдено ([no source found]) — но для Grok серый слой теперь запрещён буквой AUP.

## Дельта к Decision Memo 14.07

Memo: `[[decision-2026-07-14-personal-data-importers-grok-gemini-takeout]]` (BUILD Gemini по Takeout-рельсе + ARM Takeout-as-routine; DEFER Grok).

**Подтверждает:**
- Gemini по Takeout-рельсе — верное решение, путь «My Activity → Gemini Apps» и scheduled-export раз в 2 мес. подтверждены [established]; построенный `gemini_lib.py` едет по правильной рельсе.
- Grok: инкрементального официального API нет; xAI API истории не даёт — верно.
- Наш общий паттерн (инкремент + upsert по id + watermark + SQLite) = SOTA-консенсус, менять нечего.

**Добавляет/меняет:**
1. ⭐ **Grok: «отложить» → «дешёвый weekly-актор»**. Memo откладывал Grok целиком; DR даёт всё для лёгкой постройки без RE: известная структура `prod-grok-backend.json`, готовый парсер Owlock, паттерн «запросил → письмо → скачал» = наш же `takeout_pull`. Это ровно тот «полноценный запрос→скачай актор», условие для которого memo оставлял открытым.
2. ⭐ **Gemini nightly-надстройка**: memo о `gemini_webapi` не знал. Это опциональный P1-слой для «ChatGPT-parity» (в memo такой возможности не значилось вовсе).
3. ⚠️ **Новый риск полноты Takeout**: memo считал Takeout-рельсу «низкой ломкостью»; DR добавляет спор о трункации → в арминг cron добавить канарейку полноты (сверка с UI), не только freshness.
4. ⚠️ **Новый юридический факт**: AUP xAI (26.06.2026) прямо запрещает автоматизацию — любой будущий RE-слой Grok это осознанный серый шаг, фиксировать явным решением, не дефолтом.
5. **X-архив как источник Grok-данных** (memo не разбирал) — закрыт: раздельные истории.

## Рекомендуемая архитектура импортёра (АК-47)

Порядок постройки для нашего флота (рутины — на always-on узле, окно 00:30–04:30 Лиссабон):

- **P0 Gemini (уже в работе):** армить существующую связку — Google scheduled export раз в 2 мес. + Gmail-детект «export ready» (паттерн `takeout_pull.py`) + `gemini_lib.py`. Добавить канарейку полноты (п.3 дельты).
- **P0 Grok (новое, взамен «отложить»):** weekly-актор по образцу Takeout-рельсы: (1) разовый ручной экспорт → зафиксировать реальный layout; (2) парсер по мотивам Owlock (`prod-grok-backend.json` → MD + frontmatter, thinking в `<details>`); (3) SQLite `grok_conversations(id, content_hash, ...)`, локальная дельта; (4) триггер экспорта — Playwright-клик по Settings→Export на выделенном профиле хаба + IMAP-забор письма; (5) канарейки: `export_bytes>0`, `parsed >= prev*0.95`, «есть новое за 14 дней при живом использовании».
- **P1 Gemini nightly (опционально):** `gemini_webapi` overlay — `list_chats` → дельта → `read_chat`; pin версии, rate-limit ≤1 чат/с c backoff; 3 ночи подряд fail → авто-даунгрейд на Takeout-only + алерт.
- **P2 Grok nightly (по умолчанию ВЫКЛ):** `/rest/app-chat/*` cookie-рельса — только явным решением Антона (AUP-запрет), с человеческим темпом.
- **Сквозное:** сырые ZIP/JSON → `_originals` до любого парсинга; идемпотентный upsert по conversation_id + hash; frontmatter `source/conversation_id/created/updated/exported_at/content_hash`; метрики + heartbeat + exit-code-контракт как у `nightly_sync.py` (0/7/8); лестница фолбэков закодирована стейт-машиной, не «в голове». Не использовать: API-ключи для истории, X-архив как рельсу, закрытые расширения как единственную зависимость.

## Источник

- Прогон 1 (CLI): `E:\Obsidian\_originals\deep-research\DR26-07-15-HUB-01-grok-gemini-export-grokcli.md` (Grok Build CLI 0.2.106, grok-4.5 --effort high)
- Прогон 2 (web Heavy): `E:\Obsidian\_originals\deep-research\DR26-07-15-HUB-01-grok-gemini-export-grokweb-heavy.md` (581 источник, чат https://grok.com/c/2b53681e-d528-4b37-b217-99f4f076ac9b)
- Fanout-леджер: `_drafts/DR-FANOUT-DR26-07-15-HUB-01-grok-gemini-export.md`
- Реестр: [[_DR-Registry]] (статус collected)
- Мета-наблюдение по рельсе DR: web-Heavy нашёл существенно больше (в т.ч. `/rest/app-chat`-endpoints и структуру экспорта), CLI дал более дисциплинированные confidence-тэги и таблицу разногласий — пара «Heavy + CLI» как два прогона одного вендора реально ловит слепые зоны друг друга.

---

🧒 **Простыми словами:** мы спросили у двух роботов-разведчиков, как каждую ночь забирать твои разговоры с Grok и Gemini в твой второй мозг. Ответ: волшебной кнопки, как у ChatGPT, ни у кого нет. Зато у обоих есть честная «посылка по запросу»: у Grok — «пришлите мне мои данные» письмом, у Gemini — посылка Takeout от Google (мы её уже умеем ловить). План простой: ловим посылки регулярно и раскладываем только новое. Для Gemini есть ещё «чёрный ход» для ежедневной свежести — можно включить потом. Для Grok чёрный ход тоже нашли, но xAI недавно написал в правилах «роботам нельзя», поэтому его держим выключенным. И главное — робот обязан кричать, когда он тихо сломался: считаем посылки и проверяем контрольные чаты. Итог: Grok больше не «отложен» — его сборщика можно строить дёшево прямо сейчас.
