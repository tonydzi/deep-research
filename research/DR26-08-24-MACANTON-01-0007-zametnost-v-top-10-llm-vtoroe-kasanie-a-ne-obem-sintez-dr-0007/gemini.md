# DR26-08-24-MACANTON-01-0007 — raw report from gemini

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
rail: gemini-web
vendor: Google Gemini (gemini.google.com)
mode: Deep Research + 3.6 Thinking (вендорский DR)
chat: https://приватный чат
collected: 2026-08-24T09:05Z


---

Стратегия заметности в Open Source
Contents
Share and export
Create
Архитектура DevRel-конверсии и стратегии OSS-заметности в экосистемах Frontier LLM
Карта верифицированных источников

В таблице ниже систематизированы ключевые документальные и академические первоисточники, использованные при формировании аналитических выводов настоящего отчета.

Идентификатор	Первоисточник / Организация	URL публикации	Дата публикации / Обращения	Тематическая область
	IEEE / MSR / ResearchGate	https://www.researchgate.net/publication/281401625_Matching_GitHub_Developer_Profiles_to_Job_Advertisements	Август 2015	Модели сопоставления активности разработчиков на GitHub с IT-вакансиями
	arXiv / SE 2030 Workshop	https://arxiv.org/html/2504.06387v1	Апрель 2025	Оценка технических компетенций через открытые репозитории кода
	FindJob.live Research	https://findjob.live/the-networking-necessity-building-connections-in-a-fast-movi	Июнь 2024	Индустриальные кейсы найма через документированный вклад в OSS
	Reddit / Hugging Face AMA	https://www.reddit.com/r/LocalLLaMA/comments/1n8c3l2/ama_with_hugging_face_science_the_team_behind/	Сентябрь 2024	Траектории найма исследовательской группы Hugging Face через открытый код
	Sequoia Capital Podcast	https://sequoiacap.com/podcast/training-data-dhanji-prasanna	Октябрь 2023	Кейс найма CTO компании Block через ранние вклады в open source
	Google Developers / Medium	https://developers.google.com/community/experts	Июль 2024	Критерии отбора и структура реферальной программы Google Developer Experts (GDE)
	Google Cloud Partners	https://partners.cloud.google.com/google-cloud-ambassadors	Январь 2025	Архитектура программ Google Cloud Ambassador и сертификация ML
	QA Skills / UK AISI	https://qaskills.sh/blog/inspect-ai-evals-tutorial-2026	Февраль 2026	Спецификация и среда исполнения бенчмарков UK AISI Inspect AI
	UK AISI / GitHub	https://github.com/UKGovernmentBEIS/inspect_evals	Май 2026	Стандарты контрибьюции в Inspect Evals и критерии валидации QA
	arXiv / UK AISI, Arcadia Impact	https://arxiv.org/pdf/2507.06893	Июль 2025	Уроки разработки и поддержки открытого репозитория бенчмарков AISI
	OpenAI / BuiltInSF	https://www.builtinsf.com/job/residency-2026/7840622	Январь 2026	Программа OpenAI Residency: отбор талантов через open source
	GitHub REST API Docs	https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api	Март 2026	Вторичные лимиты (Secondary Rate Limits) и правила антиабьюза GitHub
	EmergentMind / IEEE / ACM	https://www.emergentmind.com/topics/pull-request-acceptance	Январь 2026	Предиктивные модели принятия PR: размер дельты, linked issue, социальный граф
	Empirical Software Engineering	https://www.researchgate.net/publication/383108411_Predicting_the_First_Response_Latency_of_Maintainers_and_Contributors_in_Pull_Requests	Октябрь 2024	Исследование факторов задержки первого ответа мейнтейнеров на pull requests
	arXiv / Software Engineering	https://arxiv.org/html/2601.13466v1	Январь 2026	Влияние скорости резолюции PR на трехкратное удержание контрибьюторов
	Model Context Protocol	https://github.com/modelcontextprotocol/python-sdk/blob/main/CONTRIBUTING.md	Февраль 2026	Регламент контрибьюции MCP: запрет нераскрытых AI-агентов, Issue-First
	Google Open Source Blog	https://opensource.googleblog.com/2022/09/announcing-the-second-group-of-open-source-peer-bonus-winners-in-2022.html	Октябрь 2022	Google Open Source Peer Bonus: механизм номинации внешних контрибьюторов
	TensorFlow Models / GitHub	https://github.com/tensorflow/models/issues/8635	Ноябрь 2021	Номинация внешних контрибьюторов моделей на Google Open Source Peer Bonus
	Daniel Stenberg Blog (curl)	https://daniel.haxx.se/blog/2024/01/02/the-i-in-llm-stands-for-intelligence/	Апрель 2026	Анализ AI Slop в открытых проектах и закрытие публичных Bug Bounty программ
	vLLM Project / GitHub	https://github.com/vllm-project/vllm-omni/blob/main/.claude/skills/review-pr/references/delivery/maintainer-style-study.md	Май 2026	Анализ стиля ревью мейнтейнеров на базе выборки из 928 pull requests
	DevRelCon / Orbit Model	https://developerrelations.com/talks/using-the-orbit-model-to-demonstrate-community-value/	Май 2022	Применение модели Orbit для квантификации ценности инженерных сообществ
	Meta for Developers / ImpactOpen	https://www.impactopen.org/	Февраль 2026	Meta Llama Impact Grants: грантовые программы объемом до $500K
1. Механика конверсии: от первого ответа к устойчивым отношениям и офферу

Уровень уверенности: Established

Архитектура социально-технического доказательства

В индустрии открытого исходного кода и Developer Relations традиционный рекрутинговый пайплайн (скрининг резюме, алгоритмические секции, поведенческие интервью) заменяется моделью непрерывного социально-технического доказательства. Высокая плотность внешних откликов (71–93%), зафиксированная в инженерных замерах, свидетельствует о преодолении первичного барьера видимости, однако конверсия этого внимания в институциональные предложения требует прохождения четко структурированной лестницы взаимодействия.

Уровень лестницы	Тип взаимодействия (Touchpoint)	Сигнал для мейнтейнера / Вендора	Сроки перехода
Уровень 0: Co-Design & Discovery	

Развернутое обсуждение архитектуры в живом issue до отправки кода

	Высокая инженерная культура, понимание скоупа проекта, отсутствие эго	1–3 недели
Уровень 1: High-Signal PR	

Компактный PR (≤3 коммитов, ≤25 строк) со сквозными тестами

	Минимальная нагрузка на ревьюера, предсказуемость исполнения, надежность	2–6 недель
Уровень 2: Sustained Multi-Touch	

Систематическое ревью чужих PR, воспроизведение багов в песочницах

	Снижение операционного бремени кор-команды, переход в категорию негласного мейнтейнера	2–4 месяца
Уровень 3: Direct Championing	

Номинация на Peer Bonus, тегирование в закрытых RFC, персональный реферал

	Личное поручительство инженера вендора, гарантированный обход ATS-фильтров	4–6 месяцев
Уровень 4: Offer / Strategic Contract	

Оффер в команду Core/DevRel, грант (Impact Grant/Residency) или B2B-контракт

	Нулевое время онбординга, доказанная способность автономно решать задачи вендора	6–9 месяцев
Задокументированные индустриальные прецеденты

Hugging Face (Science & Post-Training Teams): Практика формирования ключевой инженерной команды Hugging Face исторически строилась вокруг открытых артефактов. Инженеры Леандро фон Верра (Leandro von Werra) и Льюис Танстолл (Lewis Tunstall) были наняты в штат после длительной совместной работы над открытыми репозиториями экосистемы и написания книги по архитектуре трансформеров совместно с сооснователем компании Томасом Вольфом. Эд Бичинг (Edward Beeching) вошел в состав научной группы благодаря публикации независимых ML-проектов и активной интеграции библиотек. Руководство Hugging Face прямо формулирует этот принцип как максимизацию «поверхности соприкосновения с удачей» (luck surface area) через постоянное инженерное присутствие в GitHub и Discord.

Block / Square (Кейс Dhanji Prasanna): Будущий CTO Block Дханджи Прасанна начал вносить коммиты в инфраструктурные библиотеки экосистемы еще в 2011 году. Его регулярные технические патчи и участие в архитектурных дискуссиях привлекли внимание первого CTO компании Боба Ли, что переросло в прямой оффер в обход классических этапов найма.

OpenAI Residency Track: Официальная спецификация исследовательской программы OpenAI Residency прямо выделяет кандидатов с нестандартным бэкграундом, доказавших свою квалификацию через весомый вклад в open-source и самостоятельные исследовательские проекты, а не академические регалии. Шестимесячный оплачиваемый трек резидентуры служит главным мостом конверсии независимых контрибьюторов в штат постоянных исследователей frontier-моделей.

Решающий шаг и временные горизонты

Решающим фактором конверсии выступает не суммарное число открытых пулл-реквестов, а готовность внешней лаборатории взять на себя долгосрочную поддержку нетривиального функционального блока (например, поддержание тестовых песочниц k8s для Inspect AI или реализация кастомных CUDA-ядер инференса). Эмпирический горизонт перехода от первых ответов к офферу составляет от 3 до 9 месяцев при условии регулярного присутствия с интервалом не реже 1–2 качественных технических взаимодействий в неделю.

2. Анализ экосистем вендоров: нормы, антипаттерны и программы признания

Уровень уверенности: Established для Anthropic, UK AISI, Google, Microsoft; Emerging для Hugging Face, Meta, Qwen, DeepSeek, Mistral, Cohere

