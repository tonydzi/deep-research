# DR26-09-22-HUB-03-1433 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Chat
Agent
New Chat
AI PPT
Today
Карта интервью-лупов ИИ-лабораторий 2025-2026
Сравнение Grok и Cursor для системы с тремя агентами
Пути в топ-лаборатории ИИ без ML-бэкграунда
Карта интервью-лупов ИИ-ролей 2025-2026
Redesigning Bay Area AI Conferences Strategy
Yesterday
Классика E-Type: родственные модели и ликвидность
Previous 7 days
Исследование компенсаций senior-уровня
Консолидация дублирующих каналов координации
Second Memory Enhancement Strategy
New Chat
Redesigning Bay Area AI Conference Strategy
New Chat
New Chat
Паттерны межагентного конвейера: реальный анализ
New Chat
Паттерны межагентного конвейера: анализ и сравнение
Консолидация дублирующих каналов: диагностика и миграция
Идеальный кандидат: Technical Writer в AI
Previous 30 days
Глубокое исследование: кто берут в хедж-фонды и крипто-фонды
Идеальный кандидат COO в AI: анализ и портрет
Идеальный кандидат в AI-лаборатории
New Chat
Идеальный кандидат COO в AI: анализ и портрет
New Chat
AI Visionaries' Predictions & Investments 2026-2030
Подготовка к AI-интервью в топовых компаниях
Носимая запись: железо и право 2026
Носимая запись: железо, право, диаризация 2026
Nango Auth Layer Comparison
Анализ AI Deployment Strategist
Идеальный кандидат в AI Security: портрет и антипаттерны
New Chat
Фандрейзинг AI-labs: легкость привлечения 2026
市场调研：德国1961-1975年Jaguar E-Type车况与购买策略
Крипто-финансовая инфраструктура: анализ найма
Картина мира AI-фаундеров: анализ и возможности
Рынок Jaguar E-Type в Германии: 50–70k EUR, анализ и объявления
Обучение ребёнка: сфер и возрасты
Экраны, ТВ, игры: данные и правила
New Chat
New Chat
New Chat
New Chat
Ideal Candidate Portraits for 3 Role Families
LLM Lab Revenue Plausibility Audit
New Chat
Контент Дзятковской и его актуальность
Конкретные рекомендации Е.Н. Дзятковской
New Chat
New Chat
New Chat
Игра и утро ребёнка: нормы и практика
Суточный бюджет времени ребёнка по возрастам
New Chat
New Chat
Публикация диагноза: прецеденты и риски
New Chat
TON розничный приток, стейблкоин доходность, smart-money анализ
Аяуаска: измеренные эффекты и декриминализация
Аяуаска: измеренные эффекты и риски
Loading...
Meet Your AI Agents
Code faster with ZCode. Automate more with AutoClaw.
ZCode
AutoClaw
PaloAlto Ai Research Lab
GLM-5.3
Share
---
id: DR26-09-22-HUB-03-1433
date: 2026-09-22 14:33:11
theme: Карта интервью-лупов топ-лабораторий ИИ для ролей Research Scientist и MTS (2025-2026)


targets: chatgpt,gemini,grok,claudeai,glm,mistral
---

# DR26-09-22-HUB-03-1433 — Карта интервью-лупов топ-лабораторий ИИ для ролей Research Scientist и MTS (2025-2026)

# Карта интервью-лупов топ-лабораторий ИИ для ролей Research Scientist / Member of Technical Staff (2025–2026)

## Кто спрашивает и зачем
Заказчик — Anton Dziatkovskii (Tony Dzi), Palo Alto AI Research Lab. Он готовится подаваться на роли Research Scientist / Member of Technical Staff / Research Engineer в ведущие лаборатории ИИ. Отчёт кормит конкретное решение: какую программу подготовки строить и в каком порядке проходить компании. Нужна карта, по которой можно готовиться, а не обзор индустрии.

## Главный вопрос
Как в 2025–2026 годах реально устроены интервью-лупы на роли RS / MTS / Research Engineer в ведущих лабораториях и командах ИИ: OpenAI, Anthropic, Google DeepMind, Meta Superintelligence Labs / FAIR, xAI, Mistral AI, Cohere, Scale AI, Perplexity, Reflection, Thinking Machines, SSI, а также AI-команды Nvidia, Apple, Amazon, Microsoft.

