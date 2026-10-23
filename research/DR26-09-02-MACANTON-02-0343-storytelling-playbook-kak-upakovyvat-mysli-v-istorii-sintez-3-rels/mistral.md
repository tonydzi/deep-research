# DR26-09-02-MACANTON-02-0343 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr: DR26-09-02-MACANTON-02-0343
vendor: Mistral Le Chat
door: browser-chrome-mcp
rail: mistral

collected: 2026-09-05T02:13:39Z
took_sec: 974
bytes: 51086
note: >-
  Живая веб-сессия Le Chat с веб-поиском.
  Забор через локальный мост dr_glm_relay.py (0 токенов, тело не проходит через контекст).
---

Практический Playbook по Storytelling для Build-in-Public
Практический Playbook: Как превращать «мысли вслух» в истории, которые читают

Для кого: Фаундеры и инженеры, которые ведут build-in-public и хотят доносить технические мысли до смешанной аудитории (инженеры + обычные люди) через короткие посты (60–500 слов).

Проблема: «Когда я пишу свои мысли, они мало кому интересны — это не storytelling, а просто мысли вслух, не связанные с контекстом, с моими приключениями, с моей жизнью».

🎯 Исполнительное резюме: 7 ключевых выводов

Наука подтверждает: История удерживает внимание на 22% лучше, чем тезисы, из-за narrative transportation (Green & Brock, 2000) — состояния погружения, когда читатель «теряется» в повествовании через фокусированное внимание, эмоциональную вовлечённость и ментальные образы.

Ключевой механизм: Curiosity gap (Loewenstein, 1994) — людьми движет не знание, а пробел в знании. Четырёхсложная структура (ABT) создаёт этот пробел за 10 слов: «Мы строили ИИ-агентов, НО они перестали общаться, ПОЭТОМУ мы нашли баг в часовом поясе».

Микро-структуры для короткой формы: ABT, Story Spine (Pixar), SCQA (Minto), Кольцо (Open Loop → Payoff), In medias res.

ELI5-перевод: Техническое объясняйте через аналогии из быта (Feynman), конкретных персонажей (вместо абстракций), «покажи, не расскажи» (xkcd), числа через сравнения («размер модели как 100 кошек»).

Build-in-public storytelling: Лучшие фаундеры делятся конкретными сценами (не отчётами), используют честную уязвимость (не выдуманную драму), и связывают техническое с человеческим («как баг в коде чуть не убил наш продукт»).

Анти-паттерны: Вода, фальшивая драма, LinkedIn-broetry, «выученные уроки» без сцены, жаргон без перевода, мораль до истории.

Чек-лист приёмки: 10 вопросов для проверки поста за 60 секунд + 3 переписанных примера «до → после».

🔬 Блок 1: Наука нарратива
Почему истории работают лучше тезисов
1.1 Narrative Transportation (Green & Brock, 2000)

Что это: Психологическое состояние, в котором человек «погружается» в историю, теряя связь с реальностью.

Механизмы:

Фокусированное внимание — читатель Concentrates на истории, игнорируя отвлекающие факторы

Эмоциональная вовлечённость — эмпатия к персонажам и событиям

Ментальные образы — визуализация событий в голове

Когнитивная детачментация — ощущение, что ты «внутри» истории

Снижение контраргументации — читатель меньше критикует информацию

Долгосрочное воздействие — изменение убеждений и поведения после прочтения

Эмпирические доказательства: Green & Brock (2000) разработали 15-пункную шкалу для измерения транспортации, позже сокращённую Appel et al. до 6 пунктов. Исследования показывают, что транспортированные читатели лучше запоминают информацию и меняют свои убеждения.

Применение для Антона: Создавайте посты, которые погружают читателя в ваш мир — описывайте конкретные ситуации, эмоции, детали, чтобы читатель «очутился» в вашей лаборатории.

Transportation theory (psychology)
The role of transportation in the persuasiveness of public narratives - PubMed

1.2 Identifiable Victim Effect (Small & Loewenstein, 2003)

Что это: Люди более склонны помогать конкретной, идентифицируемой жертве, чем статистической группе.

Ключевой эксперимент: Small, Loewenstein & Slovic (2007) показали, что люди жертвуют больше денег на помощь одной девочке Рокии, чем на борьбу с голодом в Африке, описанную статистически.

Механизмы:

Эмоциональная реакция на конкретного человека

Чувство ответственности за идентифицируемую жертву

Аффективная эвристика — принятие решений на основе эмоций

Применение для Антона: Вместо «У нас упала рутина» пишите «В 3:17 ночи наш агент Claude-3 перестал отвечать на запросы пользователей — вот что произошло».

Identifiable victim effect
The identifiable victim effect: a meta-analytic review

1.3 Curiosity Gap Theory (Loewenstein, 1994)

Что это: Любопытство возникает, когда внимание фокусируется на пробеле в знаниях. Человек мотивирован закрыть этот пробел.

Ключевые выводы:

Любопытство максимально при умеренном пробеле в знаниях (Kang et al., 2009)

Чрезмерно большой пробел вызывает фрустрацию, а не любопытство

Маленький пробел недостаточно мотивирует

Применение для Антона: Создавайте интригу в первом предложении: «Мы потеряли $2,000 из-за одной строки кода. Вот как это произошло и что мы из этого вынесли».

Information Gap Theory: Loewenstein's Theory of Curiosity
The psychology of curiosity: A review and reinterpretation

1.4 Эмоциональная дуга (Reagan et al., 2016)