Культура коммуникации, готовность принимать внешние ревью и официальные программы признания существенно различаются в зависимости от внутренней операционной модели вендора.

Экосистема / Вендор	Целевые репозитории	Восприятие внешних PR и ревью	Кто реально читает треды	Программы признания и грантов
Anthropic (MCP)	

modelcontextprotocol/*, anthropic-sdk-python

[cite: 20, 32]

	

Строго Issue-first; запрет автономных ботов

	

Создатели протокола (@dsp, @jspahrsummers), инженеры SDK

	

Linux Foundation MCP Working Groups


UK AISI	

inspect_ai, inspect_evals, inspect_k8s_sandbox

[cite: 10, 31]

	

Высочайший приоритет для бенчмарков со статьями arXiv

	

Исследователи безопасности и штатные инженеры института

	

Inspect Evals Official Register


Google & DeepMind	

tensorflow/models, gemma, jax, GoogleCloudPlatform/*

[cite: 9, 23]

	

Внешние PR приветствуются в upstream-тулинге и примерах

	

DevRel инженеры, Cloud Advocates, Core SWE команды

	

Google Open Source Peer Bonus, Google Developer Experts (GDE)


Microsoft	

semantic-kernel, AutoGen, csharp-sdk (MCP)

[cite: 32]

	Высокая открытость к интеграционным PR и коннекторам	DevRel менеджеры, SWE партнерских команд	Microsoft MVP, Microsoft Reactor Fellowships
Meta	

llama, pytorch, executorch, vLLM-адаптеры

	

Ядро закрыто для спонтанных PR; тулинг и оптимизации открыты

	Мейнтейнеры прикладных библиотек и экосистемные менеджеры	

Meta Llama Impact Grants (до $500K)


Hugging Face	

transformers, trl, peft, diffusers, datasets

	

Полная открытость; внешние ревью и дискуссии приветствуются

	

Лиды научных групп (Leandro, Loubna, Beeching)

	

Hugging Face Fellowships, Community Grants


OpenAI	

openai-python, evals, triton, whisper

[cite: 14, 38]

	Модерируются штатными инженерами; фокус на строгости типов	Инженеры платформенного тулинга и компиляторов	

OpenAI Residency (6-месячный FTE-трек)


Qwen / Alibaba	

Qwen, Qwen-Agent, интеграции с vLLM

	Активное обсуждение квантования (AWQ, GPTQ) и инференса	Ведущие исследователи и инженеры Qwen команды	Alibaba Cloud MVP, Open Research Grants
DeepSeek	

FlashMLA, DeepSeek-V3, DualPipe

[cite: 36, 39]

	Репозитории служат артефактами релизов; ревью узкоспециализировано	Авторы CUDA-ядер и архитекторы распределенного обучения	Открытые инженерные конкурсы и прямые офферы
Mistral AI	mistral-common, mistral-inference	Ценятся компактные PR по типизации, токенизации и vLLM	Мейнтейнеры открытых форматов и форматов весов	Mistral Hackathon Grants, Исследовательские доступы
Cohere	

cohere-python, toolkit, Rerank-тулинг

	Фокус на корпоративных коннекторах и RAG-пайплайнах	Инженеры SDK и исследователи Cohere for AI	

Cohere for AI Research Scholars

Детализированные регламенты ключевых экосистем
Anthropic (Экосистема Model Context Protocol)

В репозиториях modelcontextprotocol/* действует жесткий нормативный регламент. Любые архитектурные модификации, добавление публичных API или изменение декораторов требуют предварительного создания issue и получения статуса одобрения от мейнтейнеров. Категорически запрещены несогласованные PR («PRs are for execution, not exploration»).

Особое внимание уделяется выявлению AI-агентов: официальный регламент обязывает явно декларировать использование языковых моделей одной строкой в описании PR («Disclose it. Own it»). Любые автономные «drive-by agents», открывающие issues или PR без глубокого осмысления автором-человеком, закрываются мгновенно, а учетные записи нарушителей подвергаются перманентному бану на уровне организации GitHub.

UK AI Safety / Security Institute (UK AISI)

Фреймворк inspect_ai и сопутствующий репозиторий inspect_evals являются государственным стандартом Великобритании для оценки рисков автономных моделей. С мая 2026 года процедура контрибьюции в репозиторий inspect_evals формализована: добавление новых бенчмарков переведено в реестр /register/ и требует обязательного указания ссылки на препринт arXiv с валидацией методологии, предоставления файла логов прогона /QA/*.json на передовых моделях и интеграции защитной строки c65e97c1-4e5b-401d-82f2-4581ce8f14d7 против загрязнения обучающих выборок.

Наибольшую ценность для института представляют сложные агентские среды оценки угроз (такие как AgentThreatBench, ориентированный на OWASP Top 10 для агентов, или модули тестирования внедрения промптов CodeIPI).

Google (DeepMind и Google Cloud)

В экосистеме Google действуют два основных механизма признания внешних специалистов:

Google Open Source Peer Bonus: Программа признания внешних инженеров, в рамках которой штатные сотрудники Google номинируют авторов полезных открытых вкладов на получение денежных премий и дипломов. Ежегодно награды получают свыше 140 специалистов из десятков стран. В ряде репозиториев (например, tensorflow/models) мейнтейнеры напрямую закрепляют возможность номинации на Peer Bonus за перенос научных статей в кодовую базу фреймворка.

Google Developer Experts (GDE) по направлению Machine Learning: Глобальный статус технического признания. Попадание в программу невозможно через прямую подачу: требуется персональная рекомендация (реферал) от действующего сотрудника Google или сертифицированного эксперта GDE. Кандидат проходит проверку портфолио и два этапа квалификационных интервью: глубокий технический ML-скрининг и проверку лидерского вклада в инженерное сообщество.

3. Каденс взаимодействий, пороги спама и алгоритмы антиабьюза GitHub

Уровень уверенности: Established

Системные лимиты платформы (Secondary Rate Limits)

При масштабировании активности с использованием AI-агентов ключевым фактором сохранения инфраструктурной работоспособности является соблюдение первичных и вторичных лимитов GitHub API.

Параметр платформы	Пороговое значение	Тип санкции	Механизм предотвращения
Генерация контента (Content Creation)	

≤80 действий/мин, ≤500 действий/час (PR, Issue, Review, Comment)

	

403 Forbidden, заголовок Retry-After, временная блокировка

	Ограничение частоты генерации до 1–2 действий в минуту на весь кластер
Интервал между мутациями	

Обязательная пауза ≥1.0 секунды между POST, PATCH, PUT, DELETE

[cite: 15]

	

Активация триггера abuse detection, сброс авторизации

	

Сериализация запросов через очередь с принудительной задержкой 1.5 с


Конкурентность (Concurrency)	

≤100 одновременных запросов (REST + GraphQL)

	

Включение вторичного рейт-лимита

	

Отказ от параллельного спама; строго последовательное исполнение


Вычислительные затраты (CPU Time)	

≤90 секунд процессорного времени на 60 секунд реального времени

	

Сброс API-сессии на 60 секунд

	

Использование условных GET-запросов с ETag (получение 304 Not Modified)

Человеческие пороги восприятия и теневая фильтрация

Нарушение негласных социальных норм репозитория ведет к скрытому флагированию учетной записи мейнтейнерами (скрытие комментариев с пометкой spam или disruptive, блокировка в issue).

Взаимодействие с отдельным инженером вендора строго ограничивается одним упоминанием (@-mention) в рамках первичного ревью. Повторный пинг допустим не ранее чем через 5–10 рабочих дней (в регламенте MCP официальный срок эскалации нерешенного тикета в Discord составляет 2 недели). Массовые упоминания нескольких мейнтейнеров в одном сообщении воспринимаются как токсичное давление и ведут к игнорированию.

На уровне одного репозитория безопасный объем активности для исследовательской лаборатории составляет не более 2–3 одновременно открытых PR и не более 1 нового issue в неделю. Пакетная отправка десятков мелких PR перегружает CI и классифицируется мейнтейнерами как спам-атака.

Влияние каденса на конверсию ответов

Эмпирические исследования задержки первого ответа мейнтейнеров (First Response Latency) на массивах открытых проектов демонстрируют четкие математические закономерности:

Временной фактор: PR и технические комментарии, поданные в понедельник и вторник в рабочем часовом поясе мейнтейнеров, получают отклик на 29–39% быстрее по сравнению с запросами, отправленными в конце недели или в выходные дни.

Фактор объема описания: Лаконичные, структурированные описания с явным указанием причинно-следственной связи бага и кратким планом тестирования (Test Plan) максимизируют скорость и конструктивность обратной связи.

Фактор предыдущей истории: Репутация автора, выраженная в оперативном реагировании на комментарии рецензентов в прошлых тредах, повышает приоритет его новых запросов в очереди мейнтейнера.

4. Метрики эффективности DevRel для малой исследовательской лаборатории

Уровень уверенности: Established для предиктивных параметров принятия PR; Emerging для адаптации модели Orbit к микро-командам

Для микро-лаборатории из двух исследователей и AI-агентов валовые метрики (количество звезд на собственных форках, общее число комментариев) являются ложными ориентирами. Фокус аналитики должен быть смещен на опережающие индикаторы высокого намерения (High-Intent Leading Indicators), отражающие движение ключевых сотрудников вендора по уровням вовлеченности.

                  [ УРОВЕНЬ 1: АДВОКАЦИЯ / ЧЕМПИОНСТВО ]
                 - Персональные приглашения в закрытые RFC
                 - Номинация на Peer Bonus / Гранты
                 - Прямые рефералы на интервью / партнерства
                                  ^
                                  |
               [ УРОВЕНЬ 2: ВТОРИЧНЫЙ КОНТАКТ (SECOND-TOUCH) ]
              - Повторный ответ того же инженера в новых тредах
              - Назначение ответственным (Assignee) на Issue
              - Пулл-реквесты со статусом "Ready for Work"
                                  ^
                                  |
            [ УРОВЕНЬ 3: ПЕРВИЧНЫЙ КОНТАКТ (HIGH-SIGNAL ENTRY) ]
           - Ответ инженера вендора в треде (конверсия 71-93%)
           - Одобрение тикета мейнтейнером перед написанием кода
                                  ^
                                  |
         [ УРОВЕНЬ 4: ПАССИВНЫЙ ОХВАТ (TOP-OF-FUNNEL / DISCOVERY) ]
        - Звезды репозитория, подписки, просмотры технического блога

Модифицированная модель Orbit для инженерной лаборатории

Orbit 4 (Top-of-Funnel / Discovery): Публичные репозитории лаборатории, препринты и статьи. Метрика — просмотры и цитирования сотрудниками целевых компаний.

Orbit 3 (High-Signal Entry): Первичный ответ инженера вендора в треде issue или PR (текущий пул лаборатории — 40 уникальных инженеров).

Orbit 2 (Second-Touch & Technical Co-Design): Повторное взаимодействие того же инженера в независимых тредах, назначение лаборатории исполнителем (Assignee) тикета, присвоение лейбла ready for work.

Orbit 1 (Internal Champions): Инженер вендора самостоятельно привлекает лабораторию к ревью закрытых веток, инициирует номинацию на Open Source Peer Bonus или передает рекомендацию в рекрутинг.

Предикторы оффера и принятие решений

Анализ предиктивных моделей принятия pull requests (P
accept
	​

), построенных на алгоритмах градиентного бустинга и случайных лесов по данным сотен тысяч репозиториев (AUC-ROC 0.94–0.95), выявляет решающие факторы результативности:

P
accept
	​

=
N
total PRs
	​

N
merged
	​

	​


Среди более чем 20 измеряемых признаков наибольший статистический вес имеют:

Наличие связанного issue (Linked Issue Presence): Пулл-реквесты, логически привязанные к предварительно согласованному issue, показывают многократный прирост вероятности мержа по сравнению с изолированными инициативами (что полностью подтверждает замеренный лабораторией нулевой отклик на холодные PR).

Метрика повторного касания (Second-Touch Rate): Возврат того же инженера к диалогу в 3+ независимых задачах служит главным опережающим индикатором формирования доверия и последующего реферала.

Размер дельты кода (PR Delta Size): Пулл-реквесты объемом до 25 строк с фокусом на одном изменении демонстрируют минимальное время резолюции и максимальный процент одобрения.

Глубина технической дискуссии (Thread Depth): Диалог глубиной от 4 содержательных реплик по краевым случаям архитектуры переводит взаимодействие из плоскости формального триажа в плоскость профессионального партнерства.

5. Рычаг контента: влияние технических Write-Ups на реакцию мейнтейнеров

Уровень уверенности: Emerging

Механика взаимного усиления кода и публикаций

Публикация внешних исследовательских материалов (статей на arXiv, глубоких технических разборов в блогах и структурированных тредов в инженерных сетях) действует как мультипликатор доверия к открытому коду.

Артефакт контента	Основная платформа	Сигнал для экосистемы	Влияние на реакцию мейнтейнеров
arXiv Preprint / Whitepaper	arXiv, Google Scholar	

Академическая строгость, доказанная методология

	

Обязательное условие для включения в бенчмарки UK AISI; прямой интерес исследователей


Deep-Dive Post-Mortem / PR Breakdown	Собственный блог, Substack	

Способность решать нетривиальные системные сбои

	

Сокращение времени первого отклика; ссылка прикрепляется к описанию Issue/PR


Архитектурный тред / Visual Guide	X (Twitter), LinkedIn	

Умение транслировать ценность сложных технологий

	

Привлечение внимания лидеров DevRel-направлений, триггер для номинаций GDE/MVP

Эмпирический эффект контента базируется на трех механизмах:

Защита от презумпции сгенерированного мусора: В условиях переполнения платформ поверхностным AI-кодом подробный публичный разбор архитектурных компромиссов, профилирования памяти и граничных условий однозначно подтверждает человеческий контроль над решением.

Соответствие институциональным требованиям: Регламент UK AISI Inspect Evals 2026 года прямо требует предоставления препринта для регистрации внешних бенчмарков, превращая контент в обязательный квалификационный критерий.

Обнаружение нанимающими руководителями: Менеджеры по найму в распределенных инженерных командах систематически сканируют публикации с разбором решений для прямого выхода на авторов.

6. Анти-кейсы: стратегии, разрушающие репутацию в сообществе

Уровень уверенности: Established

Попытки форсировать заметность через агрессивную автоматизацию и поверхностные комментарии приводят к необратимым репутационным потерям.

Антипаттерн	Механика сбоя	Реальный прецедент и последствия
1. AI Slop & Галлюцинации в баг-репортах	

Отправка сгенерированных отчетов об ошибках без ручной проверки

	

Кризис в проекте cURL: закрытие публичной программы вознаграждений 01.02.2026


2. Автономные Drive-By PRs	

Запуск агентов, открывающих PR по чужим issues без участия человека

	

Регламент MCP (Anthropic): мгновенный бан аккаунтов и всей организации


3. Холодные монолитные PR	

Отправка сотен строк кода без согласования архитектуры в issue

	

Подтверждено замером: 0% откликов, закрытие с меткой not planned

[cite: 17, 20]


4. Спам-пинг мейнтейнеров	

Тегирование 3+ инженеров с запросами «Please review» через 24 часа

	

Теневой флаг спама, скрытие комментариев, блокировка в обсуждениях


5. Фарминг косметических правок	

Массовые микро-PR по правке точек, опечаток и форматирования

	

Репутация мусорного контрибьютора, отказ в грантах и программах Peer Bonus

Разбор кейса cURL: Закрытие Bug Bounty из-за AI Slop

Проект cURL под руководством Даниэля Стенберга (более 30 миллиардов установок) столкнулся с системным кризисом, вызванным наплывом сгенерированных LLM отчетов о псевдо-уязвимостях. Авторы отчетов скармливали исходный код языка Си генеративным моделям и отправляли галлюцинации в надежде на выплаты. В результате 66% всех входящих заявок не содержали реальных проблем, но требовали колоссальных затрат времени ведущих инженеров на проверку.

1 февраля 2026 года мейнтейнеры были вынуждены полностью закрыть публичную программу Bug Bounty на GitHub, а репутация авторов, применявших автоматическую генерацию, была навсегда аннулирована.

Разбор кейса Anthropic / MCP: Институциональный бан агентов

В экосистеме Model Context Protocol всплеск активности автономных ботов, парсивших открытые задачи и создававших пустые PR, привел к внедрению правила нулевой толерантности. Мейнтейнеры официально закрепили в руководстве по контрибьюции: любые пулл-реквесты от неавторизованных автономных агентов закрываются без рассмотрения, а профили нарушителей блокируются на уровне всей организации.

7. Недельный операционный плейбук микро-лаборатории

Регламент оптимизирован для команды из 2 исследователей и AI-агентов. Он устанавливает предельные потолки активности, исключающие риски вторичных рейт-лимитов GitHub и гарантирующие высокую ценность каждого контакта.

Матрица недельных потолков активности по вендорам
Экосистема / Вендор	Фокусные репозитории	Лимит новых Issue / нед.	Лимит новых PR / нед.	Потолок упоминаний (@-mentions)	Приоритетный тип вклада
Anthropic	

modelcontextprotocol/*

[cite: 32]

	1–2	

1 (строго после одобрения)

	0 (только ответ мейнтейнеру)	

SDK тайпинги, валидация FastMCP


UK AISI	

inspect_evals, inspect_ai

[cite: 10, 11]

	1	

1 сложный бенчмарк (с QA)

	1 ответственный ревьюер	

OWASP Agentic Security, инъекции


Google / DeepMind	

tensorflow/models, gemma

	1	

1 (фокус на Peer Bonus)

	0	

Перенос архитектур статей, багфиксы


Microsoft	

semantic-kernel, csharp-sdk

[cite: 32]

	2	1	1 DevRel контакт	Коннекторы корпоративных сред
Meta	

llama, pytorch, executorch

[cite: 37]

	1	1	0	Оптимизация инференса, RAG-инструменты
Hugging Face	

transformers, trl, peft

	2	2	1 мейнтейнер	

Оптимизация методов постобучения


OpenAI	

openai-python, evals

	1	1	0	Исправление граничных случаев в SDK
Qwen / Alibaba	

Qwen, интеграции vLLM

	2	1	1 инженер Alibaba	

Квантованные ядра, оптимизация KV-кэша


DeepSeek	

FlashMLA, DualPipe

[cite: 39]

	1	1 (строго проверенный фикс)	0	

Специфические оптимизации под GPU


Mistral AI	mistral-common	1	1	0	Совместимость форматов токенизации
Cohere	

cohere-python, toolkit

[cite: 40]

	1	1	0	

Коннекторы семантического поиска

Структура недельного спринта

Понедельник (Запуск цикла и подача решений):

Инициация 2–3 новых технических issues с предварительным анализом граничных случаев в репозиториях европейского и американского регионов.

Отправка пулл-реквестов, прошедших локальное сквозное тестирование в изолированных контейнерах (максимизация фактора быстрой реакции в начале недели).

Вторник — Среда (Обработка ревью и реализация бенчмарков):

Обработка замечаний мейнтейнеров с целевым временем ответа до 2–4 часов.

Развертывание тестовых песочниц k8s и валидация бенчмарков для UK AISI Inspect.

Подготовка 1 глубокого публичного технического разбора (Write-Up) по решаемой проблеме.

Четверг (Second-Touch и аудит чужих PR):

Участие в качестве независимого рецензента в чужих сложных PR (выявление утечек памяти, нарушений контрактов API в vLLM и MCP).

Публикация технического разбора в X/LinkedIn с отметкой upstream issue.

Пятница (Консолидация и обновление Orbit Dashboard):

Мониторинг незавершенных тредов. Полный мораторий на открытие новых холодных PR во избежание зависания на выходные.

Фиксация инженеров, совершивших Second-Touch, и планирование совместных активностей на следующую неделю.

8. Топ-5 критических ошибок, разрушающих заметность

Холодные пулл-реквесты без предварительного согласования в Issue:

Механизм: Отправка объемного кода без предварительного публичного обсуждения архитектуры с командой проекта.

Последствие: Нулевой отклик, закрытие мейнтейнерами с пометкой not planned, напрасная трата инженерного времени лаборатории.

Нераскрытое использование AI-агентов (AI Slop & Drive-By PRs):

Механизм: Публикация кода или баг-репортов, сгенерированных языковыми моделями без глубокого ручного аудита и прогона локальных тестов.

Последствие: Моментальная идентификация мейнтейнерами, перманентный бан учетных записей и всей организации (как в экосистеме MCP), попадание в негласные черные списки.

Пренебрежение вторичными лимитами GitHub и спам-пинг мейнтейнеров:

Механизм: Запуск параллельных скриптов без секундных пауз и массовое тегирование мейнтейнеров с просьбами о срочном ревью.

Последствие: Получение системной ошибки HTTP 403 (Secondary Rate Limit), скрытие комментариев как спама и утрата доверия инженеров вендора.

Фокус на косметических правках (Trivial PR Farming):

Механизм: Попытка нарастить формальное число коммитов через исправление форматирования, опечаток в README и мелких комментариев.

Последствие: Формирование репутации мусорного контрибьютора, дисквалификация из программ признания вроде Google Open Source Peer Bonus и нулевой интерес со стороны нанимающих менеджеров.

Отсутствие работы над метрикой Second-Touch и распыление усилий:

Механизм: Однократное комментирование в десятках различных репозиториев без повторного возвращения к ответившим инженерам.

Последствие: Наличие сотен поверхностных контактов при нулевой глубине отношений; отсутствие внутренних защитников (Champions), способных инициировать оффер или грант.

Стратегическое резюме

Текущие показатели лаборатории (71–93% первичного отклика на технические комментарии в чужих тредах при 40 уникальных ответивших инженерах ведущих вендоров) представляют собой исключительно высокий конверсионный результат, подтверждающий актуальность и глубину инженерной мысли.

Для успешной трансформации этого внимания в формальные офферы, гранты (Meta Llama Impact Grants до $500K, OpenAI Residency) и номинации (Google Open Source Peer Bonus, GDE) лаборатории необходимо:

Полностью прекратить создание холодных PR, направив 100% инженерного потенциала на совместное проектирование решений через предварительно согласованные issues (модель Co-Design).

Сконцентрироваться на показателе Second-Touch, выстраивая регулярную работу с уже ответившим пулом инженеров Anthropic, Google, UK AISI и OpenAI через решение их системных блокеров.

Упаковывать ключевые программные решения в строгие рецензируемые артефакты (статьи на arXiv для бенчмарков UK AISI, архитектурные разборы в блогах), создавая неоспоримое доказательство экспертного уровня для нанимающих руководителей.

Sources used in the report
researchgate.net
(PDF) Matching GitHub Developer Profiles to Job Advertisements - ResearchGate
Opens in a new window
arxiv.org
Towards Evidence-Based Tech Hiring Pipelines - arXiv
Opens in a new window
findjob.live
Networking for Remote & Freelance Roles - Career Launchpad | Findjob.Live
Opens in a new window
reddit.com
AMA with Hugging Face Science, the team behind SmolLM, SmolVLM, Fineweb and more. : r/LocalLLaMA - Reddit
Opens in a new window
sequoiacap.com
Dhanji Prasanna: Transforming Block with Open Source Goose | Sequoia Capital
Opens in a new window
developers.google.com
Google Developer Experts
Opens in a new window
alainchautard.com
How to become a Google Developer Expert (GDE)? - Alain Chautard
Opens in a new window
medium.com
Being an ML Google Developer Expert | by Margaret Maynard-Reid - Medium
Opens in a new window
partners.cloud.google.com
Google Cloud Ambassadors
Opens in a new window
qaskills.sh
Inspect AI Evals Tutorial (UK AISI) — 2026 Guide - QASkills.sh
Opens in a new window
github.com
UKGovernmentBEIS/inspect_evals: Collection of evals for Inspect AI · GitHub
Opens in a new window
ukgovernmentbeis.github.io
UK AISI Autonomous Systems Evaluation Standard
Opens in a new window
arxiv.org
Developing and Maintaining an Open-Source Repository of AI Evaluations: Challenges and Insights - arXiv
Opens in a new window
builtinsf.com
Residency 2026 - OpenAI | Built In San Francisco
Opens in a new window
docs.github.com
Best practices for using the REST API - GitHub Docs
Opens in a new window
docs.github.com
Rate limits for the REST API - GitHub Docs
Opens in a new window
emergentmind.com
Pull Request Acceptance Analysis - Emergent Mind
Opens in a new window
researchgate.net
Predicting the First Response Latency of Maintainers and Contributors in Pull Requests
Opens in a new window
arxiv.org
Governance Matters: Lessons from Restructuring the data.table OSS Project - arXiv
Opens in a new window
github.com
CONTRIBUTING.md - modelcontextprotocol/python-sdk - GitHub
Opens in a new window
opensource.googleblog.com
Announcing the second group of Open Source Peer Bonus winners in 2022
Opens in a new window
loicmathieu.fr
I am a Google Developer Expert (GDE) on Google Cloud Platform (GCP) technologies
Opens in a new window
github.com
Community requests: New paper implementations · Issue #8635 · tensorflow/models - GitHub
Opens in a new window
daniel.haxx.se
The I in LLM stands for intelligence | daniel.haxx.se
Opens in a new window
daniel.haxx.se
High-Quality Chaos | daniel.haxx.se
Opens in a new window
github.com
vllm-omni/.claude/skills/review-pr/references/delivery/maintainer-style-study.md at main ... - GitHub
Opens in a new window
blog.n8n.io
Migrating Community Metrics to Orbit using n8n
Opens in a new window
developerrelations.com
Using the Orbit model to demonstrate community value - Developer Relations
Opens in a new window
grantedai.com
Llama Impact Grant (2026) - Granted AI
Opens in a new window
impactopen.org
ImpactOpen — Every open AI-for-good opportunity, in one place
Opens in a new window
github.com
GitHub - UKGovernmentBEIS/inspect_k8s_sandbox: A Kubernetes sandbox environment for use with inspect_ai
Opens in a new window
github.com
Model Context Protocol - GitHub
Opens in a new window
github.com
Specification and documentation for the Model Context Protocol - GitHub
Opens in a new window
modelcontextprotocol.io
Contributing to MCP - Model Context Protocol
Opens in a new window
github.com
Practical AI safety engineering: evaluation frameworks and red-teaming tools for assessing frontier AI systems - GitHub
Opens in a new window
github.com
GitHub - vllm-project/vllm: A high-throughput and memory-efficient inference and serving engine for LLMs
Opens in a new window
developer.meta.com
Open Source AI: Driving AI Innovation and Economic Growth | Llama - Meta for Developers
Opens in a new window
daniel.haxx.se
Mythos finds a curl vulnerability | daniel.haxx.se
Opens in a new window
github.com
Pull requests · deepseek-ai/FlashMLA - GitHub
Opens in a new window
buttondown.com
[AINews] How To Scale Your Model, by DeepMind - Buttondown
Opens in a new window
github.com
Add AgentThreatBench: OWASP Agentic Top 10 security evaluation suite (UK AISI / inspect_evals) · Issue #3776 · EleutherAI/lm-evaluation-harness - GitHub
Opens in a new window
opensource.googleblog.com
2024 - Google Open Source Blog
Opens in a new window
discuss.google.dev
regarding a new member - Compute Infrastructure - Google Developer forums
Opens in a new window
markepear.dev
Developer marketing metrics and attribution (practitioners guide)
Opens in a new window
insightpartners.com
Measuring the Impact of a Community-Led Growth Motion | Insight Partners
Opens in a new window
daniel.haxx.se
A human in control | daniel.haxx.se
Opens in a new window
freecodecamp.org
What to Do If You Don't Get into Outreachy: Exploring Other Paths to Open Source
Opens in a new window
daniel.haxx.se
The pressure | daniel.haxx.se
Opens in a new window
github.com
modelcontextprotocol/python-sdk at deep-kondah.com - GitHub
Opens in a new window
Sources read but not used in the report
arxiv.org
An Empirical Study of the Evolution of GitHub Actions Workflows - arXiv
Opens in a new window
github.com
GitHub - megagonlabs/nlp4hr-survey: A list of literature and resources related to the application of natural language processing to challenges in human resources.
Opens in a new window
computer.org
Matching GitHub Developer Profiles to Job Advertisements
Opens in a new window
github.com
karthikrshet/Career-Agents: The Open-Source AI Career Operating System. 167 Specialized AI Agents across 19 Divisions, Spoken Voice Lab (27 Languages), AI job search ,ATS Resume Studio, FAANG Interview Tracks, GitHub Portfolio Auditor, Model Context Protocol (MCP) Server, and Career Knowledge Graph.
Opens in a new window
researchgate.net
Towards Evidence-Based Tech Hiring Pipelines - ResearchGate
Opens in a new window
se.cs.uni-saarland.de
Predicting Contributions in Open-Source Software Projects - Chair of
Opens in a new window
github.com
JobSentinel/docs/references.md at main · cboyd0319 ... - GitHub
Opens in a new window
anthropic.com
Home \ Anthropic
Opens in a new window
developers.google.com
Rules of Machine Learning: | Google for Developers
Opens in a new window
acesso.kissmyscore.com
Google Developer Experts - Google for Developers
Opens in a new window
github.com
inspect_k8s_sandbox/README.md at main - GitHub
Opens in a new window
schmatz.github.io
Implementing a Deception Eval with the UK AISI's Inspect Framework
Opens in a new window
arxiv.org
Addressing OSS Community Managers' Challenges in Contributor Retention - arXiv
Opens in a new window
ime.usp.br
Investigating the Impact of Interpersonal Challenges on ... - IME-USP
Opens in a new window
github.com
Security - modelcontextprotocol/python-sdk - GitHub
Opens in a new window
github.com
modelcontextprotocol/actions: GitHub Actions relevant to the management of MCP repositories.
Opens in a new window
salary.com
How to Establish a Winning Employee Milestone Bonus Program - Salary.com
Opens in a new window
secchi.io
10+ Employee Recognition Program Ideas and Examples - secchi.io
Opens in a new window
iamrajivranjansingh.medium.com
Google Summer of Code 2026 - Medium
Opens in a new window
daniel.haxx.se
A new breed of analyzers | daniel.haxx.se
Opens in a new window
daniel.haxx.se
Daniel Stenberg | daniel.haxx.se
Opens in a new window
daniel.haxx.se
Open Source security in spite of AI | daniel.haxx.se
Opens in a new window
github.com
vllm/docs/contributing/model/multimodal.md at main - GitHub
Opens in a new window
github.com
vllm-ascend/CONTRIBUTING.md at main - GitHub
Opens in a new window
github.com
vllm-metal/CONTRIBUTING.md at main - GitHub
Opens in a new window
github.com
vllm/docs/contributing/profiling.md at main - GitHub
Opens in a new window
github.com
vllm-omni/CONTRIBUTING.md at main - GitHub
Opens in a new window
github.com
vllm/docs/contributing/editing-agent-instructions.md at main - GitHub
Opens in a new window
github.com
vllm/CONTRIBUTING.md at main - GitHub
Opens in a new window
github.com
[Doc]: Placeholder CONTRIBUTING.md leaves new contributors without a checklist before they hit ReadTheDocs #3520 - GitHub
Opens in a new window
github.com
Evaluate and challenge DevRel metrics · Issue #293 · meilisearch
Opens in a new window
news.ycombinator.com
Show HN: Coding Career Handbook | Hacker News
Opens in a new window
talents.vaia.com
Ambassador - Part-time in West Jordan at Cohere | Apply now! - Talents by Vaia
Opens in a new window
lightreading.com
Vodafone favorite Cohere may finally be set to disrupt telecom - Light Reading
Opens in a new window
ziprecruiter.com
Cohere Ambassador Job in Puyallup, WA - ZipRecruiter
Opens in a new window
grantedai.com
Llama Impact Grants (2026) - Granted AI
Opens in a new window
www2.fundsforngos.org
Call for Proposals: Llama Impact Grants - fundsforNGOs
Opens in a new window
respan.ai
Accelerators & AI Grants Finder - Respan
Opens in a new window
Thoughts
Определение структуры исследования и стартовых гипотез
Формирую комплексный план анализа эффективности DevRel и стратегий заметности в открытом исходном коде для крупнейших AI-вендоров. Приступаю к декомпозиции задач: от отслеживания цепочек конверсии озерных вкладов в офферы до оценки рисков автоматических банов и спам-фильтров платформы GitHub.
Фокусные направления поиска и сбор данных
Проектирую серию поисковых направлений, включающих исследование партнерских программ и стипендий (OpenAI Residency, Google GDE, Hugging Face Fellows), поиск эмпирических исследований по глубине обсуждений и конверсии второстепенных касаний, а также анализ резонансных анти-кейсов использования AI-ботов для ревью кода.
Определение ближайших шагов
Начинаю сбор фактических данных по воронкам найма через open-source, критериям участия в программах признания вендоров и официальным алгоритмам обнаружения подозрительной активности на платформах разработки.
arxiv.org
An Empirical Study of the Evolution of GitHub Actions Workflows - arXiv
github.com
GitHub - megagonlabs/nlp4hr-survey: A list of literature and resources related to the application of natural language processing to challenges in human resources.
computer.org
Matching GitHub Developer Profiles to Job Advertisements
researchgate.net
(PDF) Matching GitHub Developer Profiles to Job Advertisements - ResearchGate
arxiv.org
Towards Evidence-Based Tech Hiring Pipelines - arXiv
github.com
karthikrshet/Career-Agents: The Open-Source AI Career Operating System. 167 Specialized AI Agents across 19 Divisions, Spoken Voice Lab (27 Languages), AI job search ,ATS Resume Studio, FAANG Interview Tracks, GitHub Portfolio Auditor, Model Context Protocol (MCP) Server, and Career Knowledge Graph.
researchgate.net
Towards Evidence-Based Tech Hiring Pipelines - ResearchGate
se.cs.uni-saarland.de
Predicting Contributions in Open-Source Software Projects - Chair of
github.com
JobSentinel/docs/references.md at main · cboyd0319 ... - GitHub
anthropic.com
Home \ Anthropic
Декомпозиция ключевых направлений исследования
Для систематизации поиска декомпозирую шесть целевых вопросов на измеримые и конкретные аспекты. Оцениваю параметры конверсии OSS-активности в офферы, специфику работы с экосистемами ведущих ИИ-вендоров (включая Anthropic, OpenAI, DeepMind, Microsoft, Meta и UK AISI), а также риски превышения каденса касаний на GitHub. Также формирую структуру для анализа эффективности публикаций технического контента и сбора реальных анти-кейсов.
Анализ метрик и механизмов защиты платформ
Изучаю пороги спам-фильтрации GitHub, включая вторичные лимиты запросов и алгоритмы обнаружения автоматизированной активности, чтобы определить безопасную частоту взаимодействий с мейнтейнерами. Параллельно прорабатываю критерии DevRel-метрик для малых лабораторий, уделяя внимание показателям повторного отклика (second-touch rate) и глубине дискуссионных веток.
Дальнейшие действия по сбору эмпирических данных
Перехожу к точечному поиску кейсов найма через открытый исходный код, программ партнерства и резидентуры ключевых LLM-вендоров, а также задокументированных случаев потери репутации из-за неконструктивной активности в открытых репозиториях.
findjob.live
Networking for Remote & Freelance Roles - Career Launchpad | Findjob.Live
reddit.com
AMA with Hugging Face Science, the team behind SmolLM, SmolVLM, Fineweb and more. : r/LocalLLaMA - Reddit
sequoiacap.com
Dhanji Prasanna: Transforming Block with Open Source Goose | Sequoia Capital
developers.google.com
Google Developer Experts
developers.google.com
Rules of Machine Learning: | Google for Developers
alainchautard.com
How to become a Google Developer Expert (GDE)? - Alain Chautard
partners.cloud.google.com
Google Cloud Ambassadors
discuss.google.dev
regarding a new member - Compute Infrastructure - Google Developer forums
medium.com
Being an ML Google Developer Expert | by Margaret Maynard-Reid - Medium
acesso.kissmyscore.com
Google Developer Experts - Google for Developers
Анализ сигналов найма и критериев признания
Отмечаю интересные закономерности в воронках найма ключевых AI-компаний: публикация технических разборов и систематические патчи к экосистемным библиотекам часто служат прямым триггером для прохождения отбора без стандартного скрининга. Анализируя формальные программы признания (включая инициативы Google и Microsoft), фиксирую жесткие требования к рефералам и доказанному публичному влиянию на комьюнити.
Систематизация рисков и платформенных ограничений
Прорабатываю проблематику спам-фильтрации на платформах разработки и в сообществах вендоров. Оцениваю тонкую грань между высокой активностью и потерей репутационного капитала, опираясь на зафиксированные публичные анти-кейсы автоматических ревью и некорректных патчей.
Формирование плана точечных поисковых запросов
Направляю основные усилия на углубленный сбор данных по всем 11 целевым вендорам, изучение жестких лимитов GitHub API/Abuse limits, получение академических метрик повторного взаимодействия (second-touch rate) и оценку конверсии публичных статей о PR в отклики мейнтейнеров.
qaskills.sh
Inspect AI Evals Tutorial (UK AISI) — 2026 Guide - QASkills.sh
github.com
UKGovernmentBEIS/inspect_evals: Collection of evals for Inspect AI · GitHub
github.com
GitHub - UKGovernmentBEIS/inspect_k8s_sandbox: A Kubernetes sandbox environment for use with inspect_ai
ukgovernmentbeis.github.io
UK AISI Autonomous Systems Evaluation Standard
github.com
inspect_k8s_sandbox/README.md at main - GitHub
schmatz.github.io
Implementing a Deception Eval with the UK AISI's Inspect Framework
arxiv.org
Developing and Maintaining an Open-Source Repository of AI Evaluations: Challenges and Insights - arXiv
github.com
Add AgentThreatBench: OWASP Agentic Top 10 security evaluation suite (UK AISI / inspect_evals) · Issue #3776 · EleutherAI/lm-evaluation-harness - GitHub
github.com
Practical AI safety engineering: evaluation frameworks and red-teaming tools for assessing frontier AI systems - GitHub
builtinsf.com
Residency 2026 - OpenAI | Built In San Francisco
Выявление стандартов и программ ведущих ИИ-институтов
Синтезирую новые данные по спецификациям и программам участия ключевых вендоров. Отмечаю жесткие регламенты интеграции в экосистему оценки безопасности UK AISI (Inspect AI), где требуется строгое соответствие авто-тестам, наличие доказательств проведенного QA и canary-строк. Также фиксирую формальные требования к кандидатам в исследовательской программе OpenAI Residency, где ключевым критерием отбора выступает демонстрация высокой скорости обучения и самостоятельная работа над open-source проектами.
Систематизация алгоритмов отбора и критериев сообществ
Сопоставляю институциональные стандарты с практиками ревью в крупнейших репозиториях. Анализирую разрыв между приемом контрибуций в малые инструментальные модули и строгим регламентом крупных организаций, где прямые PR от внешних участников без предваряющего обсуждения отсекаются или остаются без ответа.
Организация серии точечных поисковых запросов
Планирую масштабную серию точечных поисковых сессий. Направляю фокус на выявление лимитов GitHub (secondary rate limits, shadow-flagging), эмпирических исследований по удержанию разработчиков и метрикам second-touch rate, правил работы в сообществах specific LLM-вендоров (Anthropic, Meta, DeepSeek, Mistral), а также на поиск задокументированных анти-кейсов ИИ-спама в открытых проектах.
docs.github.com
Rate limits for the REST API - GitHub Docs
docs.github.com
Best practices for using the REST API - GitHub Docs
arxiv.org
Addressing OSS Community Managers' Challenges in Contributor Retention - arXiv
researchgate.net
Predicting the First Response Latency of Maintainers and Contributors in Pull Requests
emergentmind.com
Pull Request Acceptance Analysis - Emergent Mind
arxiv.org
Governance Matters: Lessons from Restructuring the data.table OSS Project - arXiv
ime.usp.br
Investigating the Impact of Interpersonal Challenges on ... - IME-USP
github.com
Pull requests · deepseek-ai/FlashMLA - GitHub
Ограничения платформы и эмпирические закономерности PR
Выявляю жесткие платформенные пороги GitHub: документация по вторичным лимитам API строго ограничивает создание контента (не более 80 мутирующих запросов в минуту и 500 в час), а между сериями действий требуются интервалы. Сопоставляю это с академическими исследованиями принятия PR, где размер изменений (оптимально до 25 строк) и скорость первого ответа мейнтейнера напрямую коррелируют с удержанием контрибьюторов и итоговым мержем.
Анализ реакций на автоматизированный шум
Изучаю прецеденты негативного восприятия ИИ-сгенерированных пулл-реквестов и комментариев крупными OSS-проектами (включая публичную реакцию Даниэля Стенберга и сообщества Linux). Негативная реакция мейнтейнеров на низкокачественные или автоматически созданные патчи подтверждает гипотезу о необходимости глубокой предварительной валидации перед отправкой.
Дальнейшие фокусированные запросы
Перехожу к детализированному сбору информации по специфическим экосистемам каждого из вендоров: изучаю динамику обсуждений в репозиториях Anthropic, OpenAI, DeepSeek, Mistral, Qwen и UK AISI, а также механику конверсии публичных статей о PR в отклики мейнтейнеров.
github.com
CONTRIBUTING.md - modelcontextprotocol/python-sdk - GitHub
github.com
modelcontextprotocol/python-sdk at deep-kondah.com - GitHub
github.com
Security - modelcontextprotocol/python-sdk - GitHub
github.com
modelcontextprotocol/actions: GitHub Actions relevant to the management of MCP repositories.
github.com
Model Context Protocol - GitHub
modelcontextprotocol.io
Contributing to MCP - Model Context Protocol
github.com
Specification and documentation for the Model Context Protocol - GitHub
Правила участия и политика AI-вкладов в ключевых проектах
Анализируя регламенты крупнейших экосистемных репозиториев (включая Model Context Protocol), фиксирую строгие требования к прозрачности: авторы обязаны явно декларировать использование ИИ-ассистентов, брать полную ответственность за предложенные изменения и вступать в диалог без применения авто-ответчиков. Автоматическая рассылка предложений или неразглашение ИИ-генерации приводит к моментальному закрытию запросов и блокировке в организации. Кроме того, подтверждается жесткое правило: любые существенные изменения требуют предварительного утверждения через проблему (issue), а 'холодные' внесения кода отклоняются.
Выявление механизмов модерации и репутационных барьеров
Сопоставляю новые регламенты с рисками отторжения внешних контрибуций мейнтейнерами. Оцениваю, как формальные политики проектов защищают команду от некачественных ИИ-вкладов и создают фильтры, через которые проходят только участники, погруженные в контекст архитектуры и способные лично аргументировать решения в комментариях.
Детализация поиска по программам признания и метрикам
Планирую проверить критерии программ профессионального признания (Google Open Source Peer Bonus, Microsoft MVP для AutoGen и Semantic Kernel, программы Cohere и Meta), оценить влияние концепции 'публичного обучения' на видимость для рекрутеров, а также собрать конкретные кейсы блокировок за неадекватное использование ИИ в известных Linux-сообществах.
loicmathieu.fr
I am a Google Developer Expert (GDE) on Google Cloud Platform (GCP) technologies
freecodecamp.org
What to Do If You Don't Get into Outreachy: Exploring Other Paths to Open Source
salary.com
How to Establish a Winning Employee Milestone Bonus Program - Salary.com
secchi.io
10+ Employee Recognition Program Ideas and Examples - secchi.io
iamrajivranjansingh.medium.com
Google Summer of Code 2026 - Medium
opensource.googleblog.com
Announcing the second group of Open Source Peer Bonus winners in 2022
opensource.googleblog.com
2024 - Google Open Source Blog
github.com
Community requests: New paper implementations · Issue #8635 · tensorflow/models - GitHub
Механики признания и наград в ИИ-экосистемах
Синтезирую данные по системам поощрения независимых разработчиков со стороны ключевых технологических гигантов. Отмечаю особенности программы Google Open Source Peer Bonus: выдвижение участников происходит напрямую сотрудниками компании за внесение ценного вклада в экосистему, что открывает прямую связь с инженерами вендора и часто выступает мостом к получению статуса эксперта (GDE). Фиксирую, что ключевым фактором номинации является не объем кода, а решение архитектурных проблем и качественное техническое описание PR.
Интеграция корпоративных программ и экспертных сообществ
Анализирую взаимосвязь между регулярным ревью сторонних репозиториев и получением официального статуса эксперта в экосистемах Microsoft и Google. Разбираю цепочку конверсии: от регулярной поддержки узкоспециализированных библиотек до получения рекомендаций от внутренних инженеров вендоров и последующего приглашения в закрытые группы разработчиков.
Планирование серии поисковых сессий по рискам и сообществам
Направляю следующие поисковые действия на детальный разбор норм поведения и барьеров в сообществах Mistral и DeepSeek, изучение нюансов категорий программы Microsoft MVP в области ИИ, а также на сбор академических исследований MSR/ICSE по метрикам задержки ревью и анти-кейсов ИИ-генерированного спама.
daniel.haxx.se
The I in LLM stands for intelligence | daniel.haxx.se
daniel.haxx.se
The pressure | daniel.haxx.se
daniel.haxx.se
A new breed of analyzers | daniel.haxx.se
daniel.haxx.se
Daniel Stenberg | daniel.haxx.se
daniel.haxx.se
Mythos finds a curl vulnerability | daniel.haxx.se
daniel.haxx.se
High-Quality Chaos | daniel.haxx.se
daniel.haxx.se
A human in control | daniel.haxx.se
daniel.haxx.se
Open Source security in spite of AI | daniel.haxx.se
Углубленный анализ реакции проектов на AI-генерацию
Изучаю детальную хронику восприятия ИИ-анализаторов в ключевых инфраструктурных проектах. На примечательных примерах вижу сдвиг в отношении мейнтейнеров: от резкого отторжения низкокачественного генеративного 'мусора' (slop) до признания высокой ценности профильных, выверенных человеком отчетов от специализированных ИИ-агентов. Важнейший вывод: ценность взаимодействия определяется не фактом применения ИИ, а качеством предварительной проверки и отсутствием ложных срабатываний.
Перегрузка ведущих разработчиков и барьеры репутации
Анализирую нагрузки на ключевых мейнтейнеров открытого кода: обработка некачественных автоматически созданных отчетов отнимает ресурсы от реальной разработки и приводит к закрытию публичных программ вознаграждения. Это подтверждает гипотезу о том, что для построения доверительных отношений с крупными ИИ-вендорами критически важен жесткий фильтр собственного контента и избегание любых признаков авто-спама.
Финальная детализация по экосистемам и метрикам
Направляю завершающую серию запросов на уточнение правил участия в ключевых открытых репозиториях (vLLM, Llama), изучение метрик оценки DevRel-эффективности для малых команд (включая модель Orbit) и анализ концепции публичной демонстрации результатов разработки для прямого найма.
github.com
vllm/docs/contributing/model/multimodal.md at main - GitHub
github.com
vllm-ascend/CONTRIBUTING.md at main - GitHub
github.com
vllm-metal/CONTRIBUTING.md at main - GitHub
github.com
vllm/docs/contributing/profiling.md at main - GitHub
github.com
vllm-omni/CONTRIBUTING.md at main - GitHub
github.com
vllm/docs/contributing/editing-agent-instructions.md at main - GitHub
github.com
vllm/CONTRIBUTING.md at main - GitHub
github.com
[Doc]: Placeholder CONTRIBUTING.md leaves new contributors without a checklist before they hit ReadTheDocs #3520 - GitHub
github.com
GitHub - vllm-project/vllm: A high-throughput and memory-efficient inference and serving engine for LLMs
github.com
vllm-omni/.claude/skills/review-pr/references/delivery/maintainer-style-study.md at main ... - GitHub
blog.n8n.io
Migrating Community Metrics to Orbit using n8n
insightpartners.com
Measuring the Impact of a Community-Led Growth Motion | Insight Partners
developerrelations.com
Using the Orbit model to demonstrate community value - Developer Relations
markepear.dev
Developer marketing metrics and attribution (practitioners guide)
github.com
Evaluate and challenge DevRel metrics · Issue #293 · meilisearch
news.ycombinator.com
Show HN: Coding Career Handbook | Hacker News
Стилистика ревью и внутренние регламенты vLLM
Анализирую практические руководства по проведению пулл-реквестов в экосистеме vLLM. Отмечаю, что мейнтейнеры предпочитают лаконичные и прямые комментарии без вводных приветствий, сфокусированные на конкретных триггерах сбоев и границах контрактов. Пространные объяснения допускаются только при изменении архитектуры. Также фиксирую применение автоматизированных инструментов предварительной проверки конвенций проекта перед отправкой кода.
Адаптация модели Orbit и метрики DevRel для небольших команд
Исследую адаптацию фреймворков измерения сообщества для микро-лабораторий. Рассматриваю отход от поверхностных показателей маркетинга в сторону опережающих метрик: коэффициента взаимности (reciprocity rate), глубины дискуссии и динамики повторных ответов мейнтейнеров. Выявляю, что наиболее сильным предсказателем приглашения на собеседование служит переход из публичного треда в синхронные закрытые каналы или прямые упоминания разработчиком в новых задачах.
Финализация сбора данных и завершение анализа
Завершаю серию поисковых сессий по партнерским программам Cohere и Meta Llama, чтобы собрать полный объем сведений. Перехожу к обобщению результатов, формированию карты источников с датами, оценке уровня уверенности по всем шести вопросам, составлению недельного плейбука касаний и списка ключевых ошибок.
talents.vaia.com
Ambassador - Part-time in West Jordan at Cohere | Apply now! - Talents by Vaia
buttondown.com
[AINews] How To Scale Your Model, by DeepMind - Buttondown
lightreading.com
Vodafone favorite Cohere may finally be set to disrupt telecom - Light Reading
ziprecruiter.com
Cohere Ambassador Job in Puyallup, WA - ZipRecruiter
grantedai.com
Llama Impact Grant (2026) - Granted AI
developer.meta.com
Open Source AI: Driving AI Innovation and Economic Growth | Llama - Meta for Developers
grantedai.com
Llama Impact Grants (2026) - Granted AI
impactopen.org
ImpactOpen — Every open AI-for-good opportunity, in one place
www2.fundsforngos.org
Call for Proposals: Llama Impact Grants - fundsforNGOs
respan.ai
Accelerators & AI Grants Finder - Respan
Оценка программ поддержки и гранатовых инициатив
Завершаю анализ условий участия в специализированных программах вендоров. Изучаю специфику грантовых инициатив (включая Llama Impact Grants от Meta), где акцент смещен с индивидуального контрибьютинга в исходный код на создание прикладных решений и социально значимых продуктов поверх открытых моделей. Отмечаю, что участие в таких программах дает прямой доступ к менторам и инженерным командам вендора, выступая альтернативной лестницей к технологическому партнерству.
Синтез собранных данных по всем шести направлениям
Подвожу итоги комплексного сбора информации по экосистемам топ-10 LLM-вендоров, платформенным лимитам GitHub, метрикам вовлеченности (second-touch rate, модель Orbit) и анти-кейсам ИИ-спама. Систематизирую выявленные факты для формирования уровней уверенности (established, emerging, speculative) по каждому исследуемому вопросу.
Переход к подготовке финального отчета
Завершаю этап поиска и синтеза информации. Приступаю к составлению итогового аналитического отчета на русском языке, включающего карту источников с датами, недельный плейбук по каденсу касаний для каждого вендора и список из топ-5 критических ошибок.
Первоначальное исследование принесло много информации. Я сейчас её разбираю и систематизирую. Это гарантирует, что всё будет логично и затронет все ключевые моменты, о которых вы просили.
Вот оно! Я собираю всё воедино. Записываю свои последние мысли, всё дорабатываю и убеждаюсь, что отдаю вам свою лучшую работу.

=== ССЫЛКИ ПАНЕЛИ ===
https://www.researchgate.net/publication/281401625_Matching_GitHub_Developer_Profiles_to_Job_Advertisements
https://arxiv.org/html/2504.06387v1
https://findjob.live/the-networking-necessity-building-connections-in-a-fast-movi
https://www.reddit.com/r/LocalLLaMA/comments/1n8c3l2/ama_with_hugging_face_science_the_team_behind/
https://sequoiacap.com/podcast/training-data-dhanji-prasanna
https://developers.google.com/community/experts
https://www.alainchautard.com/how-to-become-a-google-developer-expert-gde/
https://medium.com/google-developer-experts/being-an-ml-gde-2ba774c820bc
https://partners.cloud.google.com/google-cloud-ambassadors
https://qaskills.sh/blog/inspect-ai-evals-tutorial-2026
https://github.com/UKGovernmentBEIS/inspect_evals
https://ukgovernmentbeis.github.io/as-evaluation-standard/
https://arxiv.org/pdf/2507.06893
https://www.builtinsf.com/job/residency-2026/7840622
https://docs.github.com/en/rest/using-the-rest-api/best-practices-for-using-the-rest-api?apiVersion=2026-03-10
https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api
https://www.emergentmind.com/topics/pull-request-acceptance
https://www.researchgate.net/publication/383108411_Predicting_the_First_Response_Latency_of_Maintainers_and_Contributors_in_Pull_Requests
https://arxiv.org/html/2601.13466v1
https://github.com/modelcontextprotocol/python-sdk/blob/main/CONTRIBUTING.md
https://opensource.googleblog.com/2022/09/announcing-the-second-group-of-open-source-peer-bonus-winners-in-2022.html
https://www.loicmathieu.fr/wordpress/informatique/je-suis-google-developer-expert-gde-sur-les-technologies-google-cloud-platform-gcp/
https://github.com/tensorflow/models/issues/8635
https://daniel.haxx.se/blog/2024/01/02/the-i-in-llm-stands-for-intelligence/
https://daniel.haxx.se/blog/2026/04/22/high-quality-chaos/
https://github.com/vllm-project/vllm-omni/blob/main/.claude/skills/review-pr/references/delivery/maintainer-style-study.md
https://blog.n8n.io/migrating-community-metrics-to-orbit-using-n8n/
https://developerrelations.com/talks/using-the-orbit-model-to-demonstrate-community-value/
https://grantedai.com/grants/llama-impact-grant-meta-in-partnership-with-data-science-af-0b1938ee
https://www.impactopen.org/
https://github.com/UKGovernmentBEIS/inspect_k8s_sandbox
https://github.com/modelcontextprotocol
https://github.com/modelcontextprotocol/modelcontextprotocol
https://modelcontextprotocol.io/community/contributing
https://github.com/cjackett/ai-safety
https://github.com/vllm-project/vllm
https://developer.meta.com/ai/llama-ai-innovation/
https://daniel.haxx.se/blog/2026/05/11/mythos-finds-a-curl-vulnerability/
https://github.com/deepseek-ai/FlashMLA/pulls
https://buttondown.com/ainews/archive/ainews-how-to-scale-your-model-by-deepmind/
https://github.com/EleutherAI/lm-evaluation-harness/issues/3776
https://opensource.googleblog.com/2024/
https://discuss.google.dev/t/regarding-a-new-member/192364
https://www.markepear.dev/blog/developer-marketing-metrics
https://www.insightpartners.com/ideas/measuring-the-impact-of-a-community-driven-motion/
https://daniel.haxx.se/blog/2026/06/10/a-human-in-control/
https://www.freecodecamp.org/news/what-to-do-if-you-dont-get-into-outreachy/
https://daniel.haxx.se/blog/2026/05/26/the-pressure/
https://github.com/modelcontextprotocol/python-sdk?ref=deep-kondah.com
https://arxiv.org/html/2602.14572v3
https://github.com/megagonlabs/nlp4hr-survey
https://www.computer.org/csdl/proceedings-article/msr/2015/5594a362/12OmNx5YvoP
https://github.com/karthikrshet/Career-Agents
https://www.researchgate.net/publication/390638169_Towards_Evidence-Based_Tech_Hiring_Pipelines
https://www.se.cs.uni-saarland.de/theses/EmilyRiesMA.pdf
https://github.com/cboyd0319/JobSentinel/blob/main/docs/references.md
https://www.anthropic.com/
https://developers.google.com/machine-learning/guides/rules-of-ml
https://acesso.kissmyscore.com/?_=%2Fcommunity%2Fexperts%23xybe3UUSZzb%2FwWzrEi6oMN0hhMpZRy91%2Fop54RM%3D
https://github.com/UKGovernmentBEIS/inspect_k8s_sandbox/blob/main/README.md
https://schmatz.github.io/deception-eval-with-inspect/
https://arxiv.org/html/2602.11447v1
https://www.ime.usp.br/~gerosa/papers/ICSE_2025_Feeling_Welcome.pdf
https://github.com/modelcontextprotocol/python-sdk/security
https://github.com/modelcontextprotocol/actions
https://www.salary.com/resources/how-to/establish-a-winning-employee-milestone-bonus-program
https://secchi.io/looking-for-impactful-employee-recognition-program-ideas-and-examples-in-this-blog-learn-10-innovative-employee-recognition-ideas-with-real-world-examples-to-inspire-your-own-initiatives-discover/
https://iamrajivranjansingh.medium.com/google-summer-of-code-2026-7b77d6e64ade
https://daniel.haxx.se/blog/2025/10/10/a-new-breed-of-analyzers/
https://daniel.haxx.se/blog/author/daniel/
https://daniel.haxx.se/blog/2026/02/03/open-source-security-in-spite-of-ai/
https://github.com/vllm-project/vllm/blob/main/docs/contributing/model/multimodal.md
https://github.com/vllm-project/vllm-ascend/blob/main/CONTRIBUTING.md
https://github.com/vllm-project/vllm-metal/blob/main/CONTRIBUTING.md
https://github.com/vllm-project/vllm/blob/main/docs/contributing/profiling.md
https://github.com/vllm-project/vllm-omni/blob/main/CONTRIBUTING.md
https://github.com/vllm-project/vllm/blob/main/docs/contributing/editing-agent-instructions.md
https://github.com/vllm-project/vllm/blob/main/CONTRIBUTING.md
https://github.com/vllm-project/vllm-omni/issues/3520
https://github.com/meilisearch/devrel/issues/293
https://news.ycombinator.com/item?id <tg-id>
https://talents.vaia.com/companies/cohere/ambassador-part-time-33321642/
https://www.lightreading.com/5g/vodafone-favorite-cohere-may-finally-be-set-to-disrupt-telecom
https://www.ziprecruiter.com/c/Cohere/Job/Ambassador-(Front-Desk-&-Customer-Service)/-in-Tacoma,WA?jid=1c0f30a1ff870f78
https://grantedai.com/grants/llama-impact-grants-meta-for-developers-970d3c7c
https://www2.fundsforngos.org/latest-funds-for-ngos/call-for-proposals-llama-impact-grants/
https://www.respan.ai/accelerator-grant-finder
https://arxiv.org/html/2602.14572v3
https://github.com/megagonlabs/nlp4hr-survey
https://www.computer.org/csdl/proceedings-article/msr/2015/5594a362/12OmNx5YvoP
https://www.researchgate.net/publication/281401625_Matching_GitHub_Developer_Profiles_to_Job_Advertisements
https://arxiv.org/html/2504.06387v1
https://github.com/karthikrshet/Career-Agents
https://www.researchgate.net/publication/390638169_Towards_Evidence-Based_Tech_Hiring_Pipelines
https://www.se.cs.uni-saarland.de/theses/EmilyRiesMA.pdf
https://github.com/cboyd0319/JobSentinel/blob/main/docs/references.md
https://www.anthropic.com/
https://findjob.live/the-networking-necessity-building-connections-in-a-fast-movi
https://www.reddit.com/r/LocalLLaMA/comments/1n8c3l2/ama_with_hugging_face_science_the_team_behind/
https://sequoiacap.com/podcast/training-data-dhanji-prasanna
https://developers.google.com/community/experts
https://developers.google.com/machine-learning/guides/rules-of-ml
https://www.alainchautard.com/how-to-become-a-google-developer-expert-gde/
https://partners.cloud.google.com/google-cloud-ambassadors
https://discuss.google.dev/t/regarding-a-new-member/192364
https://medium.com/google-developer-experts/being-an-ml-gde-2ba774c820bc
https://acesso.kissmyscore.com/?_=%2Fcommunity%2Fexperts%23xybe3UUSZzb%2FwWzrEi6oMN0hhMpZRy91%2Fop54RM%3D
https://qaskills.sh/blog/inspect-ai-evals-tutorial-2026
https://github.com/UKGovernmentBEIS/inspect_evals
https://github.com/UKGovernmentBEIS/inspect_k8s_sandbox
https://ukgovernmentbeis.github.io/as-evaluation-standard/
https://github.com/UKGovernmentBEIS/inspect_k8s_sandbox/blob/main/README.md
https://schmatz.github.io/deception-eval-with-inspect/
https://arxiv.org/pdf/2507.06893
https://github.com/EleutherAI/lm-evaluation-harness/issues/3776
https://github.com/cjackett/ai-safety
https://www.builtinsf.com/job/residency-2026/7840622
https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api
https://docs.github.com/en/rest/using-the-rest-api/best-practices-for-using-the-rest-api?apiVersion=2026-03-10
https://arxiv.org/html/2602.11447v1
https://www.researchgate.net/publication/383108411_Predicting_the_First_Response_Latency_of_Maintainers_and_Contributors_in_Pull_Requests
https://www.emergentmind.com/topics/pull-request-acceptance
https://arxiv.org/html/2601.13466v1
https://www.ime.usp.br/~gerosa/papers/ICSE_2025_Feeling_Welcome.pdf
https://github.com/deepseek-ai/FlashMLA/pulls
https://github.com/modelcontextprotocol/python-sdk/blob/main/CONTRIBUTING.md
https://github.com/modelcontextprotocol/python-sdk?ref=deep-kondah.com
https://github.com/modelcontextprotocol/python-sdk/security
https://github.com/modelcontextprotocol/actions
https://github.com/modelcontextprotocol
https://modelcontextprotocol.io/community/contributing
https://github.com/modelcontextprotocol/modelcontextprotocol
https://www.loicmathieu.fr/wordpress/informatique/je-suis-google-developer-expert-gde-sur-les-technologies-google-cloud-platform-gcp/
https://www.freecodecamp.org/news/what-to-do-if-you-dont-get-into-outreachy/
https://www.salary.com/resources/how-to/establish-a-winning-employee-milestone-bonus-program
https://secchi.io/looking-for-impactful-employee-recognition-program-ideas-and-examples-in-this-blog-learn-10-innovative-employee-recognition-ideas-with-real-world-examples-to-inspire-your-own-initiatives-discover/
https://iamrajivranjansingh.medium.com/google-summer-of-code-2026-7b77d6e64ade
https://opensource.googleblog.com/2022/09/announcing-the-second-group-of-open-source-peer-bonus-winners-in-2022.html
https://opensource.googleblog.com/2024/
https://github.com/tensorflow/models/issues/8635
https://daniel.haxx.se/blog/2024/01/02/the-i-in-llm-stands-for-intelligence/
https://daniel.haxx.se/blog/2026/05/26/the-pressure/
https://daniel.haxx.se/blog/2025/10/10/a-new-breed-of-analyzers/
https://daniel.haxx.se/blog/author/daniel/
https://daniel.haxx.se/blog/2026/05/11/mythos-finds-a-curl-vulnerability/
https://daniel.haxx.se/blog/2026/04/22/high-quality-chaos/
https://daniel.haxx.se/blog/2026/06/10/a-human-in-control/
https://daniel.haxx.se/blog/2026/02/03/open-source-security-in-spite-of-ai/
https://github.com/vllm-project/vllm/blob/main/docs/contributing/model/multimodal.md
https://github.com/vllm-project/vllm-ascend/blob/main/CONTRIBUTING.md
https://github.com/vllm-project/vllm-metal/blob/main/CONTRIBUTING.md
https://github.com/vllm-project/vllm/blob/main/docs/contributing/profiling.md
https://github.com/vllm-project/vllm-omni/blob/main/CONTRIBUTING.md
https://github.com/vllm-project/vllm/blob/main/docs/contributing/editing-agent-instructions.md
https://github.com/vllm-project/vllm/blob/main/CONTRIBUTING.md
https://github.com/vllm-project/vllm-omni/issues/3520
https://github.com/vllm-project/vllm
https://github.com/vllm-project/vllm-omni/blob/main/.claude/skills/review-pr/references/delivery/maintainer-style-study.md
https://blog.n8n.io/migrating-community-metrics-to-orbit-using-n8n/
https://www.insightpartners.com/ideas/measuring-the-impact-of-a-community-driven-motion/
https://developerrelations.com/talks/using-the-orbit-model-to-demonstrate-community-value/
https://www.markepear.dev/blog/developer-marketing-metrics
https://github.com/meilisearch/devrel/issues/293
https://news.ycombinator.com/item?id <tg-id>
https://talents.vaia.com/companies/cohere/ambassador-part-time-33321642/
https://buttondown.com/ainews/archive/ainews-how-to-scale-your-model-by-deepmind/
https://www.lightreading.com/5g/vodafone-favorite-cohere-may-finally-be-set-to-disrupt-telecom
https://www.ziprecruiter.com/c/Cohere/Job/Ambassador-(Front-Desk-&-Customer-Service)/-in-Tacoma,WA?jid=1c0f30a1ff870f78
https://grantedai.com/grants/llama-impact-grant-meta-in-partnership-with-data-science-af-0b1938ee
https://developer.meta.com/ai/llama-ai-innovation/
https://grantedai.com/grants/llama-impact-grants-meta-for-developers-970d3c7c
https://www.impactopen.org/
https://www2.fundsforngos.org/latest-funds-for-ngos/call-for-proposals-llama-impact-grants/
https://www.respan.ai/accelerator-grant-finder