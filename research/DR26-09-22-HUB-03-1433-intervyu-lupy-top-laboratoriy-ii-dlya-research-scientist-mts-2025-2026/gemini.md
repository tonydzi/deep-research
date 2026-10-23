# DR26-09-22-HUB-03-1433 — raw report from gemini

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Upgrade
Conversation with Gemini
You said --- id: DR26-09-22-HUB-03-1433 date: 2026-09-22 14:33:11 theme: Карта интервью-лупов топ-лабораторий…

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

Gemini said
DR26-09-22-HUB-03-1433 — Карта интервью-лупов топ-лабораторий ИИ для ролей Research Scientist и MTS (2025-2026)
1. Архитектура лупов по компаниям (2025–2026)
Компания	Роли	Последовательность этапов	Длительность этапа	Общая длительность
OpenAI	RS, MTS	1. Resume Screen → 2. Recruiter (30m) → 3. HM Screen (30m) → 4. Skills Assessment (take-home / HackerRank 2.25h ИЛИ 4 live раунда) → 5. Virtual Onsite (4-6h)	Этапы по 45-60м	3–5 недель
Anthropic	RS, MTS	1. Recruiter (30m) → 2. Research Discussion (60-90m) → 3. Research Presentation → 4. Research Day (4-6h) → 5. Leadership/Alignment (1-2h)	Этапы по 60-90м	4–6 недель
Google DeepMind	RS	1. Recruiter → 2. HM/Project dive → 3. ML Coding (без ИИ) → 4. Onsite (4-5 раундов: math, taste, values) → 5. Hiring Committee	Очные раунды по 60м	6–10+ недель
xAI	MTS, RE	1. App (Statement of Exceptional Work) → 2. Engineer Call (15-30m) → 3. Tech Screen → 4. Onsite (2-3 coding/sys design) → 5. HM Follow-up	Этапы по 45-60м	2–3 недели
Mistral AI	RS, RE	1. Prescreen (30m) → 2. Coding (String math + L2 Tensor) → 3. Logic/Math oral → 4. Research Theory → 5. Code Review → 6. Behavioral → 7. Presentation & Design	Этапы по 30-60м	3–5 недель

Ограничения данных: По компаниям Cohere, Scale AI, Perplexity, SSI, Nvidia, Apple, Amazon специфичных полных write-ups по 2026 году в верифицируемых источниках не найдено. Имеются лишь фрагменты, например, Amazon включает этап "OA - AI assisted coding question".   
Reddit

2. Форматы этапов и реальные примеры задач
ML-кодинг (Implementation & Tensors)

Кандидатов просят реализовывать ML-компоненты с нуля без фреймворков высшего уровня.

DeepMind: «Implement multi-head self-attention from scratch in NumPy. No autocompletion. 30 minutes max».   
AimVantage
+ 2

Mistral: Написание L2-дистанций для матриц входных данных X (N×D) и центров кластеров C (K×D) (тензорное программирование). Реализация математики на строках без конвертации в int.   
Ophy AI

ML Debugging & Systems (Инфраструктура)

Mistral (Код-ревью): Кандидату выдается класс replay buffer для RL-пайплайна (с батчингом, хранением транзишенов). В коде 8 заложенных багов. Дается 45 минут на их изоляцию и исправление.   
Sundeep Teki

xAI (ML Debugging): «A model-quality metric regressed after a systems optimization». Требуется пошагово изолировать причину: данные, numerical precision, батчинг или kernel behavior.   
Ophy AI
+ 1

ML Math & Derivations

DeepMind: Классический математический дрилл у доски. Примеры вопросов 2026 года: вывод градиента softmax cross-entropy, derivation of layer norm, KL divergence.   
AimVantage
+ 1

Mistral: Устные логические, вероятностные и математические пазлы. Задаются исключительно голосом без текста на экране, требуют решения в реальном времени.   
Ophy AI

Research Discussion / Job Talk (Защита портфолио)   
CleverPrep

Презентация с глубоким погружением в одну статью. Хронологический пересказ резюме считается провалом.