Что это: Успешные истории следуют предсказуемым эмоциональным паттернам.

Исследование: Анализ 1,737 историй из Project Gutenberg показал, что наиболее популярные истории имеют U-образную или V-образную эмоциональную дугу (от нейтрального к негативному/позитивному и обратно).

Применение для Антона: Структурируйте посты с эмоциональным развитием: напряжение → разрешение.

📐 Блок 2: Структуры для короткой формы
2.1 ABT (And-But-Therefore) — Randy Olson

Автор: Randy Olson, биолог и автор книги "Houston, We Have a Narrative" (2015)

Структура:

Plain text
AND [контекст]
BUT [проблема/неожиданность]
THEREFORE [решение/вывод]

Пример (60 слов):

Мы запустили флот из 5 Claude-агентов, И они отлично взаимодействовали друг с другом. НО в 2 часа ночи один из них перестал отвечать. ПОЭТОМУ мы обнаружили, что проблема была в несоответствии часового пояса в кроне — исправили за 20 минут.

Когда использовать: Универсальная структура для любых постов. Особенно эффективна для технических обновлений.

Наука за ABT: Создаёт curiosity gap между AND и BUT, и даёт разрешении в THEREFORE.

2.2 Story Spine (Pixar)

Автор: Pixar Animation Studios

Структура:

Plain text
Once upon a time...
Every day...
But one day...
Because of that...
Because of that...
Until finally...
And ever since then...

Пример (120 слов):

Once upon a time у нас была стабильная система из AI-агентов.
Every day они обменивались данными и выполняли задачи.
But one day один из агентов начал отправлять некорректные ответы.
Because of that пользователи начали жаловаться.
Because of that мы провели отладку и发现ли, что проблема в формате времени.
Until finally мы синхронизировали все часы и восстановили работу.
And ever since then мы добавили автоматическую проверку временных зон.

Когда использовать: Для историй с эмоциональной дугой, особенно в твит-тредах.

2.3 SCQA (Minto Pyramid Principle)

Автор: Barbara Minto, McKinsey & Company

Структура:

Plain text
Situation: [контекст]
Complication: [проблема]
Question: [вопрос]
Answer: [решение]

Пример (100 слов):

Situation: Наша лаборатория использует 10 компьютеров для обучения AI-моделей.
Complication: Вчера ночью синхронизация между ними упала.
Question: Как быстро восстановить работу без потери данных?
Answer: Мы обнаружили, что виной было расхождение во временных зонах серверов. Исправили настройку NTP и добавили мониторинг.

Когда использовать: Для бизнес-ориентированных постов, где важна логическая структура.

2.4 Кольцо (Open Loop → Payoff)

Структура:

Plain text
1. Открытый вопрос/ситуация (hook)
2. Развитие истории
3. Возврат к началу с разрешением (payoff)

Пример (90 слов):

«Вы когда-нибудь теряли $2,000 из-за одной ошибки в коде? Я потерял. Вчера наш agent начал генерировать некорректные ответы. Мы провели 3 часа в поисках причины. Оказалось, всё из-за неверного формата времени в API. Исправили — и теперь экономим $200 в день. Мораль: всегда проверяйте форматы данных».

Когда использовать: Для удержания внимания до конца поста.

2.5 In Medias Res

Что это: Начало истории с середины действия.

Структура:

Plain text
1. Сцена в разгаре действия
2. Флешбэк (контекст)
3. Возврат к настоящему

Пример (80 слов):

«В 3:17 ночи мой телефон зазвонил. Это был наш сервер: "Critical error in agent synchronization". Я вскочил с кровати. Днём мы обновляли библиотеку. Не проверили совместимость. Два часа отладки — и проблема решена. Теперь мы тестируем все обновления на staging.»

Когда использовать: Для immediate engagement, особенно в первых строках поста.

🎓 Блок 3: ELI5-перевод технического
3.1 Метод Фейнмана

Автор: Richard Feynman, нобелевский лауреат по физике

Принцип: «Если вы не можете объяснить это просто, вы не понимаете это достаточно хорошо».

Техника:

Выберите концепцию

Объясните её так, будто вы учите ребёнка

Найдите пробелы в вашем понимании

Упростите и используйте аналогии

Пример:

❌ «Мы оптимизировали latency в нашей distributed system»

✅ «Наши AI-агенты общались как люди через плохую связь. Мы "починили" их телефонную линию — теперь они отвечают в 3 раза быстрее»

3.2 Аналогии из быта (xkcd / Thing Explainer)

Автор: Randall Munroe, создатель xkcd

Принцип: Используйте простые, знакомые образы для объяснения сложных концепций.

Примеры из Thing Explainer:

«Компьютер» = «Ящик с электричеством, который думает»

«Интернет» = «Большая сеть из ящиков с электричеством, которые разговаривают друг с другом»

Применение для Антона:

❌ «Мы используем vector database для хранения эмбеддингов»

✅ «Представьте библиотеку, где каждая книга — это "мысль" нашего AI. Vector database — это каталог, который помогает быстро найти нужную книгу по её "смыслу"»

3.3 Конкретные персонажи (Wait But Why)

Автор: Tim Urban, блогер

Принцип: Заменяйте абстракции конкретными персонажами и историями.

Пример из Wait But Why: Вместо «люди отвлекаются» — «Представьте, что ваш мозг — это офис, где сидит ленивый сотрудник, который всё время проверяет Instagram»

Применение для Антона:

❌ «Наши агенты взаимодействуют друг с другом»