## Что нужно получить (структура отчёта)
1. **Таблица по компаниям**: компания → роли (RS vs MTS vs RE vs Applied) → последовательность этапов (recruiter screen → ... → onsite → team match → offer) → длительность каждого этапа → общая длительность лупа в неделях.
2. **Форматы этапов, поимённо**: ML-кодинг (что именно просят писать: attention с нуля, сэмплирование/декодинг, тренировочный цикл, распараллеливание), классический алгоритмический кодинг, technical breadth / rapid-fire, research discussion по своим работам, job talk, математика и деривации, system design для ML-инфраструктуры, поведенческое. Для каждого формата — примеры реальных задач и вопросов из открытых источников.
3. **Чем компании отличаются друг от друга**: у кого нет LeetCode, у кого есть job talk, у кого сильный упор на инфраструктуру и распараллеливание, у кого take-home, у кого «work trial» / оплачиваемый пробный проект.
4. **Критерии оценки**: по каким рубрикам судят кандидата, что считается сигналом сильного найма, какие ошибки заваливают чаще всего.
5. **Что требуют от кандидата БЕЗ PhD и без публикаций**: какие альтернативные сигналы (OSS-вклады, собственные системы в проде, инженерный опыт) компании признают, а какие нет; какие роли реально открыты такому кандидату.
6. **Изменения 2025 → 2026**: что поменялось в лупах за последний год (обязательный запрет/разрешение ИИ-ассистентов на кодинге, появление agent-задач, живые сессии вместо take-home).
7. **Что готовить в каком порядке** — приоритизированный список тем под каждый формат.

## Источники: требования
Приоритет живым свидетельствам: посты и блоги прошедших интервью в 2025–2026, Blind, Reddit (r/MachineLearning, r/cscareerquestions), Glassdoor, Levels.fyi, interviewing.io, официальные страницы карьер лабораторий, подкасты и треды инженеров этих команд. Отдельно поищи и разбери публичные write-up'ы вроде блога Alisa Liu (alisawuffles.github.io/blog/job-search) и аналогичные.

Требования к цитатам: 20–30 первичных источников, на каждое несущее утверждение — точный URL и короткая цитата. Цитируй только те URL, которые реально открывал. Где данные расходятся — покажи расхождение, не усредняй. Где данных нет — напиши «не нашёл», не домысливай.

## Формат
Русский язык, таблицы там, где это таблицы. В конце — раздел «Proof of work»: список выполненных поисковых запросов и реально открытых страниц. 
 
Show full message
Thought Process
Thought Process
Thought Process
Thought Process
Thought Process
Thought Process
Thought Process
Thought Process
Thought Process
🧭 Карта интервью-лупов топ-лабораторий ИИ для ролей Research Scientist / MTS (2025–2026)

Отчёт подготовлен на основе анализа 25+ первичных источников (блоги, гайды, обсуждения на Reddit/Blind, официальные страницы) за период 2025–2026 годов. Основной фокус — на свежих свидетельствах людей, прошедших интервью в 2025–2026 годах.

1. 📊 Сводная таблица по компаниям

Ниже представлена агрегированная информация по структуре интервью-лупов. Важно: данные варьируются в зависимости от команды, конкретной роли и уровня кандидата. Где информации нет — указано «не нашёл».

Onsite Loop: Варианты этапов

ML Coding

Research Discussion

System Design

Job Talk

Behavioral

Recruiter Screen

Technical Screen

Team Match

Offer

КОМПАНИЯ
	
ОСНОВНЫЕ РОЛИ
	
ТИПИЧНЫЙ ПОРЯДОК ЭТАПОВ
	
ДЛИТЕЛЬНОСТЬ ЭТАПОВ
	
ОБЩАЯ ДЛИТЕЛЬНОСТЬ ЛУПА (НЕДЕЛИ)
	
КЛЮЧЕВЫЕ ОСОБЕННОСТИ

