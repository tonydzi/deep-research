# DR26-10-22-MACANTON-02-1416 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Как `sqlite-graph-memory` получить первые внешние встраивания: результаты исследования

**Дата проверки — 23 октября 2026 года.** События рассматриваются относительно даты запроса, 22 октября; текущие счётчики GitHub помечены как снимок на 23 октября.

**Для SGM существуют открытые пути в чужую документацию, включая проекты, которые принимают совсем маленькие пакеты. Но найденные данные не подтверждают, что само включение обычно приносит существенный рост звёзд.** Самые близкие примеры — `m3-memory`, `langchain-ckg` и `pydantic-ai-chdb`: их действительно приняли в официальные документы LangChain/Pydantic AI в июле–августе 2026 года. Сейчас у них соответственно **29, 0 и 2★**. За две недели после принятия в доступной истории сохранилось **5, 0 и 0 новых звёзд**. Ниже — даты, PR, участники и расчёты.

Для Show HN удалось восстановить **20 точных 48-часовых окон** по опубликованному архиву отдельных звёзд. **Подтверждённую выборку из 20 авторов именно без аудитории не нашёл**: малый репозиторий не доказывает отсутствия подписчиков, команды или параллельного продвижения.

Числовые таблицы, сохранённые ответы API и ссылки для воспроизведения: **[sgm_research_data_2026-10-23.zip](sandbox:/workspace/scratch/4d420af85346/sgm_research_data_2026-10-23.zip)**.

## Как читать числа

В отчёте разделены три результата:

- **«Сработало включение»** — чужой maintainer принял PR, рецепт или плагин.
- **«Наблюдается рост»** — после события появились звёзды или загрузки.
- **«Встраивание принесло N★»** — причинная атрибуция. Такого надёжно установленного числа для рассмотренных встраиваний **не нашёл**.

Обозначение **S14** ниже — число звёзд, поставленных в 14 полных UTC-сутках до или после события и **сохранившихся на дату проверки**. День события исключён. Это не точный исторический чистый прирост: GitHub удаляет запись после снятия звезды. Новый дневной API, используемый Star History, описан GitHub **14.10.2026**; поэтому восстановление доступно, но с этим ограничением. genui{"citation":{"refs":["turn57view3","turn47view0"]}}

---

# (а) Куда встроиться

## 1. Площадки с подтверждённым механизмом приёма

**Все текущие ★ в таблице — на 23.10.2026. Это масштаб репозитория, а не посещаемость нужной страницы.** Просмотры страниц, число переходов с них и долю принятых заявок для перечисленных площадок **не нашёл**.

Возраст означает возраст конкретной страницы или подтверждённую нижнюю границу её существования. Возраст всего репозитория вместо него не подставляю.