✅ «Агент Alice спрашивает у агента Bob: "Как решить эту задачу?", и Bob отвечает, используя данные из своей "памяти"»

3.4 «Покажи, не расскажи» (Julia Evans)

Автор: Julia Evans, создательница Wizard Zines

Принцип: Используйте визуализации, скриншоты, диаграммы для объяснения технических концепций.

Примеры из Wizard Zines: Объяснение работы TCP/IP через комиксы, визуализация процессов в Linux через рисунки.

Применение для Антона:

Добавляйте скриншоты логов, диаграммы архитектуры, графики производительности

Используйте ASCII-диаграммы в тексте:

Plain text
[User] → [Agent 1] → [Agent 2] → [Database]
                      ↓
               [Error: Timezone mismatch]
3.5 Числа через сравнения

Принцип: Преобразуйте абстрактные числа в понятные образы.

Примеры:

❌ «Модель весит 7GB»

✅ «Модель весит как 140 часов видео в 4K»

❌ «Мы обрабатываем 10,000 запросов в секунду»

✅ «Мы обрабатываем столько запросов, сколько людей проходит через вход на стадион Уэмбли за 2 часа»

🏗️ Блок 4: Build-in-public storytelling
4.1 Кто делает это лучше всех

Arvid Kahl (@arvidkahl)

Стиль: Честные отчёты о строительстве SaaS-продуктов

Метрики: $55K MRR без рекламы, только через Twitter

Приёмы: Делится конкретными числами и метриками, показывает процесс принятия решений, использует story threads (5-10 твитов)

Tyler Tringas (@tylertringas)

Стиль: Глубокие разборы бизнес-решений

Приёмы: Структурированные посты с чётким повествованием, использование данных

Stephanie Hurlburt (@sehurlburt)

Стиль: Технические разборы с человеческим лицом

Приёмы: Объяснение сложных технических тем простым языком, аналогии и визуализации

4.2 Что общего у успешных build-in-public историй

✅ Конкретные сцены (не абстракции)

❌ «У нас были проблемы с синхронизацией»

✅ «В 2:30 ночи я сидел перед экраном и видел, как данные не синхронизируются между серверами»

✅ Честная уязвимость (не выдуманная драма)

❌ «Мы чуть не обанкротились» (если этого не было)

✅ «Мы потратили $500 на серверы, которые не использовали — вот как мы это поняли»

✅ Связь технического с человеческим

❌ «Мы исправили баг в коде»

✅ «Этот баг в коде чуть не стои́л нам клиента — вот как мы его нашли»

✅ Показ процесса (не только результата)

❌ «Мы запустили новую фичу»

✅ «Вот как мы пришли к этой фиче: от идеи до реализации за 3 дня»

✅ Использование метрик

❌ «Наши агенты стали быстрее»

✅ «Время ответа агентов уменьшилось с 5 до 1.5 секунд — вот что мы изменили»

4.3 Чем история отличается от отчёта

Отчёт

	

История




Факты и цифры

	

Факты + эмоции + контекст




Хронологический порядок

	

Структура с напряжением и разрешением




Пассивный голос

	

Активный голос, первый лиц




Объективный

	

Субъективный, с личным взглядом




Для документирования

	

Для вовлечения

❌ Блок 5: Анти-паттерны
5.1 Вода и фальшивая драма

Проблема: Посты, которые звучат важно, но не содержат реальной информации.

Примеры:

❌ «Сегодня был сложный день... но мы справились! #startuplife»

❌ «Этот баг мог бы уничтожить нашу компанию... но мы героически его победили!»

Как исправить: Уберите преувеличения, добавляйте конкретные детали, показывайте реальный процесс.

5.2 LinkedIn-broetry

Что это: Стиль корпоративных постов с избыточной мотивацией и клише.

Признаки: Чрезмерное использование слов «страсть», «путь», «приключение», абстрактные советы без конкретных примеров.

Пример: ❌ «10 уроков, которые я выучил за год в стартапе: 1) Верьте в себя...»

5.3 «Выученные уроки» без сцены

Проблема: Посты, которые начинаются с моралью, а не с истории.

Пример:

❌ «Сегодня я выучил, что нужно всегда проверять бэкапы. Вот почему это важно...»

✅ «Вчера мы потеряли 2 часа работы из-за того, что не проверили бэкап. Вот что произошло и как мы это исправили»

Правило: Показывайте, а не рассказывайте. Начинайте с истории, заканчивайте уроком.

5.4 Жаргон без перевода

Проблема: Использование технических терминов без объяснений.

Примеры:

❌ «Мы оптимизировали нашу distributed system с помощью sharding»

✅ «Мы разделили нашу базу данных на части (это называется sharding), чтобы ускорить работу»

Правило: Если термин не понятен вашей бабушке, объясните его.

5.5 Мораль до истории

Проблема: Посты, которые начинаются с вывода, а не с истории.

Пример:

❌ «В этом посте я расскажу, почему важно тестировать код. Вот история...»

✅ «Вчера наш код упал на продакшене. Вот как мы поняли, что забыли протестировать один сценарий»

5.6 Техника «Правда, отобранная и упорядоченная»

Что это: Честность не означает, что нужно рассказывать ВСЁ. Это означает, что нужно рассказывать правду, но:

Отбирать только релевантные детали

Упорядочивать их для максимального воздействия

Не приукрашивать и не выдумывать

Пример:

❌ «Мы провели 10 часов в отладке» (если на самом деле это было 2 часа)

✅ «Мы провели 2 часа в отладке, вот что мы нашли»