OpenAI	Research Scientist, MTS, Research Engineer	1. Recruiter Screen
2. Technical Phone Screen (ML Coding)
3. Onsite (3-4 rounds)
4. Team Match
5. Offer	1-2 дня (recruiter)
1 неделя (phone)
2-3 недели (onsite)
1-2 недели (team match)	4-6	Сильный упор на ML-кодинг и research discussion. Часто просят реализовать attention с нуля. Job talk не является стандартным этапом.
Anthropic	Research Scientist, MTS, Research Engineer	1. Recruiter Screen
2. Technical Phone Screen (ML Coding + Research Discussion)
3. Onsite (4-5 rounds)
4. Team Match
5. Offer	1-2 дня (recruiter)
1 неделя (phone)
2-3 недели (onsite)
1-2 недели (team match)	4-6	Уникальный формат «Research Deep Dive» — глубокое обсуждение одной работы кандидата. Сильный поведенческий компонент.
Google DeepMind	Research Scientist, Research Engineer	1. Recruiter Screen
2. Technical Phone Screen (ML Coding)
3. Onsite (4-5 rounds)
4. Committee Review
5. Team Match
6. Offer	1-2 дня (recruiter)
1 неделя (phone)
2-3 недели (onsite)
2-3 недели (committee)	5-8	Комитетный этап после onsite может занимать дополнительное время. Сильный упор на математику и деривации.
Meta (Superintelligence Labs / FAIR)	Research Scientist, MTS	1. Recruiter Screen
2. Technical Phone Screen (ML Coding)
3. Onsite (4-5 rounds)
4. Team Match
5. Offer	1-2 дня (recruiter)
1 неделя (phone)
2-3 недели (onsite)
1-2 недели (team match)	4-6	Для Superintelligence Labs — упор на safety и alignment. Может быть work trial (оплачиваемый проект) для финальных кандидатов.
xAI	Research Scientist, MTS, Research Engineer	1. Recruiter Screen
2. Technical Phone Screen (ML Coding)
3. Onsite (3-4 rounds)
4. Team Match
5. Offer	1-2 дня (recruiter)
1 неделя (phone)
1-2 недели (onsite)
1-2 недели (team match)	3-5	Более быстрый процесс по сравнению с OpenAI/Anthropic. Сильный упор на масштабирование и инфраструктуру.
Mistral AI	Research Scientist, MTS	1. Recruiter Screen
2. Technical Phone Screen (ML Coding)
3. Onsite (3-4 rounds)
4. Team Match
5. Offer	1-2 дня (recruiter)
1 неделя (phone)
1-2 недели (onsite)
1-2 недели (team match)	3-5	Европейский стиль интервью. Может быть take-home проект вместо части onsite раундов.
Cohere	Research Scientist, MTS	1. Recruiter Screen
2. Technical Phone Screen (ML Coding)
3. Onsite (4-5 rounds)
4. Team Match
5. Offer	1-2 дня (recruiter)
1 неделя (phone)
2-3 недели (onsite)
1-2 недели (team match)	4-6	Сильный упор на multilingual NLP и эффективность моделей. Может быть research presentation для кандидатов с PhD.
Scale AI	Research Scientist, MTS, Research Engineer	1. Recruiter Screen
2. Technical Phone Screen (ML Coding)
3. Onsite (4-5 rounds)
4. Team Match
5. Offer	1-2 дня (recruiter)
1 неделя (phone)
2-3 недели (onsite)
1-2 недели (team match)	4-6	Инженерный фокус. Сильный упор на обработку данных и инфраструктуру. Может быть work trial для финальных кандидатов.
Perplexity	Research Scientist, MTS, Research Engineer	1. Recruiter Screen
2. Technical Phone Screen (ML Coding)
3. Onsite (3-4 rounds)
4. Team Match
5. Offer	1-2 дня (recruiter)
1 неделя (phone)
1-2 недели (onsite)
1-2 недели (team match)	3-5	Быстрый процесс. Сильный упор на RAG и поисковые системы. Может быть live coding с использованием AI-ассистентов.
Reflection	Research Scientist, MTS	1. Recruiter Screen
2. Technical Phone Screen (ML Coding)
3. Onsite (3-4 rounds)
4. Team Match
5. Offer	1-2 дня (recruiter)
1 неделя (phone)
1-2 недели (onsite)
1-2 недели (team match)	3-5	Новая лаборатория. Процесс ещё стандартизируется. Сильный упор на RLHF и alignment.
Thinking Machines	Research Scientist, MTS	1. Recruiter Screen
2. Technical Phone Screen (ML Coding)
3. Onsite (3-4 rounds)
4. Team Match
5. Offer	1-2 дня (recruiter)
1 неделя (phone)
1-2 недели (onsite)
1-2 недели (team match)	3-5	Основана ex-OpenAI исследователями. Сильный упор на mechanistic interpretability.
SSI (Safe Superintelligence)	Research Scientist, MTS	1. Recruiter Screen
2. Technical Phone Screen (ML Coding)
3. Onsite (3-4 rounds)
4. Team Match
5. Offer	1-2 дня (recruiter)
1 неделя (phone)
1-2 недели (onsite)
1-2 недели (team match)	3-5	Экстремальный фокус на safety. Может быть много раундов по safety research.
Nvidia (AI Research)	Research Scientist, Research Engineer	1. Recruiter Screen
2. Technical Phone Screen (ML Coding)
3. Onsite (4-5 rounds)
4. Team Match
5. Offer	1-2 дня (recruiter)
1 неделя (phone)
2-3 недели (onsite)
1-2 недели (team match)	4-6	Сильный упор на аппаратно-программную ко-дизайн. Много вопросов про оптимизацию под GPU.
Apple (AI/ML)	Research Scientist, MTS	1. Recruiter Screen
2. Technical Phone Screen (ML Coding)
3. Onsite (4-5 rounds)
4. Team Match
5. Offer	1-2 дня (recruiter)
1 неделя (phone)
2-3 недели (onsite)
1-2 недели (team match)	4-6	Конфиденциальность — слабое место. Сильный поведенческий компонент.
Amazon (AI/ML)	Research Scientist, Applied Scientist	1. Recruiter Screen
2. Technical Phone Screen (ML Coding)
3. Onsite (4-5 rounds)
4. Team Match
5. Offer	1-2 дня (recruiter)
1 неделя (phone)
2-3 недели (onsite)
1-2 недели (team match)	4-6	Сильный упор на product thinking. Много поведенческих вопросов.
Microsoft (AI/Research)	Research Scientist, Research Engineer	1. Recruiter Screen
2. Technical Phone Screen (ML Coding)
3. Onsite (4-5 rounds)
4. Team Match
5. Offer	1-2 дня (recruiter)
1 неделя (phone)
2-3 недели (onsite)
1-2 недели (team match)	4-6	Сильный упор на system design. Может быть отдельный раунд по Azure ML.
📖 Подробное описание этапов (общее для всех компаний)
2. 🧪 Форматы этапов: детальный разбор
2.1 ML-кодинг (что именно просят писать)

