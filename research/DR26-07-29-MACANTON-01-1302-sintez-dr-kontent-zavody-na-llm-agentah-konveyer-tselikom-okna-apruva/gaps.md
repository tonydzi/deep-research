# DR26-07-29-MACANTON-01-1302 — raw report from gaps

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: Codex CLI (OpenAI, локальная рельса, web search) — целевой добор пробелов, которые Grok честно пометил «не нашёл»
topic: DR26-07-29-MACANTON-01-1302 — контент-заводы, добор пробелов
date_collected: 2026-07-31


note: "не полный отчёт по теме, а 8 точечных вопросов; Gemini в этот веер не попал — обе его рельсы мертвы (см. синтез)"
---

Ниже — только то, что удалось подтвердить первичными или достаточно близкими к первичным источниками. Дата проверки ссылок: 31 июля 2026 года.

## 1. Авто-апрув по таймауту: реальные последствия

**НЕ НАЙДЕНО:** публично описанных инцидентов, где контент был автоматически одобрен именно из-за истечения таймаута, с числами по отпискам, жалобам, удалённым/откаченным публикациям или ущербу репутации.

Близкие, но не закрывающие пробел источники:

- **ЗАЯВЛЕНО** — Sprout Social использует противоположное безопасное поведение: если публикацию не одобрили до назначенного времени, она не выходит и автоматически считается отклонённой. Дата публикации страницы не указана; проверено 31.07.2026. [Sprout Social Support](https://support.sproutsocial.com/hc/en-us/articles/38373940164877-Troubleshooting-Sprout-Social-Publishing-Calendar-Issues)

- **ЗАЯВЛЕНО** — OWASP Agentic Penetration Testing Standard прямо запрещает auto-approve после таймаута: действие должно перейти в `DENY`, `PAUSE` или `KILL`. Это нормативное требование, не статистика инцидентов. Дата публикации страницы не указана; проверено 31.07.2026. [OWASP APTS Human Oversight](https://owasp.org/APTS/standard/3_Human_Oversight/)

- **ЗАЯВЛЕНО** — n8n рекомендует отдельные ветки `approved`, `rejected`, `timed out`; по таймауту — эскалацию другому человеку, а не публикацию. Опубликовано приблизительно в марте 2026 года. [n8n Production AI Playbook](https://blog.n8n.io/production-ai-playbook-human-oversight/)

## 2. Reddit: апелляции и актуальные требования AutoModerator

### Официальная статистика апелляций

- **ИЗМЕРЕНО** — в январе–июне 2025 года Reddit получил 426 527 апелляций по контентным санкциям и 438 983 по санкциям аккаунтов. Средняя успешность апелляций — **38,7%**. Это объединённая статистика санкций; отдельной доли именно для shadowban Reddit не публикует. Опубликовано в 2025 году. [Reddit Transparency Report H1 2025](https://redditinc.com/hubfs/Reddit%20Inc/Content/Transparency%20Reports/RTR%20H1%202025/Reddit%20Transparency%20Report%20H1%202025%20.pdf?hsLang=en)

- **ИЗМЕРЕНО** — в июле–декабре 2025 года поступило 481 416 апелляций по контенту и 499 515 по аккаунтам. Reddit сообщил о снижении количества удовлетворённых апелляций на **66,6%** относительно предыдущего полугодия, но не напечатал итоговый общий процент успеха текстом. Опубликовано в 2026 году. [Reddit Transparency Report H2 2025](https://redditinc.com/policies/transparency-report-july-to-december-2025-reddit)

- **ИЗМЕРЕНО** — для сравнения: в январе–июне 2024 года средняя доля отменённых санкций составляла **20,8%**; во втором полугодии 2024 года — около **18,6%**. Опубликовано в 2024–2025 годах. [H1 2024](https://redditinc.com/policies/transparency-report-january-to-june-2024), [H2 2024 PDF](https://redditinc.com/hubfs/Reddit%20Inc/Content/Transparency%20Reports/RTR%20H2%202024/PDF/Reddit%20Transparency%20Report%20for%20July%20-%20December%202024.pdf?hsLang=en)

- **НЕ НАЙДЕНО:** официальной отдельной статистики успешности апелляций после shadowban, а также отдельного показателя только для постоянных банов.

### Примеры действующих числовых ограничений

Публичные конфигурации конкретных сабреддитов обычно закрыты; ниже — видимые правила и сообщения AutoModerator.

- **ЗАЯВЛЕНО** — r/csgomarketforum: аккаунт должен быть не моложе **21 дня**; сообщение также упоминает требование к comment karma в **20 или 40**, но доступный фрагмент неоднозначен. Актуальный пример опубликован в 2025 году. [Пример сообщения AutoModerator](https://us.reddit.com/r/csgomarketforum/comments/1o1o8e9/q_hows_the_genesis_terminal_price_trends/)

- **ЗАЯВЛЕНО** — r/NonCredibleDefense: не менее **100 combined karma** и возраст аккаунта не менее **4 месяцев**. Опубликовано в июле 2026 года. [Пример удаления](https://es.reddit.com/r/NonCredibleDefense/comments/1uwyoxl/made_a_post_ahead_of_the_world_cup_game_hope_yall/?sort=old)

- **ЗАЯВЛЕНО** — r/spotify: минимум **40 combined karma** и возраст аккаунта минимум **30 дней**. Опубликовано в июле 2026 года. [Пример сообщения AutoModerator](https://vi.reddit.com/r/spotify/comments/1ulzmu5/uncanny_indie_pop/?sort=top)

- **ЗАЯВЛЕНО** — r/Bellingham: аккаунт автора должен быть не моложе **30 дней**. Опубликовано в июле 2026 года. [Правила в выдаче Reddit](https://en.reddit.com/r/Bellingham/comments/1upev4t/rbellingham/)

- **ЗАЯВЛЕНО** — публичный рабочий пример конфигурации 2025 года: `account_age: "< 15 days"`, `comment_karma: "< 15"`; альтернативное правило — `comment_karma: < 75` и `combined_subreddit_karma: < 15`. Это пример модератора, а не официальное правило названного сабреддита. Опубликовано 15.07.2025. [r/AutoModerator](https://www.reddit.com/r/AutoModerator/comments/1m0mlmr/accounts_have_been_bypassing_automod_as_if_its/)

- **ЗАЯВЛЕНО** — другой пример 2025 года: возраст менее **30 дней** и comment karma менее **150** используются как отдельные основания удаления. Опубликовано 01.03.2025. [r/modhelp](https://www.reddit.com/r/modhelp/comments/1j0p158)

## 3. Официальные потолки органических публикаций

### Facebook Page

- **НЕ НАЙДЕНО:** официального фиксированного числа органических публикаций Page в сутки.

- **ЗАЯВЛЕНО** — Meta подтверждает наличие динамических ограничений по скорости и количеству действий, но прямо говорит, что подробные rate limits не раскрывает. Дата публикации страницы не указана; проверено 31.07.2026. [Facebook Help Center](https://www.facebook.com/help/101389386674555)

### X

- **ЗАЯВЛЕНО** — официальный предел аккаунта: **2 400 posts в сутки**, дополнительно разделённых на получасовые интервалы. В лимит входят reposts; это общий технический предел, а не отдельная норма специально для «органических» публикаций. Дата публикации страницы не указана; проверено 31.07.2026. [X Help: Understanding X limits](https://help.x.com/en/rules-and-policies/x-limits)

### Telegram-канал

- **НЕ НАЙДЕНО:** официального суточного потолка обычных публикаций в Telegram-канале.

- **НЕ НАЙДЕНО:** основания считать лимиты Bot API на скорость отправки сообщений официальным суточным лимитом органического канала. Это другой тип ограничения.

## 4. Аудитория сама распознала нераскрытый AI-контент

**НЕ НАЙДЕНО:** полевого исследования, где:

1. AI-авторство заранее не раскрывалось;
2. аудитория сама его обнаружила;
3. затем были измерены реальные отписки, удержание, жалобы или изменение репутации.

Близкие случаи не соответствуют критерию:

- **ИЗМЕРЕНО** — после внешнего расследования о нераскрытом AI-контенте CNET выяснилось, что из минимум **77** AI-материалов исправления потребовались **41**. Это число исправлений, но не замер доверия или отписок и не самостоятельное массовое распознавание читателями. Расследование относится к январю 2023 года. Сводка и ссылки на первичные материалы: [Automated journalism](https://en.wikipedia.org/wiki/Automated_journalism)

- **НЕ НАЙДЕНО:** у CNET публичных чисел по отпискам, отменам подписки или изменению доверия после раскрытия.

## 5. Рандомизированные сравнения формулировок AI-раскрытия

**НЕ НАЙДЕНО:** рандомизированного сравнения именно «сухая техническая плашка» против раскрытия в личном, человеческом тоне.

Ближайшие эксперименты:

- **ИЗМЕРЕНО** — национальный эксперимент в Южной Корее, **n=923**: без метки против “AI-assisted, editorially reviewed” и “AI-generated, editorially reviewed”. Это сравнение силы/содержания сухих меток, не сухого и личного тона. Первая версия опубликована 17.06.2026, обновлена 09.07.2026. [SSRN](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=7086474)

- **ИЗМЕРЕНО** — эксперимент **n=500** сравнивал отсутствие раскрытия, “assisted by AI” и “written by AI”, а также byline против endnote. Личного тона нет. Опубликовано в 2025 году. [Dialogue Social Science Review](https://dialoguesreview.com/index.php/2/article/view/1166)

- **ИЗМЕРЕНО** — международный эксперимент: **5 435 участников**, **43 512 оценок видео**, сравнение предварительного message-style warning и встроенной label-style disclosure в пяти странах. Это формат и размещение предупреждения, не личный голос автора. Опубликовано в 2026 году. [Computers in Human Behavior](https://www.sciencedirect.com/science/article/pii/S0747563226002104)

## 6. Рост частоты публикаций и падение метрик на пост

- **ИЗМЕРЕНО** — исследование Instagram использовало панельные данные создателей и fixed-effects regression. Найдена перевёрнутая U-зависимость: engagement сначала растёт с частотой публикаций, затем падает после пика; большое число followers, following и конкурентов усиливает wear-out. В доступном abstract нет числовой точки перелома и коэффициентов. Опубликовано в июле 2023 года. [Tafesse & Dayan, Journal of Retailing and Consumer Services](https://www.sciencedirect.com/science/article/pii/S0969698923001042)

- **ИЗМЕРЕНО**, но **без требуемого контроля** — выборочное исследование Instagram сообщало: переход с **0,5 до 1,5 поста/день** связан с падением engagement на пост на **19%**; с 1,5 до 2,5 — ещё на **9%**; до 3,5 — ещё на **6%**. При этом суммарный engagement в день рос: удвоение частоты давало примерно **1,61×** дневного engagement. Это поперечное сравнение брендов, не доказанная панельная причинность. Опубликовано в июне 2018 года. [Social Media Today](https://www.socialmediatoday.com/news/does-posting-more-often-on-instagram-reduce-reach-report/525082/)

- **НЕ НАЙДЕНО:** открытых панельных оценок, одновременно содержащих fixed effects/контроли, показатель именно «на пост» и опубликованную числовую точку, после которой дополнительный пост снижает метрику.

## 7. Ответ человека в чате останавливает таймер публикации

- **ЗАЯВЛЕНО** — n8n имеет готовые операции Slack, Gmail и Teams `send message and wait for response`. Выполнение workflow ставится на паузу; кнопка Approve/Reject или текстовый ответ возобновляет то же выполнение. Можно задать таймаут и после узла разделить три исхода: одобрено, отклонено, ответ отсутствует. Опубликовано приблизительно в марте 2026 года. [n8n Production AI Playbook](https://blog.n8n.io/production-ai-playbook-human-oversight/)

- **ЗАЯВЛЕНО** — технически ответ не «отменяет отдельный таймер»: approval-node находится в ожидающем состоянии, а человеческий callback завершает ожидание раньше deadline. Если ответ не пришёл, выход не содержит approval data и должен идти в отдельную ветку timeout. Тот же источник: [n8n](https://blog.n8n.io/production-ai-playbook-human-oversight/)

- **ЗАЯВЛЕНО** — готовый шаблон n8n маршрутизирует approval между Slack и Postgres и сохраняет состояние запроса. Дата публикации не указана; проверено 31.07.2026. [n8n workflow template](https://n8n.io/workflows/16663-route-human-in-the-loop-approval-requests-with-slack-and-postgres/)

- **ЗАЯВЛЕНО** — open-source Telegram-бот AffirmBot принимает/отклоняет предложенный контент и публикует только после одобрения. Таймаут автопубликации не описан. Дата публикации репозитория на доступной странице не указана. [GitHub: iwatkot/affirmbot](https://github.com/iwatkot/affirmbot)

- **ЗАЯВЛЕНО** — Sprout Social реализует fail-closed: если чат/интерфейс не дал одобрения до scheduled time, публикация не выходит. [Sprout Social](https://support.sproutsocial.com/hc/en-us/articles/205974715-Message-Approval-Workflows)

- **НЕ НАЙДЕНО:** публичного production-разбора, где свободный ответ человека в Slack/Telegram изменяет Google Doc и одновременно атомарно отменяет уже созданную задачу автопубликации, с описанием race-condition и гарантий exactly-once.

## 8. Google Docs API: атрибуция и пределы revision history

- **ЗАЯВЛЕНО** — ресурс Drive API `Revision` содержит `lastModifyingUser`, но поле заполняется только тогда, когда последнюю правку сделал **signed-in user**. Последнее обновление документации: 23.07.2025. [Drive API: Revision resource](https://developers.google.com/workspace/drive/api/reference/rest/v3/revisions)

- **ЗАЯВЛЕНО** — API не гарантирует запись каждой отдельной правки: revisions редакторов Docs/Sheets/Slides могут объединяться. Последнее обновление: 20.04.2026. [Changes and revisions overview](https://developers.google.com/workspace/drive/api/guides/change-overview)

- **ЗАЯВЛЕНО** — `revisions.list` может возвращать неполную историю для часто редактируемых Docs; старые ревизии могут отсутствовать, хотя UI показывает более полную историю. По умолчанию возвращается до **200** ревизий за страницу, максимум `pageSize=1000`, но это не гарантирует полноту. Документация обновлена в июле 2026 года. [revisions.list](https://developers.google.com/workspace/drive/api/reference/rest/v3/revisions/list)

- **ЗАЯВЛЕНО** — правило «обычно 30 дней или 100 ревизий» относится к purgeable revisions бинарных файлов. Для Google Docs нельзя переносить это как гарантированный срок хранения: Google отдельно предупреждает о слиянии и пропуске editor revisions в API. Документация обновлена 07.05.2026. [Manage file revisions](https://developers.google.com/workspace/drive/api/guides/manage-revisions)

- **ЗАЯВЛЕНО** — для бинарных файлов можно закрепить не более **200** revisions как `Keep Forever`; механизм не применяется к Google Docs editor files. [Revision resource](https://developers.google.com/workspace/drive/api/reference/rest/v3/revisions)

- **ЗАЯВЛЕНО** — практический разбор показывает, что `revisions.list` без явного `fields` может не вернуть `lastModifyingUser`; автор решения получает каждую revision через `revisions.get(..., fields="lastModifyingUser")`. Это Stack Overflow, не официальная гарантия. Вопрос опубликован 21.06.2013, решение обновлено 24.01.2019. [Stack Overflow](https://stackoverflow.com/questions/15190729/google-drive-api-revisions-list-and-get-methods-not-populating-lastmodifying)

- **НЕ НАЙДЕНО:** официальной гарантии, позволяющей по одной только revision history безошибочно классифицировать правку как «живой человек» против «автоматизации». `lastModifyingUser` идентифицирует signed-in principal, но не доказывает, кто инициировал действие; кроме того, ревизии могут сливаться или отсутствовать в API.

- **НЕ НАЙДЕНО:** официального Docs API, возвращающего построчный diff каждой правки вместе с неизменяемой идентичностью редактора. Drive Changes API прямо указывает, что change entry не содержит delta между revisions. Обновлено в июле 2026 года. [Track changes](https://developers.google.com/workspace/drive/api/guides/about-changes)

## 5 самых доказательных находок

1. **ИЗМЕРЕНО** — Reddit: успешность апелляций в H1 2025 составила **38,7%** при 865 510 апелляциях двух типов. [Источник](https://redditinc.com/hubfs/Reddit%20Inc/Content/Transparency%20Reports/RTR%20H1%202025/Reddit%20Transparency%20Report%20H1%202025%20.pdf?hsLang=en)

2. **ИЗМЕРЕНО** — панельная fixed-effects модель Instagram обнаружила перевёрнутую U-зависимость между частотой публикаций и engagement. [Источник](https://www.sciencedirect.com/science/article/pii/S0969698923001042)

3. **ИЗМЕРЕНО** — международный RCT AI-disclosure: 5 435 участников и 43 512 оценок видео в пяти странах. [Источник](https://www.sciencedirect.com/science/article/pii/S0747563226002104)

4. **ЗАЯВЛЕНО** — X официально публикует предел **2 400 posts/day**; Facebook скрывает числовые rate limits; у Telegram суточный предел канальных публикаций не найден. [X](https://help.x.com/en/rules-and-policies/x-limits), [Facebook](https://www.facebook.com/help/101389386674555)

5. **ЗАЯВЛЕНО** — Google предупреждает, что API-история часто редактируемых Docs может быть неполной, старые revisions могут пропускаться, а отдельные изменения — сливаться. [Manage revisions](https://developers.google.com/workspace/drive/api/guides/manage-revisions), [Changes overview](https://developers.google.com/workspace/drive/api/guides/change-overview)