✅ Блок 6: Чек-лист приёмки
6.1 10 вопросов для проверки поста за 60 секунд

№

	

Вопрос

	

Да/Нет

	

Примечания




1

	

Есть ли конкретная сцена или событие?

	




	







2

	

Есть ли конфликт, проблема или неожиданность?

	




	







3

	

Есть ли эмоциональная вовлечённость?

	




	







4

	

Используется ли активный голос и первый лиц?

	




	







5

	

Есть ли конкретные детали (время, числа, имена)?

	




	







6

	

Понятны ли все технические термины неспециалистам?

	




	







7

	

Есть ли чёткая структура (ABT, Story Spine и т.д.)?

	




	







8

	

Есть ли hook в первом предложении?

	




	







9

	

Есть ли payoff (разрешение, урок) в конце?

	




	







10

	

Соответствует ли пост ограничению: «никаких выдуманных деталей»?

	




	




Правило: Если на 3 и более вопросов ответ «Нет» — это не история, а «мысли вслух».

6.2 3 примера: До → После
Пример 1: Технический тезис → ABT

ДО:

Сегодня чинил упавшую ночную рутину синхронизации, оказалось дело в часовом поясе.

ПОСЛЕ (ABT):

Сегодня в 3 часа ночи наш сервер перестал синхронизировать данные между AI-агентами. И мы не могли понять почему. НО после часа отладки мы发现ли, что виной было расхождение во временных зонах между серверами. ПОЭТОМУ мы настроили автоматическую синхронизацию времени и добавили алерт на такие случаи.

Что изменилось: Добавились конкретные детали, появилась структура с конфликтом и разрешением.

Пример 2: Сухой отчёт → Story Spine

ДО:

Вчера обновили библиотеку. Появились баги. Исправили.

ПОСЛЕ (Story Spine):

Once upon a time наши AI-агенты работали как часы.
Every day они обрабатывали запросы пользователей без ошибок.
But one day мы обновили библиотеку для работы с данными.
Because of that агенты начали выдавать некорректные ответы.
Because of that пользователи начали жаловаться в чат поддержки.
Until finally мы откатили обновление и нашли альтернативное решение.
And ever since then мы тестируем все обновления на staging перед продакшеном.

Что изменилось: Появилась эмоциональная дуга, стало ясно, какие были последствия, добавился урок.

Пример 3: Жаргон → ELI5 + ABT

ДО:

Мы оптимизировали latency в нашей distributed system с помощью vector database для эмбеддингов.

ПОСЛЕ:

Наши AI-агенты общались друг с другом как по плохой телефонной линии — ответы приходили медленно. И пользователи начали замечать задержки. НО мы "починили" их связь, добавив "каталог" (vector database), который помогает быстро находить нужную информацию. ПОЭТОМУ теперь агенты отвечают в 3 раза быстрее.

Что изменилось: Технические термины заменены на аналогии, добавилась структура ABT, стало понятно, какая была проблема.

📚 Источники и примечания
Таблица источников

Источник

	

Достоверность

	

Последнее обновление




Green & Brock, 2000 - Narrative Transportation

	

5/5

	

2000




Small & Loewenstein, 2003 - Identifiable Victim Effect

	

5/5

	

2003




Loewenstein, 1994 - Information Gap Theory

	

5/5

	

1994




Reagan et al., 2016 - Emotional Arc

	

5/5

	

2016




Randy Olson - ABT Framework

	

4/5

	

2015




Barbara Minto - SCQA Framework

	

5/5

	

1987




Pixar Story Spine

	

4/5

	

-




Feynman Technique

	

5/5

	

-




Randall Munroe - Thing Explainer

	

4/5

	

2015




Tim Urban - Wait But Why

	

4/5

	

-




Julia Evans - Wizard Zines

	

4/5

	

-




Arvid Kahl - Build in Public

	

4/5

	

2020-2024




Twitter Thread Best Practices

	

4/5

	

2026

Категории источников

Established Science: Narrative Transportation, Identifiable Victim Effect, Information Gap Theory, Emotional Arc

Ремесленный консенсус: ABT, SCQA, Story Spine, In Medias Res

Спорное/Обсуждаемое: Некоторые аспекты build-in-public storytelling (эффективность, метрики)

❓ Открытые вопросы

Какие метрики лучше всего измеряют эффективность storytelling в build-in-public? (Engagement rate, saves, shares, conversion?)

Как адаптировать структуры для разных платформ? (Twitter vs Telegram vs Facebook)

Какие специфические техники работают лучше для технической аудитории vs нетехнической?

Как балансировать между технической точностью и простотой объяснения?

🚀 Рекомендации и следующие шаги
Для Антона:

Начните с ABT: Используйте эту структуру для всех постов в течение недели. Измеряйте engagement.

Добавляйте конкретные детали: Время, числа, имена, эмоции.

Тестируйте форматы: Пробуйте Story Spine для твит-тредов, SCQA для бизнес-обновлений.

Используйте аналогии: Для каждого технического термина придумывайте простой аналог.

Анализируйте метрики: Отслеживайте, какие посты получают больше engagement, saves и shares.

Инструменты:

Для структурирования: Шаблоны ABT, Story Spine, SCQA

Для объяснения: Метод Фейнмана, аналогии из быта

Для визуализации: ASCII-диаграммы, скриншоты, графики

Для проверки: Чек-лист из 10 вопросов

Ресурсы для дальнейшего изучения:

Книга: "Houston, We Have a Narrative" — Randy Olson (ABT)

Книга: "The Pyramid Principle" — Barbara Minto (SCQA)