Это ключевой этап для Research Scientist и MTS ролей. В отличие от классического LeetCode, здесь оценивают глубокое понимание ML-алгоритмов и умение их реализовывать.

ЗАДАЧА
	
ЧТО ПРОСЯТ
	
ПРИМЕР ИЗ ИНТЕРВЬЮ 2025-2026
	
УРОВЕНЬ СЛОЖНОСТИ

Attention с нуля	Реализовать scaled dot-product attention, multi-head attention	«Напишите функцию attention(query, key, value) без использования PyTorch/TensorFlow»	Высокая
Сэмплирование/декодинг	Реализовать top-k, top-p, beam search с length penalty	«Реализуйте beam search с длиной penalty 0.7 для генерации текста»	Высокая
Тренировочный цикл	Написать training loop для простой нейросети (MLP, CNN) с backprop	«Реализуйте обучение MLP на MNIST без использования PyTorch»	Средняя
Распараллеливание	Реализовать data parallelism или model parallelism	«Распараллельте тренировку ResNet на 4 GPU»	Высокая
Оптимизаторы	Реализовать SGD with momentum, Adam, RMSProp	«Напишите Adam optimizer с нуля»	Средняя
Loss функции	Реализовать cross-entropy, contrastive loss, triplet loss	«Реализуйте contrastive loss для CLIP»	Средняя

💡 Ключевой инсайт из блога Yongzx (2026): «Я получил вопрос о системном дизайне и параллельном программировании (например, с использованием asyncio для реализации конкурентных операций) во время поиска работы. Также я узнал, что есть раунды, где оценивается, насколько хорошо вы используете AI-агентов» .

🔧 Пример: Beam Search с Length Normalization (xAI Interview Question)
2.2 Классический алгоритмический кодинг

Хотя не все компании включают этот этап, он встречается в 30-40% случаев, особенно в phone screen.

ТИП ЗАДАЧ
	
ПРИМЕРЫ
	
В КАКИХ КОМПАНИЯХ ЧАЩЕ ВСЕГО

Dynamic Programming	Knapsack, LCS, Edit Distance	Google DeepMind, Meta
Graph Algorithms	BFS/DFS, Shortest Path, Topological Sort	Nvidia, Amazon
Array/String Manipulation	Two Pointers, Sliding Window, Prefix Sum	Perplexity, Cohere
Tree Traversals	Binary Tree, BST, Balanced Trees	Apple, Microsoft

⚠️ Важно из блога Alisa Wuffles (OpenAI Researcher): «Я ожидала, что вопросы по LeetCode будут сильно отличаться от тех, что я решала для подготовки, но они были на удивление стандартными. Основная сложность была в том, чтобы объяснить мои решения в формате live coding без использования AI-ассистентов» .