OpenAI: Ожидается структура: проблема, гипотеза, метод, baselines, метрики, результат, анализ неудач (failure analysis) и следующий эксперимент. Ключевой триггер: «Why this, and what simpler alternative did you reject?».   
PracHub
+ 2

DeepMind (Research Taste): Старший исследователь критикует методологию кандидата. Далее кандидата просят выбрать недавнюю статью самой лаборатории DeepMind и обоснованно разгромить ее выводы.   
AimVantage
+ 2

System Design для ML-инфраструктуры

Mistral (Out-of-domain design): Спроектировать систему эффективного исполнения ML-нагрузок на CPU. Задача специально дается без конкретики, чтобы проверить умение выявлять bottleneck (сеть, память или вычислитель).   
Reddit
+ 2

xAI: «Design a high-throughput inference service». Ожидается глубокое понимание роутинга, dynamic batching, управления KV-кэшем, автоскейлинга и latency objectives.   
Custom Career Content - University of Miami
+ 1

Behavioral & Values (Поведенческое)

Anthropic: Вопросы на Alignment. «How would you evaluate whether a language model is aligned?», разбор challenges of interpretability.   
Sundeep Teki
+ 1

DeepMind: Раунд Values в 2026 году ищет кандидатов, способных «удерживать сложность». Пример: «There are projects you would refuse to work on. Tell me about one and why».   
AimVantage
+ 1

3. Отличия между лабораториями

Использование ИИ-ассистентов: Правила диаметрально противоположны. Meta внедрила отдельный раунд «Coding with AI». DeepMind категорически запрещает Copilot, Cursor, GPT — бан на любые генеративные тулзы во время технических сессий. В OpenAI правила плавающие и зависят от команды (уточнять у рекрутера).   
Reddit
+ 3

Культурные раунды: Anthropic проводит глубокие Leadership интервью (1-2 часа). xAI полностью отказалась от поведенческих и этических раундов в инженерном лупе; 100% времени уходит на технический скрининг и кодинг.   
IGotAnOffer
+ 1

LeetCode vs. Applied: xAI не использует фиксированный банк LeetCode вопросов, упор делается на прикладное ООП и дизайн классов (баг-фри код ценится выше теоретически оптимального). Mistral миксует классический LeetCode с ИИ-кодингом.   
Reddit

Таймлайны найма: DeepMind славится самым долгим процессом в индустрии (до 10 недель, медленный Hiring Committee). xAI, напротив, агрессивно закрывает луп за 2-3 недели от первого звонка до оффера.   
Custom Career Content - University of Miami

4. Критерии оценки: Сигналы найма и Red Flags   
Reddit

Сигналы сильного найма (Strong Hire):

Раскрытие неудач. В OpenAI и Anthropic ценятся исследователи, которые открыто говорят о своих failed hypotheses, confounders и uncertainty. Упаковка каждого эксперимента как "абсолютного успеха" снижает доверие.   
Reddit
+ 1

Высокий Ownership. Способность аргументировать архитектурные выборы из первых принципов без использования buzzwords.   
Nora AI

Red Flags (Частые причины отказов):

Поверхностное знание своей же статьи. В DeepMind ожидают знания каждого графика, ablation и фидбека от ревьюеров вашей статьи. Незнание деталей приравнивается к отсутствию авторства.

Отказ от позиции. Ответ «I think both views have merit» на раунде Research Taste в DeepMind — быстрый путь к отказу.   
Reddit
+ 1

Отрыв от "железа". В Mistral игнорирование compute cost, memory bandwidth и latency при ответе на вопросы по качеству генерации модели считается провалом.   
Reddit

5. Требования к кандидатам БЕЗ PhD и публикаций   
Reddit

Для инженеров без публикаций путь открыт на роли MTS (Member of Technical Staff) и Research/Applied Engineer.   
Reddit

xAI: Идеальный таргет. Лаборатория не требует PhD и фокусируется на скорости написания кода и системной инженерии. Первый фильтр — «Statement of exceptional work». Кастомный распределенный инференс-сервис или сложный CUDA-kernel, зашипленный в условиях жесткого дедлайна, оценивается выше научных статей.   
Custom Career Content - University of Miami
+ 1

