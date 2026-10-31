---
dr_id: DR26-10-22-MACANTON-02-1416
title: "DR26-10-22-MACANTON-02-1416: чужая дистрибуция sgm и путь к 1000 звёздам (синтез 4 ног)"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# DR26-10-22-MACANTON-02-1416: sgm, чужая дистрибуция и путь к 1000★

Синтез сделан рельсой codex по 4 отчётам (≈204 КБ), числа и ссылки только из отчётов. Ноги: grok (22.10), claudeai (Research, Fable 5.1 Max, 276 источников), mistral (Think, canvas), chatgpt (Pro Deep research, 116 ссылок). Gemini и GLM нет, кворум 4/6.
Сырьё ног: «внутренний архив лаборатории».

## 1. Где ноги согласны

**Принятие в чужие документы и рост звёзд нужно считать отдельно.** Все четыре отчёта не установили причинной связи «конкретное встраивание принесло столько-то звёзд». ChatGPT нашёл датированные включения и окна роста; Grok, ClaudeAI и Mistral подтверждают наличие интеграций, но преимущественно без сопоставимой истории. Открытая заявка также не равна принятому включению: показательный пример у ChatGPT, [заявка basic-memory в Cline](https://github.com/cline/mcp-marketplace/issues/92), остаётся открытой **(1 нога)**.

**Show HN полезен как эксперимент, но ненадёжен как основной план достижения 1000★.** Grok, Mistral и ChatGPT приводят [Daniel King](https://danfking.github.io/blog/2026/04/23/show-hn-by-the-numbers/): медиана по корпусу составляет **2 очка**. ClaudeAI добавляет [Sturdy Statistics](https://blog.sturdystatistics.com/posts/show_hn/): большинство публикаций получают мало внимания. Средние **121★ за сутки и 189★ за двое суток** из [Launch-Day Diffusion](https://arxiv.org/abs/2511.04453) нельзя переносить на неизвестный пакет: выборка отбирает уже замеченные проекты.

**Рабочая установка внутри клиента перспективнее простого упоминания.** Grok выделяет Cline и клиентские расширения; ClaudeAI и Mistral ставят высоко плагины Claude; ChatGPT добавляет Gemini CLI и адаптеры стандартных интерфейсов. Общий механизм: чужой пользователь получает готовый сценарий установки, а не только ссылку на репозиторий. Подтверждённые двери: [Gemini Extensions](https://geminicli.com/extensions/), [Claude plugin publishing](https://code.claude.com/docs/en/plugins/publish), [Cursor Marketplace](https://cursor.com/marketplace/publish).

**Нельзя путать собственную совместимость с чужой дистрибуцией.** Все ноги находят пакеты, которые сами написали интеграции. Это ещё не означает появления в официальных документах принимающей стороны. Для [LlamaIndex](https://github.com/run-llama/llama_index/blob/main/CONTRIBUTING.md) ClaudeAI и ChatGPT отдельно подтверждают закрытие прежнего пути внесения новых integration packages в монорепозиторий.

**Полной таблицы конверсии по каналам нет.** Ни одна нога не получила сопоставимые пары «уникальные посетители README из конкретного источника → новые звёзды этих посетителей». **12★ за жизнь и 34 посетителя за последние 14 дней не образуют конверсию**: периоды различаются.

## 2. Где расходятся и чьи доказательства сильнее

| Спор | Что говорят ноги | Вывод по представленным доказательствам |
|---|---|---|
| **Фреймворки закрыты для маленьких пакетов?** | Grok, ClaudeAI и Mistral преимущественно исключают LangChain/Pydantic AI. ChatGPT предъявляет принятые [M3Retriever](https://github.com/langchain-ai/docs/pull/4917), [CKGRetriever](https://github.com/langchain-ai/docs/pull/5193) и [pydantic-ai-chdb](https://github.com/pydantic/pydantic-ai/pull/6467). | **ChatGPT сильнее опровергает общее утверждение о закрытости.** Закрытый memory-backend intake не исключает retriever или capability. Все три прецедента **(1 нога)**; они доказывают возможность включения, не значительный рост. |
| **Как подаваться в Cline?** | Первые три ноги направляют issue в `cline/mcp-marketplace`. ChatGPT описывает новый [cline/marketplace](https://github.com/cline/marketplace): PR с `entry.json`. | У ChatGPT более конкретный альтернативный процесс **(1 нога)**. Однако существование нового registry само по себе не доказывает отмену старого. Перед подачей нужно установить актуальную дверь, а не отправлять обе заявки. |
| **awesome-claude-code: PR или форма?** | Mistral советует прямой PR. Grok и ChatGPT указывают web issue form. | **Grok и ChatGPT убедительнее:** ссылаются на [CONTRIBUTING](https://github.com/hesreallyhim/awesome-claude-code/blob/main/CONTRIBUTING.md); ChatGPT восстанавливает цепочку issue → автоматический PR. |
| **Anthropic community: прямой PR?** | Mistral предлагает PR в community-репозиторий. ClaudeAI говорит об автоматическом закрытии PR; ChatGPT разделяет public directory и curated official marketplace. | **Следовать порталу [публикации](https://code.claude.com/docs/en/plugins/publish).** Подать плагин в public directory не означает попасть в официальный curated marketplace. |
| **Awesome-списки: главный двигатель или почти бесполезны?** | Grok делает их основным следующим шагом. Mistral приводит [rag-kb](https://github.com/punkpeye/awesome-mcp-servers/pull/13117): принятие при создании, затем лишь **4★** **(1 нога)**. ChatGPT находит слабые изменения темпа после включений. | Доказано, что включения доступны; **не доказано, что они обычно дают существенные звёзды**. Но один слабый кейс также не доказывает нулевую ценность всего канала. Вероятность принятия «~100%» у Mistral необоснованна. |
| **Можно планировать через 1,4★ на очко HN?** | Mistral экстраполирует коэффициент на слабые посты. Grok и ChatGPT подчёркивают отбор **491 репозитория с 258+ очками** у King. | **Правы Grok и ChatGPT:** коэффициент успешных запусков не прогноз для поста с несколькими очками. |
| **История звёзд недоступна?** | Mistral заявляет невозможность восстановления. ChatGPT предоставляет дневные [history-ряды](https://api.github.com/repos/skynetcmd/m3-memory/stargazers/history?per_page=30&page=1) и архив индивидуальных звёзд. | **ChatGPT предъявляет более воспроизводимый путь (1 нога).** Ограничения остаются: сохранившиеся звёзды отличаются от исторического чистого прироста; дневные суммы не дают точного скользящего окна. |
| **Какая конверсия нормальна?** | Mistral предлагает **3–8%**, ClaudeAI приводит **4–20%**. Grok и ChatGPT подчёркивают разные знаменатели и периоды. | **Единую норму выводить нельзя.** Отношение всех звёзд basic-memory к просмотрам собственных документов не измеряет конверсию посетителей из чужих доков. |
| **Copilot for Obsidian уже подходит?** | Mistral ссылается на MCP-коммиты; ClaudeAI на [неработающий проброс MCP в Agent Mode](https://github.com/logancyang/obsidian-copilot/issues/3698); Grok и ChatGPT не находят открытого списка внешней памяти. | Коммиты не доказывают работоспособность нужного сценария. Интеграция перспективна тематически, но пока условна. |

Числа звёзд из разных дат, например **691/693★** у TeleAI, не следует объявлять противоречием без одинакового времени снимка. Аналогично разные значения роста claude-mem за разные окна не устанавливают ошибку.

## 3. Топ-10 мест встраивания

Рейтинг качественный: фактических вероятностей принятия и посещаемости нужных страниц никто не измерил. Звёзды принимающего репозитория показывают масштаб проекта, а не гарантированный охват карточки.

| № | Место | Почему здесь | Первый шаг |
|---|---|---|---|
| 1 | [Gemini CLI Gallery](https://geminicli.com/extensions/) | Grok и ChatGPT: документированное автообнаружение снижает зависимость от очереди ревью. | Подготовить проверенное расширение, корневой `gemini-extension.json` и topic `gemini-cli-extension`; проверить появление карточки. |
| 2 | [LangChain Retrievers](https://docs.langchain.com/oss/python/integrations/retrievers) | Принятые малые graph/memory-адаптеры **(1 нога, ChatGPT)** убедительнее предположений о закрытой двери. | Сделать тонкий `BaseRetriever`, возвращающий документы и происхождение; затем Integration listing issue по [текущим правилам](https://docs.langchain.com/oss/python/contributing/publish-langchain). |
| 3 | [Claude public plugin directory](https://code.claude.com/docs/en/plugins/publish) | ClaudeAI, Mistral, ChatGPT: установка внутри привычного клиента. Приём конкретного малого аналога не установлен. | Упаковать MCP и сценарий remember/recall в plugin; проверить установку и подать через портал. |
| 4 | [Pydantic AI Third-Party Capabilities](https://pydantic.dev/docs/ai/capabilities/third-party/) | Открытый путь и принятый малый пакет **(1 нога, ChatGPT)**. | Подготовить типизированную capability поиска связанных заметок и runnable-пример; предложить PR именно сюда. |
| 5 | [Cline Marketplace](https://github.com/cline/marketplace) | Все ноги считают клиент релевантным; очереди снижают ожидаемую скорость результата. Новый registry **(1 нога)**. | Проверить действующий процесс; подготовить точные аргументы установки и проверяемую запись каталога. |
| 6 | [Cursor Marketplace](https://cursor.com/marketplace/publish) | Формальный маршрут review подтверждают несколько ног. | Подготовить plugin manifest, конфигурацию MCP и воспроизводимый workflow; отправить репозиторий через форму. |
| 7 | [OpenWebUI tutorials](https://docs.openwebui.com/tutorials/) | Mistral и ChatGPT находят community-маршруты; tutorial ближе к цели «строка в чужих доках». | Проверить рецепт связанные Markdown-заметки → SGM → MCPO → OpenWebUI; оформить по [правилам tutorial](https://docs.openwebui.com/tutorials/contributing-tutorial/). |
| 8 | [punkpeye: Knowledge & Memory](https://github.com/punkpeye/awesome-mcp-servers) | Доступный формат и большой список; прямой звёздный эффект не установлен. | Прочитать текущие требования, выполнить проверки Glama, если обязательны; предложить одну фактическую строку. |
| 9 | [TeleAI Awesome-Agent-Memory](https://github.com/TeleAI-UAGI/Awesome-Agent-Memory) | Grok и Mistral подтверждают тематическое соответствие. Emerging для малых проектов описан Grok **(1 нога)**. | Подготовить заявку в Emerging по [CONTRIBUTING](https://github.com/TeleAI-UAGI/Awesome-Agent-Memory/blob/main/CONTRIBUTING.md), без маркетинговых обещаний. |
| 10 | [mnemoverse: сравнение memory MCP](https://mnemoverse.com/docs/library/memory-mcp-servers-compared) | ClaudeAI находит включение очень малых проектов **(1 нога)**; точная аудитория важнее размера общего каталога. | Предложить редактору работающий рецепт и проверяемую таблицу отличий; не ограничиваться просьбой добавить ссылку. |

## 4. Чего не нашёл никто

- **Причинного эффекта конкретного встраивания**: сколько пользователей и звёзд оно принесло отдельно от релизов, рекламы и других публикаций.
- **Прогноза пути именно SGM к 1000★** с проверенной конверсией, стоимостью поддержки и сроком.
- **Сопоставимой конверсии README → звезда по каналам**, включая чужие документы, awesome-списки, GitHub Search, X и YouTube.
- **Репрезентативной выборки запусков авторов без аудитории.** Малый репозиторий не доказывает отсутствия подписчиков или команды.
- **Посещаемости конкретных страниц и доли принятых заявок**, необходимых для численного рейтинга «вероятность × охват».

ChatGPT частично закрыл пробел исторических окон звёзд **(1 нога)**, но это наблюдение рядом с событием, не атрибуция.

## 5. Пять действий на ближайшие 7 дней

1. **Подготовить один проверяемый сценарий:** связанные Markdown-заметки, запрос с переходом по `wikilinks`, ответ с источниками. Проверить чистую установку и точно описать зависимости поиска: ChatGPT отмечает необходимость extras `[embeddings]` для полного сценария **(1 нога)**.

2. **Довести Gemini-расширение до публичной карточки.** Зафиксировать появление и работоспособность установки; отправленный manifest результатом не считать.

3. **Сделать LangChain retriever и подать listing issue.** Использовать прецеденты M3Retriever/CKGRetriever; не начинать с PR в закрытый общий пакет.

4. **Собрать Claude plugin и подготовить Cline-заявку.** Использовать общий проверенный MCP-сценарий, но отдельные форматы площадок; перед подачей разрешить расхождение старого и нового процесса Cline.

5. **Начать измерение и подать точечные заявки в punkpeye и TeleAI.** Сохранять трафик/referrers, звёзды, PyPI и даты внешних событий. Критерии недели: принятая карточка, рабочая чужая установка и первый подтверждённый пользователь. Show HN готовить вокруг этого сценария, без обещания заданного числа звёзд.


## Check it yourself: prompt, sources, raw reports

- Prompt: [DR26-10-22-MACANTON-02-1416-dr26-10-22-macanton-02-1416-chuzhaya-distributsiya-sgm-i-put-k-1000-zv/prompt.md](DR26-10-22-MACANTON-02-1416-dr26-10-22-macanton-02-1416-chuzhaya-distributsiya-sgm-i-put-k-1000-zv/prompt.md)
- All sources (304 links): [DR26-10-22-MACANTON-02-1416-dr26-10-22-macanton-02-1416-chuzhaya-distributsiya-sgm-i-put-k-1000-zv/sources.md](DR26-10-22-MACANTON-02-1416-dr26-10-22-macanton-02-1416-chuzhaya-distributsiya-sgm-i-put-k-1000-zv/sources.md)

## Raw model reports

4 reports delivered, synthesis used 1 of 6. Note: 3 report(s) arrived after the synthesis was written; they are attached raw and were not reconciled. [DR26-10-22-MACANTON-02-1416-dr26-10-22-macanton-02-1416-chuzhaya-distributsiya-sgm-i-put-k-1000-zv/](DR26-10-22-MACANTON-02-1416-dr26-10-22-macanton-02-1416-chuzhaya-distributsiya-sgm-i-put-k-1000-zv/README.md)