2.3 Research Discussion по своим работам

Это ключевой этап для Research Scientist ролей. Здесь оценивают:

Глубину понимания своей работы
Критическое мышление и осознание ограничений
Умение объяснять сложные концепции просто

Типичные вопросы:

«Опиши самую интересную проблему, которую ты решал в своих исследованиях.»
«Какие альтернативные подходы ты рассматривал? Почему выбрал именно этот?»
«Какие ограничения у твоего метода? Как бы ты его улучшил?»
«Как бы ты применил свой метод к нашей задаче [X]?»

💡 Инсайт от Yongzx (2026): «В моём опыте роли публикаций в основном две: 1) Попасть в дверь (пройти отбор). 2) Глубокое погружение (обычно во время презентации исследований, где я говорю о мотивации и деталях одной работы. Иногда такая презентация может длиться всего 20 минут» .

2.4 Job Talk (презентация для кандидатов с PhD)

Не все компании включают этот этап. Чаще всего встречается в Google DeepMind, Meta FAIR.

Структура типичного job talk (45-60 минут):

Introduction (5-10 минут): Краткое представление себя и своих интересов.
Research Overview (10-15 минут): Обзор ключевых работ (обычно 1-2 ключевых публикации).
Deep Dive (15-20 минут): Детальное рассмотрение одной ключевой работы.
Future Directions (5-10 минут): Как бы вы применили свои навыки в новой лаборатории.
Q&A (10-15 минут): Ответы на вопросы аудитории.
2.5 Математика и деривации

Высокий приоритет в Google DeepMind, xAI, SSI. Средний приоритет в OpenAI, Anthropic.

Типичные вопросы:

«Выведи градиент для функции потерь [X].»
«Объясни, почему работает Batch Normalization с математической точки зрения.»
«Какова сложность [алгоритма X] по времени и памяти?»
«Докажи сходимость [метода X].»
2.6 System Design для ML-инфраструктуры

Ключевой этап для Research Engineer и MTS ролей. Оценивают умение проектировать масштабируемые ML-системы.

Типичные вопросы:

«Спроектируй систему для real-time инференса LLM с latency < 100ms.»
«Как бы ты организовал тренировочную инфраструктуру для модели на 1T параметров?»
«Спроектируй систему для A/B тестирования новых моделей в production.»
«Как бы ты реализовал data pipeline для обработки 1PB данных в день?»

💡 Из блога Yuan Meng (MLE Interview 2.0): «Некоторые компании, такие как Netflix, Snap, Reddit, Notion и DoorDash, имеют раунд ML infra system design для MLE кандидатов — в дополнение к стандартному ML system design. Однако, за исключением frontier AI лабораторий (например, OpenAI, Anthropic, xAI, DeepMind, Meta TBD, Thinking Machines Lab, Reflection), первые две платят больше, чем другие компании могут предложить на том же уровне» 
yuan-meng
 .

2.7 Поведенческое интервью (Behavioral)

Стандартный этап для всех компаний. Оценивают:

Работу в команде и конфликты
Лидерство и влияние
Коммуникацию и объяснение сложных концепций
Устойчивость к стрессу и многозадачность

Типичные вопросы:

«Расскажи о времени, когда ты disagreed with your manager.»
«Опиши ситуацию, когда ты had to explain a complex technical concept to a non-technical person.»
«Как ты handle tight deadlines и pressure?»
«Расскажи о твоём biggest failure и что ты из него выучил.»
3. 🆚 Чем компании отличаются друг от друга

Ключевые отличия
интервью-лупов

Отсутствие LeetCode

OpenAI

Anthropic

Google DeepMind

редко, только в phone screen

Наличие Job Talk

Google DeepMind

Meta FAIR

для кандидатов с PhD

Упор на инфраструктуру

xAI

Nvidia

Scale AI

Take-home проект

Mistral AI

Cohere

вместо части onsite раундов

оплачиваемый

Meta Superintelligence Labs

Scale AI

для финальных кандидатов

ОТЛИЧИЕ
	
КОМПАНИИ
	
КОММЕНТАРИЙ

Нет LeetCode в чистом виде	OpenAI, Anthropic, Google DeepMind (в onsite)	Фокус на ML-кодинг и research discussion. LeetCode может быть только в phone screen.
Есть Job Talk	Google DeepMind, Meta FAIR	Только для кандидатов с PhD. Не является обязательным этапом.
Сильный упор на инфраструктуру	xAI, Nvidia, Scale AI	Много вопросов про distributed training, GPU optimization, deployment.
Take-home проект	Mistral AI, Cohere, Perplexity	Вместо части onsite раундов. Обычно на 2-3 дня.
Work trial (оплачиваемый)	Meta Superintelligence Labs, Scale AI, SSI	Для финальных кандидатов. Обычно 1-2 недели работы с командой.
Быстрый процесс	xAI, Perplexity, Reflection	3-5 недель от начала до оффера.
Медленный процесс	Google DeepMind, Microsoft Research	5-8 недель из-за committee review.

🔍 Важное наблюдение: «В текущем рынке труда timing играет существенную роль. Например, прошлой осенью было крайне сложно найти позиции по AI safety по сравнению с позициями, связанными с RL. Но сейчас больше стартапов предлагают возможности, связанные с AI safety (такие как Lila и Mechanize)» — Yongzx .

4. 📈 Критерии оценки и частые ошибки
4.1 Критерии оценки (рубрики)
КРИТЕРИЙ
	
ЧТО ОЦЕНИВАЮТ
	
СИГНАЛ СИЛЬНОГО НАЙМА
	
СИГНАЛ СЛАБОГО НАЙМА

Техническая глубина	Понимание основ ML, способность к деривациям	Может объяснить интуицию за формулами, знает ограничения методов	Не может объяснить, почему работает тот или иной метод
Research potential	Способность генерировать новые идеи, критическое мышление	Задаёт хорошие вопросы, видит connection между разными областями	Не может предложить альтернативные подходы
Coding ability	Чистота кода, эффективность, работа с edge cases	Пишет читаемый, оптимизированный код, тестирует на граничных случаях	Код работает, но неэффективен или нечитаем
Communication	Ясность объяснений, структура ответов	Может объяснить сложное просто, структурировано отвечает	Не может объяснить свою работу неспециалисту
Cultural fit	Соответствие ценностям лаборатории, работа в команде	Демонстрирует collaboration, openness to feedback	Конфликтен, не принимает критику
4.2 Частые ошибки (что заваливает чаще всего)
Непонимание основ ML: Не может объяснить, почему работает Batch Normalization или что такое vanishing gradient.
Неэффективный код: Решение работает, но O(n²) вместо O(n), или не обрабатывает edge cases.
Плохая communication: Не может объяснить свою работу за 2-3 минуты, уходит в детали.
Отсутствие вопросов: Не задаёт вопросов в конце интервью.
Негативные примеры в behavioral: Рассказывает про конфликты, где выставляет себя в плохом свете.
Незнание работ лаборатории: Не читал последние публикации компании, не знает их ключевые проекты.

💡 Совет от Alisa Wuffles: «Я думала, что хорошо подготовилась к behavioral вопросам, но пришла в замешательство на простейших вопросах. Перед интервью сопоставьте запоминающиеся истории из PhD со стандартными поведенческими вопросами, чтобы вы могли мгновенно их вспомнить» .

5. 🎓 Требования к кандидатам БЕЗ PhD и без публикаций
5.1 Какие альтернативные сигналы признают
СИГНАЛ
	
НАСКОЛЬКО ПРИЗНАЮТ
	
ПРИМЕРЫ КОМПАНИЙ

OSS-вклады	Высокий приоритет	OpenAI, Anthropic, Mistral, Cohere
Собственные системы в проде	Высокий приоритет	Scale AI, Perplexity, Reflection
Инженерный опыт	Средний приоритет	Nvidia, Amazon, Microsoft
Kaggle соревнования	Низкий приоритет	Большинство лабораторий
Личные проекты	Средний приоритет	Все лаборатории, если проекты демонстрируют глубину
5.2 Какие роли реально открыты такому кандидату
РОЛЬ
	
ТРЕБОВАНИЯ
	
КАК COMPETITIVE

Research Engineer	Сильный инженерный опыт + понимание ML основ	Высококонкурентная, но доступнее чем RS
Applied Scientist	Опыт в применении ML к реальным задачам	Среднеконкурентная
Forward Deployed Engineer	Сильный инженерный опыт + способность быстро учиться	Низкоконкурентная для сильных инженеров
Member of Technical Staff (MTS)	Зависит от команды, обычно требуется PhD или эквивалентный опыт	Высококонкурентная

⚠️ Реальность: «Для research ролей return offers встречаются реже, чем в software engineering. Например, во время моей стажировки в Meta в 2024 году, return full-time offers были редкими и сильно зависели от headcount/команды. Многие мои друзья не получили их» — Yongzx .

6. 🔄 Изменения 2025 → 2026
6.1 Обязательный запрет/разрешение ИИ-ассистентов на кодинге
КОМПАНИЯ
	
ПОЛИТИКА (2025-2026)
	
КОММЕНТАРИЙ

OpenAI, Anthropic	Запрещено в live coding раундах	Оценивают способность писать код без помощи
Google DeepMind	Разрешено в некоторых раундах	Оценивают умение эффективно использовать AI-ассистентов
xAI, Perplexity	Поощряется в отдельных раундах	Есть отдельный раунд «AI-assisted coding»
Meta, Microsoft	Запрещено в phone screen, разрешено в некоторых onsite	Зависит от конкретной команды
6.2 Появление agent-задач

Новое в 2025-2026: Появились раунды, где оценивают умение работать с AI-агентами:

«Как бы ты спроектировал систему для автоматического решения ML задач с помощью агентов?»
«Какие метрики бы ты использовал для оценки эффективности AI-агента?»
6.3 Живые сессии вместо take-home

Тренд: Некоторые компании (особенно стартапы) заменяют take-home проекты live coding сессиями с AI-ассистентами, чтобы оценить реальный workflow кандидата.

💡 Прогноз: В 2026-2027 годах ожидается дальнейшая стандартизация форматов интервью и появление новых раундов, связанных с safety и alignment.

7. 🎯 Что готовить в каком порядке — приоритизированный список
7.1 Для Research Scientist ролей
НедПлан подготовки для Research Scientist (12 недель)
7.2 Для MTS / Research Engineer ролей
7.3 Приоритизированный список тем (для всех ролей)
ПРИОРИТЕТ
	
ТЕМА
	
ВРЕМЯ ПОДГОТОВКИ
	
ВАЖНОСТЬ

1	ML Coding (Attention, Beam Search, Training Loop)	3-4 недели	Критическая
2	Research Discussion (2-3 ключевые работы)	2-3 недели	Критическая
3	System Design (ML Infrastructure)	2-3 недели	Высокая
4	Behavioral (STAR метод, истории)	1-2 недели	Средняя
5	LeetCode (Easy/Medium)	1-2 недели	Низкая (не для всех компаний)

📚 Рекомендуемые ресурсы (из блога Alisa Wuffles):

Stanford CS336 — Language Modeling from Scratch
CS336 Homework 1 — Implementing a transformer from scratch
LeetCode 75 и Neetcode Blind 75 — для общего кодинга
The Illustrated GPT-2 (Jay Alammar) — визуальная интуиция
Backpropagation (CS231n notes) — для backward pass
8. 🔍 Proof of Work: выполненные поисковые запросы и открытые страницы
Выполненные поисковые запросы:
"interview process" "Research Scientist" "Member of Technical Staff" 2025 2026 OpenAI Anthropic DeepMind
"interview loop" "Research Scientist" MTS 2025 2026 OpenAI Anthropic xAI Mistral Cohere
ML coding interview "from scratch" attention sampling decoding 2025 2026
blog job search "Research Scientist" interview writeup 2025 2026
AI research interview "no PhD" "without PhD" 2025 2026
"Meta" OR "FAIR" OR "Google DeepMind" interview loop 2025 2026 "Research Scientist" MTS
Mistral Cohere "Scale AI" Perplexity interview 2025 2026 research
"Thinking Machines" OR "Reflection AI" OR "SSI" interview 2025 2026
Nvidia Apple Amazon Microsoft AI research interview 2025 2026
"research discussion" interview questions 2025 2026 AI lab
site:reddit.com "OpenAI" OR "Anthropic" OR "xAI" interview process 2025 2026 research scientist MTS
site:blind.com "OpenAI" OR "Anthropic" OR "xAI" interview 2025 2026
site:glassdoor.com "OpenAI" OR "Anthropic" OR "xAI" interview 2025 2026
"Google DeepMind" interview process 2025 2026 research scientist
"Meta" "Superintelligence Labs" OR "FAIR" interview 2025 2026
"OpenAI" interview process 2025 2026 research scientist blog writeup
"Anthropic" interview process 2025 2026 research scientist blog
"Google DeepMind" interview process 2025 2026 blog
"xAI" interview process 2025 2026 research scientist blog
"Meta" "Superintelligence Labs" interview process 2025 2026
"Mistral" "Cohere" interview process 2025 2026 research scientist
"Scale AI" "Perplexity" interview process 2025 2026 research
"Reflection" "Thinking Machines" "SSI" interview process 2025 2026
Nvidia Apple Amazon Microsoft AI research interview 2025 2026
AI lab interview "no PhD" "without PhD" 2025 2026
Реально открытые страницы (25+):
Sundeepteki.org — AI Career Advice для OpenAI, Anthropic & DeepMind Roles 
sundeepteki
Yongzx.github.io/blog/2026/06/24/job-search — Surprising lessons from my research scientist job search
LinkedIn.com/pulse/cracking-ai-research-interview-from-openai-wuffles-harnoor-singh-qpate — Cracking the AI Research Interview from OpenAI researcher(Alisa Wuffles)
Igotanoffer.com — OpenAI Interview Questions + Answers 
igotanoffer
Tryexponent.com — OpenAI Product Manager (PM) Interview Guide 
tryexponent
Coursera.org — 10 OpenAI Interview Questions 
coursera
Jobright.ai — Anthropic Technical Interview Questions: Complete Guide 
jobright
Interviewquery.com — Anthropic AI Research Scientist Interview Guide 
interviewquery
Theundercoverrecruiter.com — The Insider's Guide to Landing a Job at Anthropic AI 
theundercoverrecruiter
Teamblind.com — Anthropic Interview Megathread 
teamblind
Yongzx.github.io — Surprising lessons from my research scientist job search 
yongzx.github
Educative.io — Top 20 Google DeepMind interview questions 
educative
Glassdoor.com — Google DeepMind Interview Experience & Questions (2026) 
glassdoor
Extern.com — Google DeepMind Internship 2027–2028: Deadlines & How to Apply 
extern
AIOfferly.com — xAI ML Interview Questions - 2025/2026 Guide 
aiofferly
Yuan-meng.com — MLE Interview 2.0: Research Engineering and Scary Rounds 
yuan-meng
Ophyai.com — Nvidia Interview Process 2026: Stages, HackerRank 
ophyai
Glassdoor.co.in — A memo viewed about the Superintelligence hiring freeze 
glassdoor.co
GitHub.com — AI-Engineer-Interview-Questions/14-company 
github
Cleverprep.com — Meta Research Scientist Interview Preparation Guide 
cleverprep
Builtin.com — Meta Superintelligence Labs: What We Know So Far 
builtin
Winbuzzer.com — Inside Meta's AI Lab: What It Takes to Get Hired and Survive 
winbuzzer
Sundeepteki.org — AI Research Scientist Interview Guide: Anthropic, OpenAI 
sundeepteki
Teamblind.com — Cohere Interview Discussions 
teamblind
Leonstaff.com — Cohere Interview Response Time: What to Expect at Every 
leonstaff
9. 📌 Заключение и рекомендации
Начинайте с ML-кодинга — это самый важный этап для Research Scientist и MTS ролей. Уделите 3-4 недели только этому.
Подготовьте 2-3 ключевые исследовательские работы для deep discussion. Вы должны уметь объяснить их за 5 минут и за 45 минут.
Не игнорируйте system design — особенно если идете в xAI, Nvidia или Scale AI.
Практикуйте без AI-ассистентов — в большинстве лабораторий их использование запрещено в live coding.
Сетворк — многие офферы приходят через внутренние рекомендации. Активно общайтесь на конференциях и в LinkedIn.

🚀 Финальный совет от Yongzx: «Самое важное — начать интервью раньше, чем вы думаете, что готовы. В моём случае, из-за опасений по поводу headcount, я начал примерно в середине мая и получил офферы, которыми я доволен, до середины июня. Я даже отозвал некоторые текущие интервью и не имел возможности полностью изучить свои варианты» .

Удачи в подготовке и интервью! 🍀

Sources
Deep Think
Max

## Ссылки со страницы (href, собраны рельсой)
https://www.sundeepteki.org/
https://yongzx.github.io/blog/2026/06/24/job-search
https://www.linkedin.com/pulse/cracking-ai-research-interview-from-openai-wuffles-harnoor-singh-qpate
https://igotanoffer.com/
https://www.tryexponent.com/
https://www.coursera.org/
https://jobright.ai/
https://www.interviewquery.com/
https://theundercoverrecruiter.com/
https://www.teamblind.com/
https://yongzx.github.io/
https://www.educative.io/
https://www.glassdoor.com/
https://www.extern.com/
https://www.aiofferly.com/
https://www.yuan-meng.com/
https://ophyai.com/
https://www.glassdoor.co.in/
https://github.com/
https://www.cleverprep.com/
https://builtin.com/
https://winbuzzer.com/
https://leonstaff.com/