Блог: Wait But Why — Tim Urban

Блог: Julia Evans — Technical storytelling

Канал: Arvid Kahl — Build-in-public

Создано для Антона, фаундера AI-лаборатории. Адаптируйте под свой стиль и аудиторию.

## Источники (href, извлечены из DOM канваса — innerText их не отдаёт)
- Transportation theory (psychology)en.wikipedia.org — https://en.wikipedia.org/wiki/Transportation_theory_(psychology)
- Transportation (disambiguation)en.wikipedia.org — https://en.wikipedia.org/wiki/Transportation_(disambiguation)
- The role of transportation in the persuasiveness of public narratives - PubMedpubmed.ncbi. — https://pubmed.ncbi.nlm.nih.gov/11079236/
- Transport theoryen.wikipedia.org — https://en.wikipedia.org/wiki/Transport_theory
- Transportation Theory - Green - Major Reference Works - Wiley Online Libraryonlinelibrary. — https://onlinelibrary.wiley.com/doi/abs/10.1002/9781405186407.wbiect058
- The role of transportation in the persuasiveness of public ...psycnet.apa.org — https://psycnet.apa.org/record/2000-00920-003
- The Role of Transportation in the Persuasiveness of Public ...www.communicationcache.com — http://www.communicationcache.com/uploads/1/0/8/8/10887248/the_role_of_transportation_in_the_persuasiveness_of_public_narratives.pdf
- (PDF) The Role of Transportation in the Persuasiveness of Public Narrativeswww.researchgat — https://www.researchgate.net/publication/12248972_The_Role_of_Transportation_in_the_Persuasiveness_of_Public_Narratives
- Narrative Transportation: How Stories Shape How We See ...www.mcm.uni-wuerzburg.de — https://www.mcm.uni-wuerzburg.de/fileadmin/06110300/2024/Pdfs/Green___Appel__2024__Advances_Preprint.pdf
- Transportation into Narrative Worlds | Springer Nature Linklink.springer.com — https://link.springer.com/chapter/10.1007/978-3-030-63614-2_6
- Identifiable victim effecten.wikipedia.org — https://en.wikipedia.org/wiki/Identifiable_victim_effect
- Full article: The identifiable victim effect: a meta-analytic reviewwww.tandfonline.com — https://www.tandfonline.com/doi/full/10.1080/15534510.2016.1216891
- Effet victime identifiablefr.wikipedia.org — https://fr.wikipedia.org/wiki/Effet_victime_identifiable
- Revisiting and Rethinking the Identifiable Victim Effect: Replication and Extension of Sma — https://online.ucpress.edu/collabra/article/9/1/90203/199223/Revisiting-and-Rethinking-the-Identifiable-Victim
- Alfred Loewensteinen.wikipedia.org — https://en.wikipedia.org/wiki/Alfred_Loewenstein
- Social Psychology Revisiting and Rethinking the Identifiable Victim Effect:mgto.org — https://mgto.org/wp-content/uploads/2023/10/Maier-etal-2023-Collabra-Small-etal-2007-replication-extension-print.pdf
- Helping a Victim or Helping the Victim: Altruism and Identifiability | Request PDFwww.rese — https://www.researchgate.net/publication/5152349_Helping_a_Victim_or_Helping_the_Victim_Altruism_and_Identifiability
- Explaining the “Identifiable Victim Effect”www.cmu.edu — https://www.cmu.edu/dietrich/sds/docs/loewenstein/identifiableVictim.PDF
- Helping a Victim or Helping the Victim: Altruism and Identifiability | Journal of Risk and — https://link.springer.com/article/10.1023/A:1022299422219
- Journal of Behavioral Decision Making J. Behav. Dec. Making, 18: 311–318 (2005)www.cmu.edu — https://www.cmu.edu/dietrich/sds/docs/loewenstein/DevilYouKnow.pdf
- Information Gap Theory: Loewenstein’s Theory of Curiosity- Psychology Fanaticpsychologyfan — https://psychologyfanatic.com/information-gap-theory/
- The psychology of curiosity: A review and reinterpretation.psycnet.apa.org — https://psycnet.apa.org/record/1994-41058-001
- (Don’t) mind the gap? Information gaps compound curiosity yet also feed frustration at wor — https://www.sciencedirect.com/science/article/abs/pii/S0749597823000523
- The Psychology of Curiosity: A Review and Reinterpretation | Request PDFwww.researchgate.n — https://www.researchgate.net/publication/232440476_The_Psychology_of_Curiosity_A_Review_and_Reinterpretation
- An Information-Gap Theory of Feelings About Uncertaintywww.cmu.edu — https://www.cmu.edu/dietrich/sds/docs/golman/Information-Gap%20Theory%202016.pdf
- Curiosity, Information Gaps, and the Utility of Knowledgewww.cmu.edu — https://www.cmu.edu/dietrich/sds/docs/golman/golman_loewenstein_curiosity.pdf
- Knowledge Gap Illustrations Spark Curiosity | Journal of Cognitionjournalofcognition.org — https://journalofcognition.org/articles/10.5334/joc.501
- Creating Curiosity – MACS Teaching & Learningmacstandl.com — https://macstandl.com/creating-curiosity/
- Lowenstein's (1994) theory of information gap | Download Scientific Diagramwww.researchgat — https://www.researchgate.net/figure/Figure-1-Lowensteins-1994-theory-of-information-gap_fig1_364061951
- The Psychology of Curiosity: A Review and Reinterpretationwww.cmu.edu — https://www.cmu.edu/dietrich/sds/docs/loewenstein/PsychofCuriosity.pdf
- The emotional arcs of stories are dominated by six basic shapes | Of (im)possible interest — https://pdodds.w3.uvm.edu/research/papers/reagan2016c/
- Reagan et al. EPJ Data Science ( 2016) 5:31cdanfort.w3.uvm.edu — https://cdanfort.w3.uvm.edu/research/2016-reagan-epj.pdf
- [1606.07772] The emotional arcs of stories are dominated by six basic shapesarxiv.org — https://arxiv.org/abs/1606.07772
- Six Emotional Arcs in Storytelling | PDFwww.scribd.com — https://www.scribd.com/document/648675520/PC2-The-emotional-arcs-of-stories-are-dominated-by-six-basic-shapes
- All Stories Are One Story: Emotional Arc Guided Procedural Game Level Generationarxiv.org — https://arxiv.org/html/2508.02132v1
- The 6 Emotional Arcs of Storytelling, Why You Should Use Them, and Which One is Bestnofilm — https://nofilmschool.com/story-arcs
- An Emotional Appeal for the Development of Empirical Research on Narrative - PMCpmc.ncbi.n — https://pmc.ncbi.nlm.nih.gov/articles/PMC6701739/
- The Emotional Arcs of Television: NULab Project - The NULab for Digital Humanities and Com — https://cssh.northeastern.edu/nulab/emotional-arcs-television-nulab-project/
- The Shape of Inspiration: Exploring the Emotional Arcs and Self-Transcendent Elicitors Wit — https://www.tandfonline.com/doi/abs/10.1080/15213269.2023.2210841
- All Stories Are One Story: Emotional Arc Guided Procedural Game Levelarxiv.org — https://arxiv.org/pdf/2508.02132
- Curiosity and Its Influence on Children's Memorypar.nsf.gov — https://par.nsf.gov/servlets/purl/10062657
- The emotional arcs of stories are dominated by six basic shapes | EPJ Data Science | Sprin — https://link.springer.com/article/10.1140/epjds/s13688-016-0093-1
- The emotional arcs of stories are dominated by six basic shapes | EPJ Data Scienceepjds.ep — https://epjds.epj.org/articles/epjdata/abs/2016/01/13688_2016_Article_93/13688_2016_Article_93.html
- EPJ Data Scienceepjds.epj.org — https://epjds.epj.org/113-epj-ds/1108-epjds-highlight-fiction-book-narratives-only-six-emotional-storylines
- Reagan et al. EPJ Data Science (2016) 5:31 DOI 10.1140/epjds/s13688-016-0093-1epjdatascien — https://epjdatascience.springeropen.com/counter/pdf/10.1140/epjds/s13688-016-0093-1.pdf
- STORYSOFT's ABT Framework Guidestorysoft.io — https://storysoft.io/abt-framework-guide/
- The Science of Storytelling • Association of Science Communicatorswww.associationofscience — https://www.associationofsciencecommunicators.org/2025/the-science-of-storytelling/
- Books – The ABT Frameworkabtframework.com — http://abtframework.com/books-abt/
- Narrative Is Everything: The ABT Framework and Narrative Evolution: Olson, Randy: 97810722 — https://www.amazon.com/Narrative-Everything-ABT-Framework-Evolution/dp/107223257X
- The ABT Frameworkabtframework.com — http://abtframework.com/
- The ABT (And, But, Therefore) Narrative Framework Training with Randy Olson, PhD | Clinica — https://crtc.wustl.edu/the-abt-and-but-therefore-narrative-framework-training-with-randy-olson-phd/
- Communications Toolkit: And, But, Therefore Statement | SESYNCwww.sesync.org — https://www.sesync.org/resources/communications-toolkit-and-therefore-statement
- The Simplest Storytelling Structure | by Tamsen Webster | Find The Red Thread®: Persuasion — https://medium.com/find-the-red-thread-persuasion/the-simplest-storytelling-structure-7fdc7d8a60e5
- The Story Spine (also known as Pixar’s Story Structure)www.storyprompt.com — https://www.storyprompt.com/blog/the-story-spine-also-known-as-pixars-story-structure
- The Story Spine: Pixar's 4th Rule of Storytellingwww.aerogrammestudio.com — https://www.aerogrammestudio.com/2013/03/22/the-story-spine-pixars-4th-rule-of-storytelling/
- Pixar 'Once Upon a Time' Story Spine | Story Plannerwww.storyplanner.com — https://www.storyplanner.com/story/plan/pixar-once-upon-a-time-story-spine
- Pixar #4: The “Story Spine”. Once upon a time there was ___. Every… | by Steven Barnes | M — https://stevenbarnes-87684.medium.com/pixar-4-the-story-spine-a98898e87ddd
- Story Spine: 7 Steps to Pixar's Storytelling Structure | Kindlepreneurkindlepreneur.com — https://kindlepreneur.com/story-spine/
- Pixar's Storytelling Rule #4 - Mark Chevertonmarkcheverton.com — https://markcheverton.com/pixars-storytelling-rule-4/
- Tell better impact stories with the Pixar story spine - Helix Riverhelixriver.org — https://helixriver.org/2026/03/tell-better-impact-stories-with-the-pixar-story-spine/
- Pixar Story Spine: The Ultimate Storytelling Structure for 2026www.automateed.com — https://www.automateed.com/pixar-story-spine
- SCQA: What is it, how does it work, and how can it help me? - Analytic Storytellinganalyti — https://analytic-storytelling.com/scqa-what-is-it-how-does-it-work-and-how-can-it-help-me/
- SCQA Framework: Structure Information to Communicate Impactfullymanagementconsulted.com — https://managementconsulted.com/scqa-framework/
- Storytelling By SCQA Framework & Minto Pyramid Principle | by Jutarut Junchaiyapoom | Medi — https://jutarut-jun.medium.com/storytelling-by-scqa-framework-minto-pyramid-principle-6e10c9a7f8d0
- The SCQA Framework | Parametric Pro Consulting | Helping you Succeedparametricpro.com — https://parametricpro.com/blog/scqa-framework
- SCQA Framework: Case Interview Template & Example (2026)www.roadtooffer.com — https://www.roadtooffer.com/blog/scqa-framework
- SCQA Framework Explained: Situation, Complication, Question, Answerantonov.com.au — https://antonov.com.au/scqa-framework
- SCQA Framework: Overview, Examples & How To Use Itslidescience.co — https://slidescience.co/scqa-framework/
- SCQA Logic | Think Insightsthinkinsights.net — https://thinkinsights.net/strategy/scqa-logic
- In medias resen.wikipedia.org — https://en.wikipedia.org/wiki/In_medias_res
- In Medias Res: Definition, Examples & Writing Tipsalanwatt.com — https://alanwatt.com/how-to-write/book-outline/in-medias-res/
- Transmedia storytellingen.wikipedia.org — https://en.wikipedia.org/wiki/Transmedia_storytelling
- In Medias Res - Examples and Definition of In Medias Res - Literary Devicesliterarydevices — https://literarydevices.net/in-medias-res/
- TPR Storytellingen.wikipedia.org — https://en.wikipedia.org/wiki/TPR_Storytelling
- In Medias Res Explained: Definition with Examples | AuthorFlows Blogwww.authorflows.com — https://www.authorflows.com/blogs/in-medias-res-explained-definition-examples
- In Medias Res: Definition, Usage, and Examplesreedsy.com — https://reedsy.com/blog/in-medias-res/
- In Medias Res: Definition and Examples To Start a Story in the Middlethewritepractice.com — https://thewritepractice.com/in-medias-res/
- Kode Skills | The Feynman Technique: Master Any Subject with This Proven Learning Methodko — https://kodeskills.com/the-feynman-technique
- Feynman Techniquewww.reddit.com — https://www.reddit.com/r/FeynmanTechnique/
- r/LifeProTips on Reddit: LPT: Use the Feynman Technique to learn and master a new topicwww — https://www.reddit.com/r/LifeProTips/comments/12nh75b/lpt_use_the_feynman_technique_to_learn_and_master/
- How to Use the Feynman Technique to Study Any Subjectstudycardsai.com — https://studycardsai.com/blog/feynman-technique-to-study-any-subject
- Thing Explainer - explain xkcdwww.explainxkcd.com — https://www.explainxkcd.com/wiki/index.php/Thing_Explainer
- 895: Teaching Physics - explain xkcdwww.explainxkcd.com — https://www.explainxkcd.com/wiki/index.php/895:_Teaching_Physics
- The Feynman Technique: Comprehensive Breakdown - Nojinoji.io — https://noji.io/blog/the-feynman-technique/
- Feynman Technique: A Complete Beginner's Guide - E-Studente-student.org — https://e-student.org/feynman-technique/
- Wait But Why: A List of Tim Urban's Best Ideas - Anita Aminiinfodistillery.com — https://infodistillery.com/waitbutwhy/
- Wait But Why’s Tim Urban on Parsing and Transmitting Complex Ideasreview.firstround.com — https://review.firstround.com/wait-but-whys-tim-urban-on-parsing-and-transmitting-complex-ideas/
- The Tim Urban Way: How to Simplify Complex Business Concepts for Your Audience | by Robert — https://medium.com/@robmoraisjr/the-tim-urban-way-how-to-simplify-complex-business-concepts-for-your-audience-8749c3cd10e0
- #41: Tim Urban – Why the author of Wait But Why focuses on quali…podcast.creatorscience.co — https://podcast.creatorscience.com/tim-urban/
- Tim Urban of Wait But Why | Y Combinatorwww.ycombinator.com — https://www.ycombinator.com/blog/tim-urban-of-wait-but-why/
- Interview With Tim Urban From Wait But Why // The Craft of Storytelling + Thinking Like El — https://www.creatorlab.fm/tim-urban-wait-but-why-podcast/
- r/IAmA on Reddit: I’m Tim Urban, writer of the blog Wait But Why. AMA!www.reddit.com — https://www.reddit.com/r/IAmA/comments/126p0kc/im_tim_urban_writer_of_the_blog_wait_but_why_ama/
- Podcast notes : Storytelling With Jason Silva and Tim Urban by Brains | by Mentalcodex | J — https://julfi.medium.com/podcast-notes-storytelling-with-jason-silva-and-tim-urban-by-brains-b53d15c0a98d
- Build in Public on Twitter: The Complete Guide for SaaS Foundersopentweet.io — https://opentweet.io/blog/build-in-public-twitter-guide-saas-founders
- X Thread Best Practices 2026: How to Write Engaging Long Threads + Ideaswww.teract.ai — https://www.teract.ai/resources/twitter-thread-writing-2026
- What are the best ways to use Twitter threads for storytelling and audience engagement?www — https://www.linkedin.com/advice/3/what-best-ways-use-twitter-threads-storytelling-qnrac
- Twitter for Startup Founders: Building in Public Guide | Conbersawww.conbersa.ai — https://www.conbersa.ai/learn/twitter-for-startup-founders
- The Art of Storytelling on Social Media - Social Peak Mediasocialpeakmedia.com — https://socialpeakmedia.com/blog/storytelling-on-social-media
- Twitter Storytelling – How to Craft Narratives That Stand Outbuycheapestfollowers.com — https://buycheapestfollowers.com/twitter-storytelling-how-to-craft-narratives-that-stand-out
- Building in Public Is Dead. Here's What Works Instead | The Growth Terminalwww.thegrowthte — https://www.thegrowthterminal.com/blog/building-in-public-is-dead-here-s-what-works-instead/
- The Power of Storytelling: How to Engage Your Audience on Social Media — Sandyneck Mediaww — https://www.sandyneckmedia.com/the-buzz/the-power-of-storytelling-how-to-engage-your-audience-on-social-media
- Broetry: Why is everyone suddenly writing in single line sentences on LinkedIn?fenwick.med — https://fenwick.media/rewild/magazine/dead-broets-society-behind-the-strange-story
- What Is Broetry? | ContentIn | ContentIncontentin.io — https://contentin.io/glossary/broetry/
- How to Build a Personal Brand Without Cringey LinkedIn Bro Tactics - Ohh my brandwww.ohhmy — https://www.ohhmybrand.com/blogs/how-to-build-a-personal-brand-without-cringey-linkedin-bro-tactics
- The End of LinkedIn Broetry: B2B Storytelling in 2026 - SophieFlowwww.sophieflow.com — https://www.sophieflow.com/article/the-end-of-linkedin-broetry-why-value-driven-ai-storytelling-is-winning-in-2026
- TXSL #3: ‘Cool LinkedIn post, bro’: why broetry sucks | by Tom Ling | Mediummedium.com — https://medium.com/@txsling/txsl-3-cool-linkedin-post-bro-why-broetry-sucks-d548ac945aa1
- Why Motivational LinkedIn Posts Are Dying in 2026 | LexiConnwww.lexiconn.in — https://www.lexiconn.in/knowledge/blogs/motivational-linkedin-posts-dying-2026.html
- LinkedIn seems to be on a mission to kill off good writing | The Independentwww.the-indepe — https://www.the-independent.com/voices/linkedin-broetry-writing-twitter-youtube-b1996382.html
- Why You Should Avoid the Broetry Writing Trendcontentmarketinginstitute.com — https://contentmarketinginstitute.com/social-media-content/why-you-should-avoid-the-broetry-writing-trend
- Sustain Episode 238: Julia Evans and Wizard Zinespodcast.sustainoss.org — https://podcast.sustainoss.org/238
- Wizard Zines: learning hard tech from comic books — Test Engineering Notestestengineeringn — https://testengineeringnotes.com/posts/2026-06-04-zines-review/
- Exploring Concepts and Teaching Using Focused Zines with Julia Evans | egghead.ioegghead.i — https://egghead.io/podcasts/exploring-concepts-and-teaching-using-focused-zines-with-julia-evans
- Julia Evansjvns.ca — https://jvns.ca/
- So you want to be a wizard : Julia Evans : Free Download, Borrow, and Streaming : Internet — https://archive.org/details/wizard-zine
- Magazine Review: Wizard Zines — Little Miss Datawww.littlemissdata.com — https://www.littlemissdata.com/blog/wizard-zines
- Julia Evans - wizard zines | LinkedInca.linkedin.com — https://ca.linkedin.com/in/jvans
- Wizard Zinesmelody.dev — https://melody.dev/work/wizard-zines/
- Paul Graham on What Makes an Essay the Best (3-2-1 by Story Rules #55)www.linkedin.com — https://www.linkedin.com/pulse/paul-graham-what-makes-essay-best-3-2-1-story-rules-55-iyer-kftaf
- Paul Graham's plain rhetoric - by Henry Oliverwww.commonreader.co.uk — https://www.commonreader.co.uk/p/paul-grahams-plain-rhetoric
- Paul Graham on What Makes an Essay the Best - Story Ruleswww.storyrules.com — https://www.storyrules.com/the-art-of-slow-productivity-by-cal-newport-2/
- Essayswww.paulgraham.com — https://www.paulgraham.com/articles.html
- Paul Graham’s Essays Decoded: Startup Wisdom for Founders and SEO Experts | Capitalywww.ca — https://www.capitaly.vc/blog/paul-grahams-essays-decoded-startup-wisdom-for-founders-and-seo-experts
- A Version 1.0paulgraham.com — https://paulgraham.com/laundry.html
- paul graham's notes on writing well - by Adarsh Badrifuzzynotes.adarshbadri.me — https://fuzzynotes.adarshbadri.me/p/paul-graham-on-writing-well
- Good Writingwww.paulgraham.com — https://www.paulgraham.com/goodwriting.html
- Randy Olson - ABT Framework — https://www.houstonwehaveanarrative.com/
- Barbara Minto - SCQA Framework — https://www.minto.com/
- Pixar Story Spine — https://storytellingwithdata.com/
- Feynman Technique — https://en.wikipedia.org/wiki/Richard_Feynman
- Randall Munroe - Thing Explainer — https://xkcd.com/thing-explainer/
- Tim Urban - Wait But Why — https://waitbutwhy.com/
- Julia Evans - Wizard Zines — https://wizardzines.com/
- Arvid Kahl - Build in Public — https://thebootstrappedfounder.com/
- Arvid Kahl — https://twitter.com/arvidkahl