OSS-вклады: В Mistral глубокое понимание GPU-архитектуры и программирования на Triton проверяется уже на первом 30-минутном скрининге. Коммиты в open-source фреймворки (vLLM, Triton, TRL) служат мощным прокси-сигналом для рекрутеров.   
Reddit

OpenAI: Если вы подаетесь на MTS без статей, резюме должно квантифицировать инженерный импакт: снижение костов, масштабирование тренировочного кластера или рост метрик надежности.   
PracHub

6. Изменения 2025 → 2026   
Reddit

Усложнение Research Taste: В лупах DeepMind London 2026 года появились конфронтационные вопросы: «You disagree with a senior researcher on an experimental conclusion. Walk me through the next 15 minutes».   
AimVantage
+ 1

Сдвиг Values Round: На фоне контрактов с военными ведомствами и юнионизации в индустрии, раунд ценностей (особенно в DeepMind) перестал искать радикальных «AI doomers» или евангелистов. Оценивается взвешенность: «Я понимаю риски, но понимаю, почему мы должны это построить».   
PracHub

Специфика Agent-архитектур: На алгоритмических секциях стали появляться задачи, завязанные на архитектуру RL-агентов (например, код-ревью replay buffer), а не только классический LLM-inference.

7. Что готовить и в каком порядке (Roadmap)   
Reddit

Учитывая фокус на multi-agent/Web3 и комплексной инженерии, приоритет подготовки следует выстроить так:

Transformer Muscle Memory (Недели 1-2): Написать с нуля без AI-помощников (NumPy/PyTorch): Multi-head attention, Layer Norm, Forward pass, Sampling/Decoding. Лимит — 30 минут без синтаксических ошибок. Alisa Liu (успешный кандидат) называет это важнейшим навыком, который нужно довести до автоматизма.   
AimVantage

Системы и GPU (Недели 3-4): Разобрать архитектуру памяти GPU, Triton, управление KV-кэшем, distributed training pipelines (Data/Tensor/Pipeline parallelism). Фокус на узкие места памяти.

Paper/Project Defense Deep Dive (Недели 5-6): Подготовить один свой проект (например, из multi-agent оркестрации) до уровня защиты PhD. Выписать все weaknesses, alternatives, baselines и что бы вы сделали иначе.   
Reddit

ML Math Drill (Недели 7-8): Вывод градиентов у доски. Softmax cross-entropy, KL divergence, Backpropagation.   
Reddit
+ 1

Research Taste / Сценарное мышление (Недели 9-10): Написать 300-словный манифест «Что я откажусь строить и почему?». Подготовить обоснованную критику 2-3 последних пейперов той лаборатории, в которую идете.   
PracHub
+ 1

Proof of Work

Выполненные запросы (Search queries):

"Research Scientist" OR "Member of Technical Staff" interview loop OpenAI OR Anthropic OR DeepMind 2024 OR 2025 OR 2026

site:alisawuffles.github.io job search interview

site:[reddit.com/r/MachineLearning](https://reddit.com/r/MachineLearning) "interview" "Anthropic" OR "OpenAI" OR "DeepMind" 2024 OR 2025

"Mistral" OR "xAI" OR "SSI" interview loop "Research Scientist" OR "Member of Technical Staff"

site:teamblind.com "xAI" "MTS" interview

site:teamblind.com "OpenAI" "Research Scientist" interview 2025

AI lab interview "take-home" "live coding" 2025

Проанализированные источники: Практические гайды PracHub (OpenAI 2026), CleverPrep (Anthropic 2026), AimVantage (DeepMind 2026), Ophy AI (Mistral & xAI 2026), Reddit (r/OfferEngineering, r/leetcode), Nora AI, блог Alisa Liu.




Pro

Gemini is AI and can make mistakes, including about people. Your privacy and Gemini
Opens in a new window

Defining Technical Aspects