| Имя и URL списка | Дата / возраст страницы | Число: видимый масштаб | Как принимают; условия и причины отказа | Маленькие внешние примеры / вывод |
|---|---|---:|---|---|
| **LangChain — [Retriever integrations](https://docs.langchain.com/oss/python/integrations/retrievers)** | Проверено 23.10.2026; точный возраст страницы **не нашёл** | **148,3 тыс.★** у LangChain | Собственный пакет, совместимый интерфейс и документация. Сейчас путь: **Integration listing issue → maintainer `integration-run` → автоматический listing**. Полный hosted guide: **≥50 тыс. загрузок/месяц либо featured-исключение**. Ручной listing PR могут автоматически закрыть. Retrievers/tools поощряются; graph/KV/message-history интеграции — менее желательные категории. [Правила](https://docs.langchain.com/oss/python/contributing/publish-langchain). | **M3Retriever и CKGRetriever приняты**, точные даты ниже. Для SGM это один из наиболее конкретных путей. |
| **Pydantic AI — [Third-Party Capabilities](https://pydantic.dev/docs/ai/capabilities/third-party/)** | Отдельная страница создана **21.07.2026**: **3 месяца 1 день** на дату запроса. До этого раздел находился в другом файле. [PR переноса](https://github.com/pydantic/pydantic-ai/pull/6621) | **20,8 тыс.★** | Страница прямо приглашает **PR с внешним capability-пакетом**. Нужна совместимая capability, а не просто ссылка на MCP-сервер. Обязательный минимум ★/загрузок **не нашёл**. Дублирование имеющейся функции может ослабить заявку — это моя оценка, не опубликованный числовой порог. | **`pydantic-ai-chdb`, 2★**, принят 14.07.2026. **Сработало включение**. |
| **Google Gemini CLI — [Extensions Gallery](https://geminicli.com/extensions/)** | Проверено 23.10.2026; дату рождения gallery **не нашёл** | **107,5 тыс.★** у Gemini CLI; обход **ежедневный** | Публичный GitHub repo, topic **`gemini-cli-extension`**, валидный корневой **`gemini-extension.json`**. Автоиндексация после validation; отдельная заявка не требуется. Неверный manifest, его расположение или отсутствующий topic препятствуют появлению. [Правила](https://geminicli.com/docs/extensions/releasing/). | Пример «чужой пакет <500★ + точная дата появления» **не нашёл**. Формальный доступ открыт; эффект **не знаю**. |
| **Cursor — [Marketplace](https://cursor.com/marketplace), [подача](https://cursor.com/marketplace/publish)** | Проверено 23.10.2026; возраст страницы **не нашёл** | Посещения/установки каталога **не нашёл** | Public Git repo → форма → review команды Cursor. Валидный `plugin.json` либо `.cursor-plugin/plugin.json`, README, объявленные переменные, корректные относительные пути, локальная проверка. Невалидный manifest и несоответствие checklist мешают приёму. [Правила](https://cursor.com/docs/reference/plugins). | Датированный принятый маленький memory-плагин **не нашёл**. **Не знаю**. |
| **OpenWebUI — [Community tutorials](https://docs.openwebui.com/tutorials/)** | Проверено 23.10.2026; возраст страницы **не нашёл** | **156,5 тыс.★** | **PR с воспроизводимым tutorial** в документацию; сборка/проверка и маркировка community-supported. Для stdio MCP нужен соответствующий мост, например [MCPO](https://docs.openwebui.com/features/extensibility/plugin/tools/openapi-servers/mcp/). [Правила tutorial](https://docs.openwebui.com/tutorials/contributing-tutorial/). | Есть внешний [MCP Notion recipe](https://docs.openwebui.com/tutorials/integrations/mcp-notion). Его малый размер и дата добавления **не найдены**. Для SGM — возможный рецепт, результат **не знаю**. |
| **davila7 — [Claude Code Templates / aitmpl](https://www.aitmpl.com/)** | Процесс подтверждается [PR №782 от 05.08.2026](https://github.com/davila7/claude-code-templates/pull/782); это нижняя граница, не дата создания каталога | **32,8 тыс.★**, **334 открытых PR** | **PR с MCP JSON** в `cli-tool/components/mcps/<category>/`. Есть установка через CLI проекта. Нужны правильный формат, описание, конфигурация и проверка реального сценария. [CONTRIBUTING](https://github.com/davila7/claude-code-templates/blob/main/CONTRIBUTING.md). | Датированный **merged** пример чужого <500★ MCP **не нашёл**. Открытый PR не считаю включением. **Не знаю**. |
| **Cline — [новый Marketplace](https://github.com/cline/marketplace)** | Проверено 23.10.2026; точный возраст страницы **не нашёл** | **70,8 тыс.★** у клиента; у нового registry **8★**, около **290 открытых PR** | **PR** с `registry/mcps/<slug>/entry.json`; точные `install.args`, переменные отдельно, `npm run validate`. Merge публикует каталог. [CONTRIBUTING](https://github.com/cline/marketplace/blob/main/CONTRIBUTING.md). | Датированный принятый маленький memory-сервер **не нашёл**. Новый процесс существует; вероятность конкретного принятия **не знаю**. |
| **Agno — [Memory integration examples](https://docs.agno.com/examples/integrations/memory/mem0-integration)** | Проверено 23.10.2026; возраст страницы **не нашёл** | **42,9 тыс.★** | **Сначала issue**, затем maintainer должен поставить **`ready`**. Внешний PR без такого issue автоматически закрывается. Для SGM уместен конкретный cookbook. [Правила](https://github.com/agno-agi/agno/blob/main/CONTRIBUTING.md). | Mem0 уже имеет рецепт со строкой установки, но его размер <500★ при включении **не подтверждён**. **Не знаю**. |
| **hesreallyhim — [awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code)** | Заявка memory-проекта существует с **22.10.2025**: список жил как минимум **год** к дате запроса | **56,3 тыс.★** | **Web issue form**, не обычный внешний PR. Ресурс должен иметь **≥14 дней истории и продолжающуюся разработку либо ≥100★**. Не более одной рекомендации одновременно. Review не гарантируется. [Правила](https://github.com/hesreallyhim/awesome-claude-code/blob/main/CONTRIBUTING.md). | **claude-mem принят 25.10.2025** после заявки 22.10.2025. **Сработало включение**, эффект отдельно ниже. |
| **Zed — [Extensions](https://zed.dev/extensions)** | Проверено 23.10.2026; возраст каталога **не нашёл** | **1,9 тыс.★** у extensions repo | Git submodule + запись расширения + PR; для MCP нужен wrapper, проверенный commit, лицензия и соответствие требованиям. Однако **MCP extensions планируют заменить MCP Registry**. [Требования](https://zed.dev/docs/extensions/publishing/prerequisites). | Маленький memory-wrapper с датой принятия **не нашёл**. **Условный маршрут**: сначала проверить состояние перехода. |

Ключевые правила LangChain, Pydantic, Gemini и Agno подтверждаются текущими инструкциями площадок. **Исторические PR июля–августа не следует механически повторять сегодня:** нынешний процесс LangChain уже направляет небольшие интеграции через issue и автоматизацию. genui{"citation":{"refs":["turn33view1","turn33view2","turn93view6","turn93view0","turn93view3"]}}

### Три точных примера принятия маленьких чужих пакетов

Здесь удалось найти не только наличие ссылки, но и дату, подателя и принимающего maintainer.

| Имя / URL | Дата: заявка → принятие | Кто подал → кто принял | Число на 23.10.2026 | Вывод |
|---|---|---|---:|---|
| **[m3-memory / M3Retriever](https://github.com/skynetcmd/m3-memory)**, [LangChain PR №4917](https://github.com/langchain-ai/docs/pull/4917) | **15.07.2026 → 30.07.2026**, около **15 дней** | `skynetcmd` → `npentrel` | **29★** | **Сработало включение**. Внесены совместимые интерфейсы и внешняя документация. |
| **[langchain-ckg / CKGRetriever](https://github.com/Yarmoluk/langchain-ckg)**, [LangChain PR №5193](https://github.com/langchain-ai/docs/pull/5193) | **29.07.2026 → 04.08.2026**, около **6 дней** | `Yarmoluk` → `npentrel` | **0★** | **Сработало включение** графового `BaseRetriever`, работающего через MCP. |
| **[pydantic-ai-chdb](https://github.com/chdb-io/pydantic-ai-chdb)**, [Pydantic PR №6467](https://github.com/pydantic/pydantic-ai/pull/6467) | **14.07.2026**, около **8 ч 55 мин** от заявки до merge | `ShawnChen-Sirius` → `hramezani` | **2★** | **Сработало включение**. Это маленькая обёртка, поддерживаемая командой chDB/ClickHouse; не аналог одиночного автора без аудитории. |

Нынешние счётчики подтверждены страницами репозиториев. **Точное число ★ именно на дату принятия не нашёл**; текущие 29/0/2 нельзя выдавать за исторический снимок. genui{"citation":{"refs":["turn94view3","turn94view4","turn94view5"]}}

Особенно близок SGM пример **CKGRetriever**: принимающей стороне предложили графовый поиск через стандартный интерфейс, опубликованный пакет, лицензию и документацию. То есть доказана возможность включить небольшой graph/MCP-пакет в официальный список retrievers. **Просто наличие MCP-сервера само по себе таким адаптером не является.**

## 2. Проверенные площадки, где простого открытого пути не обнаружено

Для строк без отдельной исторической даты состояние проверено **23.10.2026**. Возраст конкретного внешнего списка, если он вообще существует, **не нашёл**.

| Имя / URL | Дата и число | Что найдено о приёме / отказе | Вывод |
|---|---|---|---|
| **[Smart Connections](https://github.com/brianpetro/obsidian-smart-connections)**, Brian Petro | 23.10.2026; **5,5 тыс.★** | Собственные embeddings/search. Открытый список внешних memory-backends с правилами подачи **не нашёл**. Smart Connect того же автора не является примером принятия независимого чужого пакета. | **Не знаю**; совместимость и внешняя дистрибуция здесь не доказаны. |
| **[Copilot for Obsidian](https://github.com/logancyang/obsidian-copilot)**, Logan Yang | 23.10.2026; **7,9 тыс.★** | Работа с агентами и собственным поиском; открытый intake внешних memory-backends **не нашёл**. | **Не знаю**. Нужен согласованный сценарий интеграции. |
| **[Text Generator](https://github.com/nhaouari/obsidian-textgenerator-plugin)** | 23.10.2026; **2,0 тыс.★** | Есть LLM providers и шаблоны. Это не подтверждает сменяемый memory-backend. Действующий приём внешнего MCP/memory-пакета **не нашёл**. | **Не знаю**. |
| **[LangGraph memory](https://docs.langchain.com/oss/python/langgraph/add-memory)** | 23.10.2026; просмотры страницы **не нашёл** | Документация описывает memory/storage/checkpoint interfaces. SGM retrieval нельзя автоматически считать checkpoint-backend. Отдельный открытый intake на эту страницу **не нашёл**. | **Не знаю**; начинать логичнее с настоящего LangChain retriever. |
| **[CrewAI Memory](https://docs.crewai.com/en/concepts/memory)** | 23.10.2026; **60,0 тыс.★** | Текущая unified Memory отличается от прежней модели short/long/entity/external. Открытый актуальный список сторонних backends с понятным приёмом **не нашёл**. | **Не знаю**; старый Mem0-рецепт не доказывает готовый путь сейчас. |
| **[LlamaIndex](https://github.com/run-llama/llama_index/blob/main/CONTRIBUTING.md)** | 23.10.2026; **52,6 тыс.★** | **Новые integration packages в монорепозиторий больше не принимают**; новый `pyproject.toml` → автоматическое закрытие. Свой пакет разрешён, но внешнее размещение в документации от этого не возникает. | **Не сработает прежний маршрут package PR**. |
| **[LibreChat MCP](https://www.librechat.ai/docs/features/mcp)** | 23.10.2026; **46,2 тыс.★** | Можно подключать MCP через конфигурацию. Открытый глобальный curated memory-list с правилами подачи **не нашёл**. Каталог внутри пользовательской установки — другая сущность. | **Не знаю** о канале дистрибуции. |
| **[Goose extensions](https://goose-docs.ai/extensions/)** | **30.07.2026** прекращён приём новых записей; сайт заявляет **70+ extensions** | Maintainer **jamadeo** объявил переход к official MCP Registry. Новые directory PR и tutorials только ради listing не принимаются. [Объявление](https://github.com/aaif-goose/goose/discussions/10830). | **Не сработает новый catalog PR**. SGM уже выполнил Registry-шаг по вашим данным. |
| **[Continue](https://continue.dev/), [репозиторий](https://github.com/continuedev/continue)** | 23.10.2026; **36,3 тыс.★** | Компания присоединилась к Cursor; репозиторий больше активно не поддерживается и read-only. Действующий приём в старый Hub не подтверждён. | **Не сработало как обоснованный новый приоритет**. |
| **[Claude public plugin directory](https://code.claude.com/docs/en/plugins/publish)** | 23.10.2026; число установок **не нашёл** | Есть submission/review через directory portal; требуется paid Claude account. Это отдельный путь от curated official marketplace. | **Не знаю**; маленький принятый memory-плагин с датой **не нашёл**. |
| **[Anthropic official marketplaces](https://code.claude.com/docs/en/plugins/anthropic-marketplaces)** | 23.10.2026; посещаемость **не нашёл** | Подача в public directory не означает попадания в `claude-plugins-official`; инструкции для official curated пути направляют к partner contact. | **Не знаю**; не считаю открытой PR-дверью. |
| **[Codex public directory](https://developers.openai.com/plugins/deploy/submission)** | 23.10.2026; посещаемость **не нашёл** | Для **локального stdio MCP** публичная подача сейчас ограничена **approved partners / allowlist организаций**. [Условия](https://developers.openai.com/plugins/deploy/desktop-only). Git-marketplace поддерживается, но создание собственного не даёт чужой аудитории. | **Условный доступ**, не быстрый путь для SGM. |
| **[awesome-obsidian](https://github.com/kmaasrud/awesome-obsidian#other)** | 23.10.2026; **9,5 тыс.★**, **54 открытых PR** | Формально принимает external tools через PR. Но близкие заявки — [Seekstone MCP №112, 07.06.2026](https://github.com/kmaasrud/awesome-obsidian/pull/112), [basemind №118, 01.07.2026](https://github.com/kmaasrud/awesome-obsidian/pull/118) — находятся в открытой очереди. | **Принятие не подтверждено**; не рассчитывал бы на быстрый результат. |
| **[awesome-brains](https://github.com/lakshyabuilds/awesome-brains)** | 23.10.2026; **5★**, **11 commits** | Тематически подходит, но правила и наблюдаемый масштаб не позволяют ожидать заметную дистрибуцию. Датированные успешные малые включения **не нашёл**. | **Не знаю**, низкий приоритет по охвату. |

Изменения Goose, Continue, LlamaIndex и ограничения Codex подтверждены текущими первичными источниками; это существенные причины не воспроизводить рекомендации из старых подборок. genui{"citation":{"refs":["turn33view0","turn47view2","turn47view3","turn93view4","turn59view0"]}}

---

# (б) Как проекты памяти получили внешнее встраивание

## 1. Кто внёс, когда и каким способом

**«Первое вообще» для большинства проектов доказать не удалось.** Ниже — наиболее ранние или наиболее чётко датированные внешние включения, которые удалось проверить. Исторические события 2023–2024 годов оставлены там, где без них ответ о первом встраивании был бы неверным.

| Имя | URL и дата внешнего события | Кто и как внёс / как нашли | Число и вывод |
|---|---|---|---|
| **basic-memory** | [Cline issue №92](https://github.com/cline/mcp-marketplace/issues/92), **16.03.2025**; [awesome-mcp-servers PR №7817](https://github.com/punkpeye/awesome-mcp-servers/pull/7817), принят **11.06.2026** | В Cline подал **phernandez**. В awesome внёс **groksrc**; приглашение и его связь с автором **не нашёл**. | Cline issue остаётся **открытым** — принятие не подтверждено. Awesome: **сработало включение**, но заявка уже описывала проект с **3 тыс.+★**; это не доказательство пути 12→1000. |
| **mcp-memory-service** | [awesome-machine-learning PR №1063](https://github.com/josephmisiti/awesome-machine-learning/pull/1063): **15.08.2025 → 06.10.2025** | Сам автор **Heinrich Krupp / doobidoo** предложил пакет и объяснил его ML-полезность. Приглашение **не нашёл**. | Около **52 дней** ожидания. **Сработало включение**, ускорения звёзд в проверенном окне нет. |
| **cognee** | [MotherDuck recipe](https://motherduck.com/blog/streamlining-ai-agents-duckdb-rag-solutions/), **29.04.2024**; [dlt](https://dlthub.com/blog/cognee), **13.11.2024**; [Memgraph ai-demos PR №25](https://github.com/memgraph/ai-demos/pull/25), **23.05.2025** | Статьи написал автор **Vasilije Markovic**. Пример Memgraph внёс **matea16**: загрузка текста, построение графа, поиск. Кто кого позвал — **не нашёл**. | **Сработало внешнее размещение**, включая настоящий кодовый пример. Звёзды, причинно принесённые каждым материалом, **не знаю**. |
| **Letta / MemGPT** | [AutoGen issue №156](https://github.com/microsoft/autogen/issues/156#issuecomment-1762504744), **14.10.2023** → [PR №803](https://github.com/microsoft/autogen/pull/803), **30.11.2023** → [PR №838](https://github.com/microsoft/autogen/pull/838), **01.12.2023** | Пользователь **kazunator** предложил MemGPT в AutoGen. **tomsib2001** принёс обратную ссылку в [MemGPT issue №20](https://github.com/letta-ai/letta/issues/20). **skzhang1** добавил ecosystem-документацию; **Charles Packer / cpacker** уточнил её. | **Сработало включение**. Это конкретная цепочка обнаружения сообществом. Личное приглашение и 14-дневный эффект **не нашёл**. |
| **mem0** | [LlamaIndex PR №16708](https://github.com/run-llama/llama_index/pull/16708): **26.10.2024 → 31.10.2024** | **Mayank / spike-spiegel-21** принёс пакет `llama-index-memory-mem0`, адаптер и notebook. Ревью — **Logan Markewich**, **Deshraj**. Историю знакомства **не нашёл**. | **Сработала нативная интеграция** со строкой `pip install llama-index-memory-mem0`. Это исторический маршрут, который LlamaIndex сегодня закрыл для новых пакетов. |
| **Graphiti** | [Neo4j Developer Blog](https://neo4j.com/blog/developer/graphiti-knowledge-graph-memory/), **24.03.2025** | Материал написал **Daniel Chalef**, основатель/CEO Zep AI. Кто пригласил — **не нашёл**. | **Сработало внешнее техническое размещение**. После него звёзд больше, но всплеск начался раньше; причинность неизвестна. |
| **memU** | [minhlucvan/claude-memu](https://github.com/minhlucvan/claude-memu), [переход на memU](https://github.com/minhlucvan/claude-memu/commit/c2266ff8b68200ac3afebdaf270c9c60702f4a43), **05.02.2026** | Внешний владелец **minhlucvan**. Приглашение/историю знакомства **не нашёл**. Более старая скопированная история claude-mem не является датой memU-интеграции. | Внешнее использование **есть**, но принимающий плагин имеет **1★** на 23.10.2026. Доказанного заметного охвата нет. |
| **claude-mem** | [awesome-claude-code issue №250](https://github.com/hesreallyhim/awesome-claude-code/issues/250), **22.10.2025** → [PR №262](https://github.com/hesreallyhim/awesome-claude-code/pull/262), **25.10.2025** | **Alex Newman / thedotmack** сам подал заявку; **hesreallyhim** проверил категорию и платные зависимости; бот создал PR. Приглашения в цепочке нет. | **Сработало включение за 3 дня**. Но автор сообщил о **120+★ за день 24 октября**, ещё **до merge**. |
| **OpenMemory от Mem0** | [SDK changelog](https://docs.mem0.ai/changelog/sdk), **07.05.2025**, v0.1.99 | Найден собственный релиз внутри экосистемы Mem0. Надёжно идентифицированное первое чужое встраивание **не нашёл**. | **Не знаю**. Общие звёзды монорепозитория Mem0 нельзя целиком приписать OpenMemory. |
| **OpenMemory от CaviraOSS** | [Отдельный репозиторий](https://github.com/CaviraOSS/OpenMemory), проверено **23.10.2026** | Это другой проект. Первое внешнее документационное встраивание, приглашение и дату приёма **не нашёл**. | **Не знаю**. Не объединяю статистику двух OpenMemory. |
| **Memory Bank** | [Cline, Nick Baumann](https://cline.bot/blog/memory-bank-how-to-make-cline-an-ai-agent-that-never-forgets), **06.02.2025** → [рецепт Cursor](https://forum.cursor.com/t/how-to-add-cline-memory-bank-feature-to-your-cursor/67868), **21.03.2025** | Во внешний Cursor workflow перенёс **ryosuke.suzuki**. Это методика и семейство Markdown-инструкций, а не один однозначный PyPI-пакет. | **Сработал перенос рецепта**. Единую серию звёзд/загрузок определить нельзя; эффект **не знаю**. |

Особенно полезны два документированных механизма: **самоподача с проверяемой интеграцией** у Mem0/claude-mem и **пользовательский запрос → внешняя документация → участие автора** у MemGPT. Истории «известный maintainer сам нашёл неизвестный пакет и позвал» для этих случаев не подтверждены. Для claude-mem порядок событий прямо виден в обсуждении заявки. genui{"citation":{"refs":["turn33view3","turn85view3","turn98view1","turn98view2","turn98view6"]}}

## 2. Что произошло со звёздами за две недели

**S14 до и после — наблюдение, а не число звёзд от встраивания.** Ссылки в числовой колонке ведут на источник дневных рядов; сохранённый снимок находится в приложенном архиве.

| Имя | Событие, дата D | S14 до → после | Вывод |
|---|---|---:|---|
| basic-memory | Awesome-list, **11.06.2026** | [**88 → 110**](https://api.github.com/repos/basicmachines-co/basic-memory/stargazers/history?per_page=30&page=1) | После на **25%** больше; вклад listing **не знаю**. |
| mcp-memory-service | Awesome-list, **06.10.2025** | [**34 → 34**](https://api.github.com/repos/doobidoo/mcp-memory-service/stargazers/history?per_page=30&page=2) | Ускорения нет. Это не доказывает causal-эффект ровно 0. |
| cognee | MotherDuck, **29.04.2024** | [**204 → 37**](https://api.github.com/repos/topoteretes/cognee/stargazers/history?per_page=30&page=5) | Всплески были до статьи; наблюдаемого ускорения после нет. |
| mem0 | LlamaIndex, **31.10.2024** | [**234 → 242**](https://api.github.com/repos/mem0ai/mem0/stargazers/history?per_page=30&page=4) | Почти одинаковый темп; вклад интеграции **не знаю**. |
| Graphiti | Neo4j, **24.03.2025** | [**232 → 736**](https://api.github.com/repos/getzep/graphiti/stargazers/history?per_page=30&page=3) | После в **3,17 раза** больше. Но **130★** появились уже **23.03**, до статьи. |
| memU | claude-memu, **05.02.2026** | [**2 732 → 1 505**](https://api.github.com/repos/NevaMind-AI/memU/stargazers/history?per_page=30&page=2) | Значительный рост уже шёл до внешнего плагина. |
| claude-mem | awesome-claude-code, **25.10.2025** | [**174 → 134**](https://api.github.com/repos/thedotmack/claude-mem/stargazers/history?per_page=30&page=2) | Всплеск предшествует принятию; ускорения в этих окнах нет. |
| **m3-memory** | LangChain, **30.07.2026** | [**1 → 5**](https://api.github.com/repos/skynetcmd/m3-memory/stargazers/history?per_page=30&page=1) | **Включение сработало**, небольшой рост рядом с событием; причинный вклад **не знаю**. |
| **langchain-ckg** | LangChain, **04.08.2026** | [**0 → 0**](https://api.github.com/repos/Yarmoluk/langchain-ckg/stargazers/history?per_page=30&page=1) | Принято, но сохранившегося звёздного роста в окне нет. |
| **pydantic-ai-chdb** | Pydantic AI, **14.07.2026** | [**0 → 0**](https://api.github.com/repos/chdb-io/pydantic-ai-chdb/stargazers/history?per_page=30&page=1) | Принято; **1** сохранившаяся звезда появилась в сам день события, исключённый из S14. |

Для CKG и chDB предшествующая база ограничена молодостью проектов: репозитории созданы **27.07.2026** и **14.07.2026** соответственно. Их нулевая база не является сравнением двух зрелых периодов. [Метаданные CKG](https://api.github.com/repos/Yarmoluk/langchain-ckg), [метаданные chDB](https://api.github.com/repos/chdb-io/pydantic-ai-chdb).

## 3. PyPI без зеркал

| Имя / URL | Даты сравнения | Число | Вывод |
|---|---|---:|---|
| **[basic-memory](https://pypistats.org/api/packages/basic-memory/overall?mirrors=false)** | До: **28.05–10.06.2026**; после: **12–25.06.2026** | **28 548 → 27 108**, **−5,04%** | После принятия в awesome-list увеличения загрузок в этом окне **нет**. |
| **[m3-memory](https://pypistats.org/api/packages/m3-memory/overall?mirrors=false)** | До: **16–29.07.2026**; после: **31.07–13.08.2026** | **4 376 → 4 802**, **+9,73%** | Рост наблюдается; сколько вызвано документацией — **не знаю**. |
| **[langchain-ckg](https://pypistats.org/api/packages/langchain-ckg/overall?mirrors=false)** | После: **05–18.08.2026** | **130** за **13 из 14** доступных суток | Полного сопоставимого 14-дневного ряда **не нашёл**; пропущенный день не заменял нулём. |
| **[pydantic-ai-chdb](https://pypistats.org/api/packages/pydantic-ai-chdb/overall?mirrors=false)** | После: **15–28.07.2026** | **833** за **11 из 14** доступных суток | Полного ряда и базы до события **не нашёл**. |
| mcp-memory-service, cognee, Mem0, Graphiti, Letta | Нужные события **2023–2025** | **Не нашёл** исторических двухнедельных рядов PyPI Stats | Нынешние загрузки не подставляю вместо исторических. |
| claude-mem, Memory Bank | Рассмотренные события **2025** | **Не применимо** как сопоставимая PyPI-метрика | Это плагин/методика, а не один соответствующий Python-пакет. |
| memU, оба OpenMemory | Рассмотренные внешние события | **Не нашёл** подходящей однозначной серии | Не смешиваю загрузки библиотеки, плагина, API-клиента и монорепозитория. |

PyPI Stats документирует ограниченную историю; фактический ответ для Basic Memory содержал **198 дней, 08.04–22.10.2026**. Более старые данные потенциально можно исследовать через публичный PyPI BigQuery dataset, но такой исторический расчёт здесь не получен. Исключение зеркал также не исключает CI и повторные установки: **downloads ≠ люди**. genui{"citation":{"refs":["turn17view2","turn37view3"]}}

**Что это меняет для SGM:** принятие маленьких пакетов доказано. Автоматический переход «строка в крупных доках → сотни звёзд» не доказан. У ближайших аналогов может быть заметная установка пакета при почти отсутствующей звёздности.

---

# (в) Show HN для dev-инструмента без аудитории

## 1. Что удалось измерить

Найден открытый архив исследования **Obada Kraishan**, опубликованного **06.11.2025**. Я пересчитал индивидуальные `starred_at` в интервале:

**исходная публикация HN ≤ время звезды < исходная публикация + 48 часов.**

Использована фиксированная версия архива от **29.08.2025**. Для каждой строки ниже проверено, что наблюдение продолжается за концом окна. Все 20 повторных расчётов совпали. Это **сохранившиеся в архиве звёзды**, не чистый исторический прирост и не причинно приписанные HN звёзды. [Архив и методика](https://github.com/obadaKraishan/Launch-Day-Diffusion/tree/867ea952721436f80fbb7e095d9201ffc3ca8d2e). genui{"citation":{"ref":"turn81view0"}}

**Ограничения выборки:**

- У авторов отсутствие аудитории **не подтверждено**.
- Все 20 репозиториев имели небольшое число сохранившихся звёзд до поста; это другой критерий.
- Исходный сбор отсеивал посты ниже **10 очков**.
- Это выборка OSS-инструментов разработки, CLI/инфраструктуры и агентной памяти, а не случайная выборка всех запусков.

## 2. Двадцать постов: даты, очки, звёзды за 48 часов

Очки проверены **23.10.2026**. Время — исходная публикация в UTC. **«Не знаю» в последней колонке относится к причинному вкладу HN**, а не к наличию измеренного роста.

| Имя / URL поста и исходных данных | Дата и время UTC | Очки | ★ до поста в архиве | ★ за 48 ч | Вывод о вкладе HN |
|---|---|---:|---:|---:|---|
| **Gonzo** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_control-theory_gonzo.jsonl) | **25.08.2025, пн, 19:44** | 74 | 47 | **261** | Не знаю |
| **Whisker** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_pipecat-ai_whisker.jsonl) | **25.08.2025, пн, 18:37** | 11 | 6 | **37** | Не знаю |
| **SecretMemoryLocker** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_secretml_secretmemorylocker.jsonl) | **25.08.2025, пн, 16:39** | 14 | 0 | **9** | Не знаю |
| **Async** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_bkdevs_async-server.jsonl) | **25.08.2025, пн, 13:21** | 107 | 2 | **227** | Не знаю |
| **Timep** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_jkool702_timep.jsonl) | **25.08.2025, пн, 05:17** | 53 | 115 | **49** | Не знаю |
| **FilterQL** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_adamhl8_filterql.jsonl) | **24.08.2025, вс, 18:55** | 77 | 14 | **8** | Не знаю; позднее продвижение |
| **CasCache** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_unkn0wn-root_cascache.jsonl) | **24.08.2025, вс, 11:24** | 20 | 0 | **27** | Не знаю |
| **Port Kill** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_kagehq_port-kill.jsonl) | **24.08.2025, вс, 03:08** | 128 | 1 | **293** | Не знаю |
| **OctaneDB** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_rijinraju_octanedb.jsonl) | **23.08.2025, сб, 06:31** | 33 | 1 | **57** | Не знаю |
| **AgentState** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_ayushmi_agentstate.jsonl) | **23.08.2025, сб, 04:57** | 16 | 1 | **37** | Не знаю |
| **Lacquer** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_lacquerai_lacquer.jsonl) | **22.08.2025, пт, 14:22** | 11 | 9 | **4** | Не знаю; слабый наблюдаемый результат |
| **FeOx** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_mehrantsi_feoxdb.jsonl) | **22.08.2025, пт, 07:57** | 37 | 1 | **70** | Не знаю |
| **Claudable** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_opactorai_claudable.jsonl) | **21.08.2025, чт, 13:02** | 38 | 87 | **362** | Не знаю |
| **DiffMem** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_growth-kinetics_diffmem.jsonl) | **21.08.2025, чт, 06:20** | 198 | 34 | **418** | Не знаю |
| **PlutoPrint** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_plutoprint_plutoprint.jsonl) | **20.08.2025, ср, 20:37** | 161 | 13 | **393** | Не знаю |
| **CCPM** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_automazeio_ccpm.jsonl) | **20.08.2025, ср, 10:32** | 175 | 2 | **1 215** | Не знаю |
| **In-Memoria** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_pi22by7_in-memoria.jsonl) | **19.08.2025, вт, 16:29** | 13 | 3 | **20** | Не знаю |
| **Memori** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_gibsonai_memori.jsonl) | **18.08.2025, пн, 14:29** | 17 | 51 | **90** | Не знаю |
| **Doxx** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_bgreenwell_doxx.jsonl) | **17.08.2025, вс, 19:52** | 261 | 14 | **1 026** | Не знаю |
| **JMAP MCP** — [HN](https://news.ycombinator.com/item?id <tg-id>), [данные](https://raw.githubusercontent.com/obadaKraishan/Launch-Day-Diffusion/867ea952721436f80fbb7e095d9201ffc3ca8d2e/src/data/raw/stargazers_wyattjoh_jmap-mcp.jsonl) | **15.08.2025, пт, 17:39** | 55 | 0 | **135** | Не знаю |

### Медианы по очкам

Это расчёт **только по 20 строкам выше**, не прогноз для SGM и не оценка типичного холодного запуска.

| Очки HN | Число постов | Медиана ★ за исходные 48 часов | Вывод |
|---|---:|---:|---|
| **<10** | 0 | **Не нашёл** | Исходный архив исключал эту группу. |
| **10–49** | 10 | **37** | Есть измерение выборки; переносимость на SGM неизвестна. |
| **50–199** | 9 | **261** | Широкий разброс; от 8 до 1 215 в этой группе. |
| **200+** | 1 | **1 026** | Один Doxx; как общая медиана практически неинформативно. |

### Существенная ловушка: 48 часов после чего

У FilterQL [официальный API](https://hacker-news.firebaseio.com/v0/item/45006686.json) хранит **24.08.2025 18:55 UTC**, а страница HN показывает **27 августа**. У Async [API](https://hacker-news.firebaseio.com/v0/item/45013572.json) — **25 августа**, страница — **26 августа**. При повторном продвижении, *second-chance*, отображаемая дата может измениться, а исходный timestamp сохраняется.

Поэтому формулировка **«77 очков дали FilterQL только 8 звёзд» некорректна**: основной показ мог произойти после измеренного окна. Корректно: «за первые 48 часов после исходной публикации сохранилось 8 звёзд; позднее пост достиг 77 очков». Это же ограничение относится к анализу ответов автора в первые два часа. genui{"citation":{"refs":["turn88view3","turn88view5"]}}

## 3. Свежие memory/MCP-примеры 2026 года и слабые запуски

Для этих случаев доступны дневные агрегаты. Они позволяют получить **границы**, но не точное rolling-окно 48 часов.

Если D — UTC-день публикации, весь D+1 входит в окно, а D и D+2 — частично. Поэтому:

**звёзды(D+1) ≤ ★48 ≤ звёзды(D)+звёзды(D+1)+звёзды(D+2).**

Это математические границы для сохранившихся звёзд, не доверительный интервал.

| Имя / URL | Дата | Очки | Звёзды D / D+1 / D+2 | Границы ★48 | Вывод |
|---|---|---:|---:|---:|---|
| [Basic Memory](https://news.ycombinator.com/item?id <tg-id>) | **15.03.2025** | 4 | 5 / 7 / 17 | **7–29** | HN получил мало внимания; причинный вклад **не знаю**. |
| [A-MEM MCP](https://news.ycombinator.com/item?id <tg-id>) | **10.01.2026** | 7 | 7 / 1 / 1 | **1–9** | Небольшой наблюдаемый рост. |
| [Breathe-Memory](https://news.ycombinator.com/item?id <tg-id>) | **26.03.2026** | 6 | 5 / 0 / 0 | **0–5** | Небольшой наблюдаемый рост. |
| [SQLite Memory](https://news.ycombinator.com/item?id <tg-id>) | **07.04.2026** | 10 | 6 / 2 / 0 | **2–8** | Близкий тематический пример со слабым результатом. |
| [MCP Memory / fellowgeek](https://news.ycombinator.com/item?id <tg-id>) | **13.08.2026** | 71 | 120 / 31 / 20 | **31–171** | Рост наблюдается; точную атрибуцию **не знаю**. |
| [LocalGPT](https://news.ycombinator.com/item?id <tg-id>) | **08.02.2026** | 331 | 439 / 86 / 42 | **86–567** | Значительное внимание; точного ★48 из дневного ряда нет. |

Источники — соответствующие `/repos/{owner}/{repo}/stargazers/history`, снимок **23.10.2026**; полные URL и ответы сохранены в архиве. Эти границы **не смешиваются** с точными архивными медианами выше.

## 4. Что объединяло заметные посты — и чего данные не доказывают

### Заголовок, вступление, ответы

| Кейс / дата | Подача и первый текст автора | Вложенные ответы автора за первые 120 минут | Число / вывод |
|---|---|---:|---|
| **DiffMem**, 21.08.2025, [пост](https://news.ycombinator.com/item?id <tg-id>) | **alexmrv** объясняет конкретный эксперимент: Git + Markdown + BM25; явно называет проект PoC. | **5** | **198 очков, 418★48**. Понятное инженерное отличие; вклад каждого элемента отдельно неизвестен. |
| **Doxx**, 17.08.2025, [пост](https://news.ycombinator.com/item?id <tg-id>) | **w108bmg** описывает личную проблему чтения `.docx` в терминале; install/run непосредственно во вступлении. | **0**; первый отдельный комментарий через **295 минут** | **261 очко, 1 026★48**. Требование обязательно отвечать в первые два часа исходной публикации данными не подтверждается. |
| **CCPM**, 20.08.2025, [пост](https://news.ycombinator.com/item?id <tg-id>) | **aroussi** объясняет потерю контекста при нескольких Claude-агентах и использование GitHub Issues. | **1**, через **59,5 минуты** | **175 очков, 1 215★48**. При этом часть обещаний README подверглась критике. |
| **Memori**, 18.08.2025, [пост](https://news.ycombinator.com/item?id <tg-id>) | **Arindam1729** представляется членом команды, объясняет SQL memory и даёт ссылки на demo apps. | **3** | **17 очков, 90★48**. Активные ответы не гарантировали высокий score. |

Число ответов пересчитано по деревьям комментариев [DiffMem](https://hn.algolia.com/api/v1/items/44969622), [Doxx](https://hn.algolia.com/api/v1/items/44934391), [CCPM](https://hn.algolia.com/api/v1/items/44960594), [Memori](https://hn.algolia.com/api/v1/items/44941043); корневое вступление считается отдельно.

### README именно на момент запуска

| Имя / историческая версия | Дата версии | Что было в начале README | Вывод |
|---|---|---|---|
| [Doxx](https://raw.githubusercontent.com/bgreenwell/doxx/ace0ec9250fa267a91ab16f7066bbfe126245404/README.md) | **17.08.2025** | Конкретное назначение `.docx` в терминале, screenshot, функции | Хорошо показывает результат. Причинный коэффициент **не знаю**. |
| [DiffMem](https://raw.githubusercontent.com/growth-kinetics/diffmem/8049fa123413b6ef2b1445be701a8f3228eb2839/README.md) | **20.08.2025** | Текстовое объяснение архитектуры Git/Markdown/BM25 и ограничений PoC | Крупная картинка в начале не была обязательной. |
| [CCPM](https://raw.githubusercontent.com/automazeio/ccpm/64f6a6a03f331e3cb7d576d432f318893d699241/README.md) | **20.08.2025** | Workflow, screenshot, длинное оглавление | Короткий README не является доказанным необходимым условием. |
| [FilterQL](https://raw.githubusercontent.com/adamhl8/filterql/5b0f99acf8fc30a6b2eddc7e34e39c9e7c4970f6/README.md) | **17.08.2025** | Короткое описание, анимированный пример запроса, оглавление | Визуальный пример сам по себе не определяет измеренный результат. |

Попадание в ретроспективную страницу HN Past/front подтверждено для **DiffMem — 21.08.2025, позиция 25; Doxx — 18.08.2025, позиция 14; PlutoPrint — 20.08.2025, позиция 21**. Это позиции на доступной исторической странице, **не лучший live-rank и не время пребывания на главной**. Их исходные публикации пришлись на четверг 06:20, воскресенье 19:52 и среду 20:37 UTC — общего обязательного временного слота здесь нет. genui{"citation":{"refs":["turn100view3","turn100view4","turn100view5"]}}

### Демо без регистрации и то, что мешало

Официальные правила Show HN требуют того, что можно попробовать, личной работы автора и готовности обсуждать; рекомендуют убирать signup/email-барьеры. Для локального dev-tool работающий install/run — подходящий способ дать попробовать продукт. Это правило площадки, не измеренный коэффициент звёзд. genui{"citation":{"ref":"turn100view0"}}

| Имя / URL | Дата | Число | Что подтверждено / вывод |
|---|---|---:|---|
| [Browser-Use MCP](https://news.ycombinator.com/item?id <tg-id>) | **19.03.2025** | **1 очко** | Ссылка ведёт на X; читатели спрашивают, где инструмент и как его получить. **Подтверждена трудность доступа**, сколько звёзд потеряно — **не знаю**. |
| [Blender MCP](https://news.ycombinator.com/item?id <tg-id>) | **20.07.2025** | **151 очко** | Читатели указывают на отсутствие обещанной реализации в linked repo. **Подтверждена проблема доверия**, количественный ущерб неизвестен. |
| [CCPM](https://news.ycombinator.com/item?id <tg-id>) | **20.08.2025** | **1 215★48** | Читатели оспаривают проценты улучшений из README, но рост всё равно большой. Формула «маркетинговый README убивает запуск» не подтверждается. |

Эти обсуждения позволяют назвать конкретные препятствия. Они не дают оснований объявлять отсутствие GIF, длинный README или несколько часов без ответов универсальной причиной провала. genui{"citation":{"refs":["turn100view1","turn100view2","turn87view5"]}}

### Почему я не использую популярное «1,4 звезды на очко»

**Daniel King, 23.04.2026**, описывает корпус **188 085 Show HN** и медиану **2 очка**, но звёздный расчёт делает только для **491 из top-500**, все с **258+ очками**. Его оценка **1,4★/очко** не является оценкой для поста с 4–20 очками и не является visitor→star conversion. Исходные данные на странице предлагаются по запросу. Собственный Burnish автор описывает как **1 очко, без star bump**. genui{"citation":{"ref":"turn96view5"}}

**Практический вывод для SGM — моя рекомендация:** готовить Show HN вокруг воспроизводимого сценария с несколькими Markdown-заметками, реальным переходом по `wikilinks` и видимыми источниками ответа. Выбирать время, когда можно обсуждать проект. Доказанного универсального «лучшего часа» для неизвестного автора памяти **не нашёл**.

---

# (г) Конверсия посетитель → звезда

## 1. Строго сопоставимых цифр по источникам почти нет

Для нужной метрики одновременно необходимы:

**новые звёзды от конкретного источника / уникальные посетители README из того же источника за то же окно.**

В найденных публикациях обычно есть одна сторона этой дроби либо смешиваются периоды, просмотры видео, посещения сайта, посещения репозитория и все звёзды.

| Источник | Найденные опубликованные числа / URL | Дата | Конверсия README → ★ | Вывод |
|---|---|---|---|---|
| **HN** | Двадцать 48-часовых звёздных окон выше; посетителей README по ним нет | События **2025**, проверка **23.10.2026** | **Не нашёл** свежего сопоставимого измерения | Очки HN — не знаменатель visitor conversion. |
| **Reddit** | В [opendirectory case study](https://varnan.tech/case-studies/how-opendirectory-hit-10-000-npm-downloads-and-350-github-stars-in-one-month-from-scratch): **483 уникальных** посетителя с `reddit.com` за 14 дней | **19.05.2026** | **Не нашёл** числа звёзд именно этих посетителей | Есть referral traffic, нет конверсии. |
| **Чужая документация** | Датированные включения и S14 выше | **2024–2026** | **Не нашёл** опубликованной пары «посетители с docs → их звёзды» | Принятый PR и общий рост недостаточны. |
| **Поиск GitHub** | В том же кейсе **123 уникальных** с `github.com`; поисковый трафик отдельно не выделен | **19.05.2026** | **Не нашёл** | `github.com` как referrer не равен GitHub Search. |
| **Awesome-списки / каталоги** | В том же кейсе `koourse.ai`: **107 уникальных** посетителей за 14 дней | **19.05.2026** | **Не нашёл** звёзд по этому источнику | Это каталог, а не доказательство конверсии awesome-списков. |
| **Twitter/X** | В том же кейсе `t.co`: **64 уникальных** посетителя за 14 дней | **19.05.2026** | **Не нашёл** | Источник переходов виден, источник звёзд — нет. |
| **YouTube** | [AirLLM video case](https://varnan.tech/case-studies/8500-github-stars-across-7-dev-tools-varnans-predictable-distribution-engine): **226 тыс.+** просмотров Instagram+YouTube совместно, **около 250★**, приписанных продвижению автором | Видео **26.02.2026**, отчёт **04.03.2026** | **Не нашёл** | Смешаны платформы, знаменатель — просмотры видео, атрибуция оценочная. Не benchmark README-конверсии. |

В opendirectory **350+★ относятся к 30 дням**, а **2 475 уникальных посетителей — к 14 дням**. Делить первое на второе и объявлять результат конверсией нельзя. Это самоотчёт команды, занимающейся продвижением, а не независимый эксперимент. genui{"citation":{"ref":"turn101view0"}}

В видеоотчёте проценты порядка 25–35% относятся к оценкам **комментаторы → звёзды**, с поправками автора. Они не измеряют README → звезда. genui{"citation":{"ref":"turn102view2"}}

## 2. Ближайшие опубликованные отношения — с явными ограничениями

| Имя / URL | Дата / окно | Число | Что можно и нельзя заключить |
|---|---|---:|---|
| **[AI doc/context sync, u/hustler-econ](https://www.reddit.com/r/ClaudeAI/comments/1se8l6h/thank_you_redditors_only_because_of_the_community/)** | Автор говорит о **2 неделях после публикаций около 22 марта**. Точную дату публикации и год из первичной страницы **не нашёл**; проверено 23.10.2026 | **60★ / 474 уникальных посетителя = 12,7%** | Самоотчёт, смешаны Reddit и LinkedIn. Это общий коэффициент за заявленное окно, не чистая Reddit-конверсия; свежесть недостаточно установлена. |
| **[Hacking the Cloud — 2025 review](https://hackingthe.cloud/blog/2025_wrap-up/)** | Отчёт **04.01.2026**, показатели собраны **28.12.2025**, период — 2025 год | **683 новых★ / 96 329 уникальных посетителей сайта = 0,71%** | Длинное окно, но это **сайт с документацией**, не уникальные посетители GitHub README. Каналы не привязаны к звёздам. |
| **[NebulaGraph / Jamie Liu](https://medium.com/nebula-graph/being-on-hacker-news-front-page-brought-us-much-more-than-just-300-stars-on-github-within-24-hours-b6a3840e99fa)** | **19.01.2020**, вне требуемого свежего периода; около **18 часов** на главной к моменту описания | **400+★ / 2 291 уникальный посетитель репозитория ≈ 17,5%+** | Близкий исторический самоотчёт по HN, но не современная норма и не доказанная индивидуальная атрибуция звёзд. |

Источники действительно публикуют эти числители и знаменатели; проценты — мой арифметический расчёт. Их нельзя объединять в диапазон «нормальная конверсия OSS — от 0,7 до 17,5%»: измеряются разные аудитории и разные действия. genui{"citation":{"refs":["turn101view4","turn101view2","turn102view0"]}}

GitHub предоставляет владельцам репозитория трафик за последние **14 дней**: просмотры, уникальных посетителей, referrers и популярные страницы. Публичной привязки каждой звезды к источнику перехода это не даёт. genui{"citation":{"ref":"turn17view3"}}

---

# Что эти данные означают для цели 1 000★

Из ваших исходных **12★** до цели требуется ещё **988★**. Обоснованную модель «N включений за M месяцев дадут 988 звёзд» по найденным данным построить нельзя.

Чисто арифметически, **если** известная доля посетителей конкретного канала становилась бы новыми stargazers, требования были бы такими:

| Условная конверсия — не benchmark | Посетителей для ещё 988★ |
|---:|---:|
| 1% | **98 800** |
| 3% | **32 934** |
| 5% | **19 760** |
| 10% | **9 880** |

Это сценарии, не прогноз. Ваши **12 накопленных звёзд и 34 посетителя за последние 14 дней** относятся к разным периодам; отношение 12/34 конверсией не является.

Я бы использовал следующие критерии прогресса:

1. **Первое принятое внешнее включение с рабочей установкой.** Успех — опубликованная карточка, adapter или tutorial, который устанавливает SGM.
2. **Первый измеримый поток из этого места.** Сохранять трафик/referrers, звёзды и PyPI по дням; отмечать дату публикации, релизы и другие появления.
3. **Первый подтверждённый чужой сценарий использования.** Issue, пример, downstream-плагин или исправление от человека, действительно встроившего пакет.
4. **Повторение удачного формата.** Расширять тот тип интеграции, который даёт пользователей, а не автоматически переходить к следующему большому репозиторию.

Для самой интеграционной карточки есть техническая деталь: текущий README SGM использует **`pip install 'sqlite-graph-memory[embeddings]'`** для полного сценария поиска и отдельно описывает индексные файлы/модели. Поэтому формулировку «без LLM-ключа» полезно сопровождать точными требованиями к первой установке и индексу. Это уменьшает риск, что maintainer воспроизведёт только базовую установку и не получит обещанный сценарий. Проверка README — **23.10.2026**. genui{"citation":{"ref":"turn37view1"}}

---

# Топ-10 мест для первой внешней строки установки SGM

Это **качественное ранжирование по вероятности принятия × потенциальному охвату**, а не вычисленные вероятности: долю принятых заявок и посещаемость конкретных страниц **не нашёл**. Учитываю открытость процесса, соответствие SGM, наличие реальной установки и масштаб принимающего проекта. Состояние — **23.10.2026**.

| № | Место / URL | Оценка вероятности × охвата | Первый конкретный шаг | Что считать результатом |
|---:|---|---|---|---|
| **1** | **[Gemini CLI Gallery](https://geminicli.com/extensions/)** | **Высокая формальная доступность × большой проект**: документирована автоиндексация, 107,5 тыс.★ у клиента | Собрать минимальное расширение с `gemini-extension.json`, запускающее проверенную команду SGM MCP; добавить `gemini-cli-extension`; проверить появление после ежедневного обхода. | Публичная карточка с работающей установкой. Рост после неё пока **не знаю**. |
| **2** | **[LangChain Retriever integrations](https://docs.langchain.com/oss/python/integrations/retrievers)** | **Хорошее соответствие после адаптера × большой проект**; есть принятые малые graph/memory-примеры | Сделать тонкий `BaseRetriever`, возвращающий документы и происхождение результата — путь заметки/связи. Опубликовать пакет и пример, затем **Integration listing issue** по текущим правилам. | Принятая запись в relevant retriever list и внешняя документация со строкой установки. |
| **3** | **[Pydantic AI Third-Party Capabilities](https://pydantic.dev/docs/ai/capabilities/third-party/)** | **Открытый PR-путь × подходящая аудитория**; 20,8 тыс.★ | Подготовить capability/toolset поиска и чтения связанных заметок, типизированный результат и короткий работающий пример. Подать один PR в эту страницу. | Принятый пакет в официальном third-party разделе. |
| **4** | **[Cursor Marketplace](https://cursor.com/marketplace/publish)** | **Условная вероятность review × потенциально широкий клиент**; трафик каталога неизвестен | Сделать plugin с SGM MCP и одним конкретным workflow работы с заметками; README, manifest, переменные и локальная проверка. Отправить repository URL через форму. | Approved listing, не просто отправленная форма. |
| **5** | **[OpenWebUI Community tutorials](https://docs.openwebui.com/tutorials/)** | **Средняя при полезном tutorial × большой проект**, 156,5 тыс.★ | Подготовить рецепт «небольшой Obsidian vault → SGM → MCPO → OpenWebUI» с несколькими связанными заметками, точной установкой и запросом, которому нужен переход по ссылке. | Merged tutorial с воспроизводимой установкой SGM. |
| **6** | **[Claude Code Templates](https://www.aitmpl.com/)** | **Средняя, с существенной очередью × тематический install-каталог**, 32,8 тыс.★ | Подготовить MCP JSON по формату проекта и один PR с доказанным Claude Code-сценарием и инструкцией проверки. | После merge — карточка и работающая установка через CLI Templates. |
| **7** | **[Новый Cline Marketplace](https://github.com/cline/marketplace)** | **Средняя/пониженная из-за очереди × подходящий клиент**, 70,8 тыс.★ | Создать schema-valid `registry/mcps/sqlite-graph-memory/entry.json`; проверить чистую установку и точные arguments. Использовать новый registry. | Запись принята и видна клиенту; открытый PR результатом не считать. |
| **8** | **[Agno memory examples](https://docs.agno.com/examples/integrations/memory/mem0-integration)** | **Условная, issue-first × большой framework**, 42,9 тыс.★ | Открыть issue с компактным runnable-примером и объяснением недостающего сценария — retrieval по существующим wikilinks. Получить `ready`, затем оформлять cookbook PR. | `ready` → merged cookbook → строка установки. |
| **9** | **[awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code)** | **Избирательный review × большой тематический список**, 56,3 тыс.★ | После проверки возрастного eligibility заполнить web issue form. Показать конкретное использование в Claude Code; приложить короткий воспроизводимый сценарий. | Принятие maintainer и merge автоматической записи. Эффект измерять отдельно. |
| **10** | **[Zed Extensions](https://zed.dev/extensions)** | **Условная и временная возможность × меньший наблюдаемый каталог** | **Сначала проверить статус перехода на MCP Registry.** Если приём MCP extensions ещё полезен, сделать минимальный wrapper одного сервера и submodule PR. | Опубликованное расширение; если переход уже завершён, использовать имеющуюся регистрацию SGM в MCP Registry. |