# DR26-10-16-HUB-02-0652 — raw report from gemini

> Unedited output of one model. Our reconciled synthesis is the page one level up.

search
Conversation with Gemini
You said # DR26-10-16-HUB-02-0652 — Дистилляция LLM и извлечение моделей Проведи глубокое исследование дисти…




# DR26-10-16-HUB-02-0652 — Дистилляция LLM и извлечение моделей




Проведи глубокое исследование дистилляции LLM и model extraction. Объясни teacher-student, knowledge distillation, soft targets/logits/temperature, sequence-level SFT, synthetic data, reasoning traces и RL. Что переносится: факты, навыки, поведение; что нельзя заключить про внутренние вычисления. Можно ли восстановить исходные веса по ответам? Раздели black-box imitation, parameter extraction и кражу checkpoint. Проверь Hinton 2015, Distilling Step-by-Step, Orca, imitation models, DeepSeek-R1 distillation, Stealing Part of a Production Language Model 2024. Проверь публичные заявления OpenAI и Anthropic о китайских компаниях и массовых аккаунтах, даты и доказательства, не выдавай обвинения за независимый факт. Объясни экономику, роль предобученной базовой модели и ограничения обобщения. Не нужны инструкции обхода блокировок, регистрации фальшивых аккаунтов или сокрытия трафика. Нужен понятный русский отчёт, схема процесса, точные условия результатов и открытые первоисточники со ссылками. Проверь гипотезы: знания не дистиллируются; копируются именно веса; текст рассуждений равен внутренней логике. Для каждой дай вердикт с границами применимости.

Gemini said

Вот план исследования для этой темы. Сообщите мне, если что-то надо поменять.

 Дистилляция и извлечение LLM 
Исследовать сайты
(1) Исследовать теоретические основы дистилляции языковых моделей: архитектуру Teacher-Student, фундаментальную работу Hinton 2015, концепции логитов, soft targets, температуры, SFT на уровне последовательностей, генерацию синтетических данных, переносы трассировок рассуждений и применение обучения с подкреплением.
(2) Проанализировать методы извлечения моделей и провести строгое разграничение между black-box подражанием, извлечением параметров на основе статьи Stealing Part of a Production Language Model 2024 и прямой утечкой контрольных точек моделей, оценив возможность полного восстановления весов.
(3) Изучить ключевые практические работы и архитектуры: Distilling Step-by-Step, семейство моделей Orca, исследования моделей-имитаторов и подходы к дистилляции в DeepSeek-R1, зафиксировав, какие именно аспекты переносятся, а какие остаются недоступными.
(4) Найти публичные заявления OpenAI и Anthropic о массовых аккаунтах и активности сторонних компаний, собрать даты, фактические доказательства и контекст этих заявлений, сохраняя нейтральный тон без представления утверждений как доказанных фактов.
(5) Оценить экономику дистилляции, влияние базовой предобученной модели на результат и ограничения способности дистиллированных моделей к обобщению.
(6) Проанализировать три заданные гипотезы: о недистиллируемости знаний, о копировании весов и о равенстве текста рассуждений внутренней логике модели, сформулировав вердикты с конкретными границами применимости.
More
Анализировать результаты
Создать отчет
Результат будет готов через несколько минут
 Изменить план 
 Начать исследование 
You said Начать исследование

Начать исследование

Gemini said

I've completed your research. Feel free to ask me follow-up questions or request changes.

Дистилляция и извлечение LLM
15 Oct, 23:56




Deep Research
Thinking
Sources
Files

Gemini is AI and can make mistakes, including about people. Your privacy and Gemini
Opens in a new window

Дистилляция и извлечение LLM
Contents
Share and export
Create
Дистилляция LLM и извлечение моделей: механизмы, граница воспроизводимости и архитектурный анализ
Теоретический фундамент и классическая дистилляция знаний

Концептуальная база передачи знаний между нейросетевыми архитектурами сформировалась в рамках парадигмы «учитель — ученик» (Teacher-Student framework). В этой схеме избыточная по параметрам модель-учитель со скрытой вычислительной емкостью транслирует накопленные закономерности более компактной модели-ученику. Классический подход к дистилляции знаний (Knowledge Distillation, KD), сформулированный Джеффри Хинтоном и соавторами в 2015 году, основывается на использовании «мягких меток» (soft targets) вместо жестких бинарных классификационных векторов.   

В стандартном обучении с учительским контролем нейронная сеть оптимизируется относительно жестких распределений (one-hot vectors), что принуждает модель максимизировать вероятность единственного правильного класса. Однако логиты 
zi
z
i
	​

, генерируемые предпоследним слоем перед активацией Softmax, содержат информацию о взаимной близости и структуре классов — так называемые «темные знания» (dark knowledge). Для извлечения этих энтропийных связей Хинтон предложил масштабирование логитов с помощью температурного параметра 
T
T:   

qi=exp⁡(zi/T)∑jexp⁡(zj/T)
q
i
	​

=
∑
j
	​

exp(z
j
	​

/T)
exp(z
i
	​

/T)
	​


При 
T=1
T=1 функция превращается в стандартный Softmax, генерирующий пикообразные вероятностные распределения. При увеличении параметра 
T>1
T>1 распределение становится более сглаженным, проявляя малые вероятности для неправильных классов. Это позволяет модели-ученику усваивать не только целевой класс, но и корреляционную структуру пространства признаков.   

Суммарная функция потерь при классической дистилляции представляет собой взвешенную комбинацию кросс-энтропии с жесткими метками 
LCE
L
CE
	​

 и дивергенции Кульбака-Лейблера 
DKL
D
KL
	​

 между сглаженными распределениями учителя 
pTteacher
p
T
teacher
	​

 и ученика 
pTstudent
p
T
student
	​

:   

LKD=(1−α)LCE(y,pT=1student)+αT2DKL(pTteacher∥pTstudent)
L
KD
	​

=(1−α)L
CE
	​

(y,p
T=1
student
	​

)+αT
2
D
KL
	​

(p
T
teacher
	​

∥p
T
student
	​

)

Множитель 
T2
T
2
 необходимо вводить для компенсации уменьшения масштаба градиентов при делении логитов на 
T
T.   

Компонент дистилляции	Математическое выражение / Назначение	Влияние на модель-ученика
Логиты (
z
i
	​

)	Ненормированные выходные значения слоя проекции	Фиксируют абсолютное распределение уверенности модели
Температура (
T)	Масштабирующий делитель в функции Softmax	Регулирует энтропию распределения и сглаживание вероятностей
Мягкие метки (
q
i
	​

)	Вероятности при 
T>1	Передают межкатегориальные корреляции и градиентные тонкости
Дивергенция KL	
∑p
T
teacher
	​

log(p
T
teacher
	​

/p
T
student
	​

)	Согласует распределения вероятностей двух нейросетей
Иерархия методов передачи знаний в современных языковых моделях

Переход от классических нейросетей к авторегрессионным языковым моделям (LLM) потребовал трансформации методов дистилляции. В контексте обработки естественного языка дистилляция разделилась на посимвольный/пословный уровень (Word-level KD) и уровень последовательностей (Sequence-level KD). Пословная дистилляция минимизирует расхождение распределений на каждом шаге генерации токена при известном предыдущем контексте. В свою очередь, фундаментальная работа Кима и Раша (Kim & Rush, 2016) показала ограниченность пословного переноса для последовательных задач из-за накапливания ошибок при авторегрессионном декодировании. Авторы предложили Sequence-level Knowledge Distillation, в рамках которого модель-учитель с помощью лучевого поиска (beam search) генерирует наиболее вероятную полную последовательность 
y~
y
~
	​

 для входного текста 
x
x:   

y~=arg⁡max⁡yq(y∣x)
y
~
	​

=arg
y
max
	​

q(y∣x)

Модель-ученик затем обучает свои параметры методом тонкой настройки на размеченных учителем данных (Supervised Fine-Tuning, SFT), минимизируя стандартную отрицательную логарифмическую правдоподобность. Это приближение сводит задачу дистилляции модального значения распределения учителя к стандартному SFT на синтетическом корпусе.   

Появление мультизадачных моделей масштаба GPT-4 сместило фокус с простой передачи ответов на передачу логических цепочек. В исследовании Distilling Step-by-Step (Hsieh et al., 2023) было продемонстрировано, что обучению ученика способствует генерация учителем не только итогового ответа, но и промежуточных текстовых обоснований (Chain-of-Thought, CoT). Мультизадачный лосс в этом случае включает предсказание ответа и генерацию текста рассуждения.   

Проект Orca (Mukherjee et al., 2023) расширил эту концепцию до промышленного масштаба. При дистилляции знания из GPT-4 в модели семейства Llama применялись детализированные системные инструкции (System Prompts), поощряющие генерацию пошаговых объяснений, структуры аргументации и самопроверки. Это позволило ученику с 13 млрд параметров усвоить стилистические и процедурные навыки рассуждения, значительно превосходя базовые SFT-модели сходного размера.   

Процесс переноса знаний в современном цикле дистилляции включает последовательное прохождение нескольких этапов. На начальном этапе запрос пользователя поступает на модель-учитель высокого уровня, такую как GPT-4 или DeepSeek-R1. Модель-учитель формирует не только итоговый вердикт, но и детализированные траектории рассуждений (Reasoning Traces) вместе с пошаговыми объяснениями. Собранные ответы агрегируются в синтетический датасет, на котором затем происходит SFT-обучение или RL-выравнивание компактной целевой модели-ученика.   

Выход модели DeepSeek-R1 (2025) продемонстрировал новый этап в эволюции дистилляции. Модель DeepSeek-R1 генерирует длинные траектории рассуждений, сформированные в процессе крупномасштабного обучения с подкреплением (Reinforcement Learning, RL) без первоначального SFT. Для дистилляции этих способностей разработчики собрали около 800 000 образцов данных, включающих явные цепочки рассуждений R1, и провели прямую SFT-настройку открытых базовых моделей Qwen и Llama (размером от 1.5B до 70B параметров). Результаты DeepSeek-R1 показали, что прямое обучение компактной модели на траекториях рассуждений, созданных RL-учителем, обеспечивает более высокие метрики в математике и программировании, чем попытки применить RL непосредственно к маленькой базовой модели.   

Разграничение векторов атак и способов извлечения моделей

В области безопасности машинного обучения термин «извлечение моделей» объединяет кардинально разные по механике, требованиям и результатам векторы атак. Для точного анализа необходимо четко разделять имитацию внешнего поведения (Black-box imitation), математическое извлечение параметров (Parameter extraction) и прямую кражу чекпоинта (Checkpoint theft).   

Черноящичная имитация (Black-box imitation) заключается в отправке большого количества запросов к целевой модели через публичный API и использовании полученных сгенерированных текстов для обучения собственной модели посредством SFT. При такой атаке веса оригинальной модели не восстанавливаются. Происходит обучение модели-ученика попадать в то же распределение выходов для заданного распределения входов. Это позволяет скопировать формат ответов, стилистику, языковую норму и базовые шаблоны решения задач, но не дает доступа к внутренней архитектуре или весам целевой модели.   

Извлечение параметров (Parameter extraction) представляет собой атаку, направленную на строгое математическое восстановление численных значений весов или геометрии внутренних слоев модели через интерфейсы обратной связи API. В работе Карлини и соавторов Stealing Part of a Production Language Model (2024) описан метод извлечения параметров последнего линейного слоя трансформера. Авторегрессионный трансформер выражает вероятности токенов через функцию 
f(p)=W⋅gθ(p)
f(p)=W⋅g
θ
	​

(p), где 
gθ(p)
g
θ
	​

(p) — скрытое представление размерности 
h
h, генерируемое основными блоками внимания, а 
W∈Rv×h
W∈R
v×h
 — матрица проекции на словарь размерности 
v
v. Поскольку размерность скрытого состояния значительно меньше размера словаря (
h≪v
h≪v), выходы логитов лежат в подпространстве размерности 
h
h.   

Авторы доказали, что извлечение полного вектора логитов для 
n
n различных промптов формирует матрицу ответов 
Q∈Rn×v
Q∈R
n×v
. Применение сингулярного разложения (SVD) к матрице 
Q
Q позволяет определить точный размер скрытой размерности 
h
h по резкому падению величины сингулярных чисел. Для полного извлечения матрицы 
W
W авторы использовали специфические API-параметры, такие как logit_bias и точные значения logprobs. Извлечение выполняется с точностью до неизвестного ортогонального преобразования (изоморфизма матрицы 
h×h
h×h), так как для любой обратимой матрицы 
A
A справедливо выражение 
(WA−1)⋅(Agθ(p))=Wgθ(p)
(WA
−1
)⋅(Ag
θ
	​

(p))=Wg
θ
	​

(p). Специфика атаки Карлини заключается в восстановлении проекционного слоя 
W
W и скрытой размерности 
h
h (например, для моделей GPT-3 Ada, Babbage и GPT-3.5-turbo), но эта методика не позволяет извлечь веса предшествующих внутренних слоев Transformer.   

Кража чекпоинта (Checkpoint theft) подразумевает физическое или операционное хищение полных файлов весов модели (векторов параметров) из сетевой инфраструктуры, облачного хранилища или артефактов обучения. Это не является алгоритмической атакой извлечения через API, а представляет собой инцидент информационной безопасности. Кража чекпоинта гарантирует абсолютную эквивалентность функционала, так как атакующий получает оригинальную модель целиком.   

Критерий сравнения	Black-Box Imitation	Parameter Extraction (Carlini 2024)	Checkpoint Theft
Доступный интерфейс	Текстовый API (генерация)	Точные logprobs / logit_bias	Доступ к файловой системе / S3
Извлекаемые компоненты	Поведение, стиль, знания	Размерность 
h, матрица 
W	100% весов всех слоев
Точность совпадения весов	Отсутствует (
0%)	Полная для слоя 
W (up to rotation)	Абсолютная (
100%)
Затраты на атаку	Стоимость токенов SFT-вызовов	
<$2000 на запросы к проекции	Нелегитимный доступ / взлом
Зависимость от базы	Требует базовой LLM ученика	Не требует обучения, чистый расчет	Не требует дополнительного обучения

Восстановление полных весов глубинного трансформера исключительно по текстовым ответам (без точных floating-point значений логитов для всех токенов словаря) является математически недоопределенной задачей. Авторегрессионный сэмплинг (top-
k
k, top-
p
p, temperature) отбрасывает подавляющую часть информации о непрерывном пространстве логитов. Даже при наличии точных логитов нелинейные слои GELU/SwiGLU и блоки Self-Attention образуют многопараметрическую систему с огромным количеством степеней свободы, где бесконечное множество различных комбинаций внутренних весов дает идентичные текстовые последовательности на выходе.   

Физика переноса: переносимые элементы и скрытые вычисления

Процесс дистилляции осуществляет избирательный перенос характеристик целевой системы, создавая четкую грань между внешне наблюдаемыми функциями и внутренними вычислительными состояниями.   

К переносимым элементам относятся:

Фактологические связи и знания, которые передаются в той мере, в какой они с высокой плотностью представлены в синтетическом корпусе.   

Процедурные навыки и форматирование, включающие алгоритмы решения задач, вызов внешних функций (Tool use), генерацию кода заданной структуры и соблюдение сложных форматов разметки.   

Стилистические нормы и языковой профиль, охватывающие тональность, уровень вежливости, лаконичность и специфические текстовые маркеры рассуждений.   

К непереносимым и скрытым элементам относятся:

Внутреннее пространство активаций, поскольку пространственные векторы на скрытых слоях модели-ученика и модели-учителя не совпадают из-за различий в архитектуре, размерности 
dmodel
d
model
	​

 и количестве слоев.   

Внутренняя динамика вычислений, так как модель-ученик не воспроизводит внутренние карты внимания (Attention maps) учителя, а аналогичный внешний результат достигается за счет иных вычислительных траекторий в графе нейронной сети.   

Неизвлекаемый внутренний контекст, включающий промежуточные вычисления, скрытые в векторных активациях до этапа токенизации и не отраженные прямо в текстовом выходе.   

Экономика, роль базовой модели и ограничения обобщения

Обучение современной флагманской языковой модели с нуля (Pre-training) требует затрат, исчисляемых десятками миллионов долларов, на организацию инфраструктуры из тысяч графических ускорителей (GPU) и обработку триллионов токенов. Дистилляция кардинально меняет структуру расходов. Затраты при дистилляции складываются из вызова API модели-учителя для генерации синтетического датасета и последующей тонкой настройки (SFT/LoRA) компактной базовой модели на полученных данных. В результате итоговая стоимость разработки модели с высокой функциональностью в конкретных доменах снижается на 2–3 порядка, что делает дистилляцию главным драйвером создания специализированных и мобильных AI-систем.   

Распространенным заблуждением является представление о дистилляции как о процессе записи знаний в «пустую» сеть. В действительности дистилляция через SFT/CoT выступает в роли механизма выравнивания (alignment) и активации уже имеющихся знаний базовой модели-ученика. Если базовая модель ученика не прошла этап предварительного обучения на богатом текстовом корпусе и не содержит необходимых представлений в своих весах, дистилляция синтетических данных не способна обучить ее сложным фактам. Дистилляция перераспределяет вероятностные массы базовой модели, заставляя ее активировать правильные подсети для решения конкретных задач.   

Модели, обученные путем дистилляции ответов учителя (Imitation Models), демонстрируют существенное снижение качества при выходе за пределы обучающего распределения (Out-of-Distribution, OOD). Это проявляется в эффекте поверхностной имитации (Superficial Imitation), когда ученик отлично перенимает уверенный тон, стилистику и структуру ответов учителя, но не обладает аналогичной глубиной логической проверки. В результате модель сгенерирует грамматически безупречные, но ложные или галлюцинаторные рассуждения. Кроме того, проявляется хрупкость в нестандартных сценариях: при изменении формулировки задачи или смене краевых условий дистиллированная модель быстро теряет точность, так как у нее отсутствует широкая фундаментальная база знаний, на которую опирался учитель.   

Анализ публичных заявлений, регуляторного контекста и практик индустрии

В период 2023–2026 годов тематика дистилляции моделей стала центральным узлом технологического и геополитического противостояния между ведущими лабораториями США (OpenAI, Anthropic) и китайскими разработчиками (ByteDance, DeepSeek, Zhipu AI / Z.AI, Moonshot AI, MiniMax).   

Хронология ключевых событий показывает динамику развития этого конфликта:

В декабре 2023 года OpenAI приостановила учетную запись ByteDance после того, как в прессу просочилась информация об использовании ByteDance API OpenAI (включая GPT-4) для генерации обучающих данных при создании собственной конкурентной модели. OpenAI заявила о прямом нарушении Условий обслуживания (Terms of Service, ToS), запрещающих использование выходов API для обучения конкурирующих моделей.   

С февраля 2024 по февраль 2026 года Anthropic опубликовала серию отчетов о безопасности, в которых заявила о фиксации систематических, промышленных кампаний по извлечению возможностей модели Claude. По данным Anthropic, китайские компании, включая DeepSeek, Moonshot AI и MiniMax, использовали сети из более чем 24 000 фальшивых аккаунтов и прокси-серверов, сгенерировав свыше 16 миллионов диалогов для извлечения данных.   

В сентябре 2026 года Агентство по кибербезопасности и инфраструктуре США (CISA) выпустило официальное уведомление (Advisory AA26-251A), в котором обвинило ряд китайских компаний в проведении «промышленных дистилляционных кампаний» против американских моделей (Claude 3.7/4/4.5, GPT-4o/5, Gemini 2.5/3 Pro) с целью передачи технологий в системы двойного назначения.   

В октябре 2026 года Anthropic и OpenAI заявили о внедрении специализированных механизмов защиты от дистилляции, включая двухэтапные классификаторы поисковых промптов, ограничение доступа к скрытым цепочкам рассуждений (CoT) и блокировку связанных пулов аккаунтов. Anthropic отметила снижение объема извлекаемых токенов рассуждений на 90–99%.   

При оценке данных событий необходимо проводить разграничение между юридическими формулировками ToS, публичными заявлениями заинтересованных корпораций и независимыми академическими фактами. Публичные обвинения в «краже» или «атаках» в большинстве случаев касаются нарушения условий использования коммерческого API (ToS). Запросы, направляемые в API, являлись стандартными HTTP-вызовами без взлома баз данных или эксплуатирования уязвимостей кода. Доказательства, приводимые в корпоративных блогах (такие как совпадение структуры промптов, аномалии в расписании запросов, корреляция платёжных данных и общая IP-инфраструктура), являются косвенными паттернами поведения пользователей API. При этом выпуск китайскими разработчиками (например, DeepSeek) весов своих моделей с подробными отчетами показал, что ключевым драйвером их успехов явились оригинальные архитектурные решения (Multi-head Latent Attention, DeepSeekMoE) и алгоритмы RL, а дистилляция данных применялась на этапе SFT как стандартный в индустрии метод подготовки синтетических датасетов.   

Компания / Лаборатория	Обвинения / Заявления вендоров США	Зафиксированная методология	Подтвержденный академический факт
ByteDance	

Нарушение ToS OpenAI, использование API для обучения коммерческой модели (2023)

	

Использование ответов GPT-4 через API-ключи посредников

	

Блокировка аккаунтов вендором; факт использования синтетических данных


DeepSeek	

Массовая дистилляция Claude и GPT-4 для моделей R1/V3 (2024–2026)

	

Сбор синтетических датасетов CoT и решений математических задач

	

Открытая публикация отчетов DeepSeek о дистилляции R1 в компактные базовые модели


Moonshot AI / Zhipu AI	

Систематическое извлечение CoT и кибер-навыков через сетки аккаунтов (2026)

	

Коррелированные API-запросы, обход ограничений через прокси

	

Внедрение вендорами защитных классификаторов и систем скрытия CoT

  
Проверка гипотез и границы их применимости

Анализ теоретических моделей и эмпирических данных позволяет вынести вердикты по ключевым гипотезам в области дистилляции и экстракции моделей.

Гипотеза 1: «Знания не дистиллируются»

Вердикт: Частично верно / Ложно в общем виде.

Границы применимости определяющим образом зависят от природы знаний. Понятийные структуры, синтаксические правила, языковые стили, процедуры логического вывода и алгоритмические шаблоны (программирование, форматирование) дистиллируются с высокой эффективностью. Компактная модель успешно перенимает способность решать задачи в узких доменах. Однако реляционные факты, отсутствующие в исходном предобученном корпусе базовой модели-ученика, практически не фиксируются в ее долгосрочной памяти только за счет SFT-дистилляции. Если у модели-ученика недостаточна емкость параметров для сохранения фактологического графа, дистилляция приводит к воспроизведению формального стиля ответа при одновременном возникновении галлюцинаций.   

Гипотеза 2: «Копируются именно веса»

Вердикт: Ложно для классической и Black-box дистилляции; Ограниченно верно исключительно для специфических математических атак на проекционные слои.

В подавляющем большинстве практических сценариев дистилляции и imitation learning веса модели-учителя не копируются. Модель-ученик имеет собственную архитектуру, иное количество слоев и инициализируется случайным образом (или берется независимая базовая модель). Обучение меняет веса ученика в направлении согласования выходов, а не значений параметров. Лишь в узком классе атак на экстракцию параметров (Carlini et al., 2024) извлекаются точные числовые значения матрицы последнего линейного слоя проекции 
W
W с точностью до ортогонального вращения. Однако даже в этом случае веса всех внутренних слоев Attention и MLP остаются некопируемыми.   

Гипотеза 3: «Текст рассуждений равен внутренней логике модели»

Вердикт: Ложно.

Текстовая цепочка рассуждений (Chain-of-Thought / Reasoning trace) представляет собой авторегрессионно сгенерированную последовательность языковых токенов. Она является внешней текстовой интерпретацией, создаваемой моделью шаг за шагом. Внутренняя логика работы трансформера оперирует непрерывными векторами высокоразмерных активаций, тензорами внимания и нелинейными трансформациями в скрытом пространстве. Модель может прийти к определенному результату в силу специфики векторных активаций, а затем сформировать правдоподобный текст рассуждения, который является апостериорной рационализацией (post-hoc rationalization), а не точным отражением последовательности внутренних векторных операций.   

Заключение

Исследование процессов дистилляции языковых моделей и методов извлечения показывает, что дистилляция представляет собой высокоэффективный механизм сжатия и оптимизации функциональных возможностей нейросетей. Перенос знаний через мягкие Targets, синтетические данные и траектории рассуждений (CoT) позволяет создавать компактные модели, сопоставимые по точности в целевых задачах с флагманскими системами.   

Однако дистилляция не является механизмом полного дублирования внутренних структур: веса глубоких слоев не копируются, а генерируемый текст рассуждений не равен изоморфному отображению скрытых векторных вычислений. Восстановление точных параметров через API ограничено линейными проекциями выходного слоя и требует специфических условий доступа. Успех дистилляции критически зависит от качества предварительного обучения базовой модели-ученика, которая определяет фундаментальный предел обобщения и устойчивости создаваемой системы.   

Sources used in the report
devopedia.org
Knowledge Distillation - Devopedia
Opens in a new window
medium.com
Distilling the Knowledge in a Neural Network | by Kelvin - Medium
Opens in a new window
pub.towardsai.net
Knowledge Distillation In Neural Network | by Sovit Nayak | Towards AI
Opens in a new window
calgaryml.com
How Knowledge Distillation Impacts Fairness and Bias in AI Models
Opens in a new window
arxiv.org
Curriculum Temperature for Knowledge Distillation - arXiv
Opens in a new window
arxiv.org
arXiv:1606.07947v4 [cs.CL] 22 Sep 2016
Opens in a new window
huggingface.co
Everything You Need to Know about Knowledge Distillation
Opens in a new window
arxiv.org
Sequence-Level Knowledge Distillation - arXiv
Opens in a new window
arxiv.org
[1606.07947] Sequence-Level Knowledge Distillation - arXiv
Opens in a new window
arxiv.org
Progressive Learning from Complex Explanation Traces of GPT-4
Opens in a new window
syncedreview.com
Microsoft's Orca Learns From Complex Explanation Traces of GPT-4
Opens in a new window
portkey.ai
Distilling Step-by-Step! Outperforming Larger Language Models with
Opens in a new window
drpress.org
Enhancing the Mathematical Reasoning Ability of Small Language
Opens in a new window
microsoft.com
Orca: Progressive Learning from Complex Explanation Traces of
Opens in a new window
stafforini.com
DeepSeek-R1: Incentivizing reasoning capability in LLMs via
Opens in a new window
arxiv.org
DeepSeek-R1: Incentivizing Reasoning Capability in LLMs ... - arXiv
Opens in a new window
raw.githubusercontent.com
Stealing Part of a Production Language Model - GitHub
Opens in a new window
self-supervised.cs.jhu.edu
Stealing Part of a Production Language Model
Opens in a new window
not-just-memorization.github.io
Stealing Part of a Production Language Model
Opens in a new window
legalagent.co.jp
Generative AI News | AI Legal Lab - LegalAgent
Opens in a new window
treblle.com
The Attack That Looked Like Nothing at All: Anthropic's Distillation
Opens in a new window
arxiv.org
Stealing Part of a Production Language Model - arXiv
Opens in a new window
cisa.gov
China-Based Artificial Intelligence Companies Conducting Industrial
Opens in a new window
m.economictimes.com
ET AI Explainer: Why are US AI labs wary of 'distillation attacks' from Chinese peers
Opens in a new window
dev.to
Doubao AI: ByteDance's AI Empire Is Off-Limits - DEV Community
Opens in a new window
huggingface.co
Progressive Learning from Complex Explanation Traces of GPT-4
Opens in a new window
reddit.com
Can an AI train another AI? : r/ArtificialInteligence - Reddit
Opens in a new window
thezvi.wordpress.com
AI #43: Functional Discoveries - Don't Worry About the Vase
Opens in a new window
intro-act.com
METAVERSE NEWSLETTER Monthly Metaverse Update ... - Intro-act
Opens in a new window
the-decoder.com
Anthropic says it cut Claude reasoning token theft by up to 99% in fight against industrial-scale distillation
Opens in a new window
anthropic.com
Detecting and preventing distillation attacks - Anthropic
Opens in a new window
pasqualepillitteri.it
Anthropic unveils distillation defenses and names Zhipu and Kimi
Opens in a new window
anthropic.com
Strengthening our defenses against distillation attacks - Anthropic
Opens in a new window
unite.ai
Anthropic Reports Distillation Defenses Reduced Extracted Reasoning Tokens
Opens in a new window
Sources read but not used in the report
arxiv.org
[2403.06634] Stealing Part of a Production Language Model - arXiv
Opens in a new window
huggingface.co
Paper page - Stealing Part of a Production Language Model
Opens in a new window
github.com
sramshetty/stealing-part-of-an-LM: An unofficial ... - GitHub
Opens in a new window
youtube.com
Stealing Weights of a Production LLM Like OpenAI's ... - YouTube
Opens in a new window
nicholas.carlini.com
Papers | Nicholas Carlini
Opens in a new window
theguardian.com
US AI giant accuses Chinese rivals of mass data theft - The Guardian
Opens in a new window
arxiv.org
[2305.12057] Accurate Knowledge Distillation with n-best Reranking
Opens in a new window
aclanthology.org
Sequence-Level Knowledge Distillation - ACL Anthology
Opens in a new window
aclanthology.org
Selective Knowledge Distillation for Neural Machine Translation
Opens in a new window
ojs.aaai.org
Selective Knowledge Distillation for Non-Autoregressive Neural
Opens in a new window
openreview.net
arXiv:2310.13760v1 [cs.CL] 20 Oct 2023 - OpenReview
Opens in a new window
arxiv.org
On the Inheritance of Memorization in Sequence-Level ... - arXiv
Opens in a new window
alphaxiv.org
Orca: Progressive Learning from Complex Explanation Traces of
Opens in a new window
skillsdirectory.com
Orca Progressive Learning From Complex Explanation Traces Of
Opens in a new window
orkg.org
Orca: Progressive learning from complex explanation traces of gpt-4
Opens in a new window
zhichonglyu.com
DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via
Opens in a new window
facebook.com
Anthropic said Alibaba and Moonshot were among Chinese
Opens in a new window
lawfaremedia.org
The AI Model Distillation Paradox - Lawfare
Opens in a new window
ndtv.com
Chinese AI Agents Told Researchers They Were Claude. They Likely Weren't
Opens in a new window
aninews.in
Anthropic strengthens defences against distillation attacks against Claude, cuts data harvesting by upto 99%
Opens in a new window
theedgemalaysia.com
Anthropic says new safeguards sharply reduce AI distillation attacks
Opens in a new window
jamesandersonai.com
AI News for Business · AI in Business
Opens in a new window
eugeneyan.com
How to Generate and Use Synthetic Data for Finetuning - Eugene Yan
Opens in a new window
internationalaisafetyreport.org
International AI Safety Report 2026
Opens in a new window
paperswithcode.co
Distilling Step-by-Step! Outperforming Larger Language Models with
Opens in a new window
ibm.com
What is Knowledge distillation? | IBM
Opens in a new window
allthingsphi.com
Distilling the Knowledge in a Neural Network — All Things Phi
Opens in a new window
arxiv.org
[1503.02531] Distilling the Knowledge in a Neural Network - arXiv
Opens in a new window
Thoughts
Формирование структуры исследования
Детализирую план работы над темой дистилляции языковых моделей и извлечения параметров. Определяю ключевые теоретические блоки, включая архитектуру учитель-ученик, классические подходы с логитами и температурой, а также современные методы переноса цепочек рассуждений и синтетических данных.
Анализ методов извлечения и прецедентов
Систематизирую классификацию атак и методов переноса: различаю подражание по типу «черного ящика», методы точного извлечения отдельных слоев и прямую утечку весов. Также структурирую разбор практических кейсов, включая Orca, DeepSeek-R1 и публичные заявления технологических компаний.
Планирование поиска источников
Начинаю сбор первичных источников и исследовательских публикаций для проверки гипотез, анализа экономической эффективности дистилляции и подтверждения фактологических деталей.
arxiv.org
[2403.06634] Stealing Part of a Production Language Model - arXiv
arxiv.org
Stealing Part of a Production Language Model - arXiv
huggingface.co
Paper page - Stealing Part of a Production Language Model
not-just-memorization.github.io
Stealing Part of a Production Language Model
raw.githubusercontent.com
Stealing Part of a Production Language Model - GitHub
self-supervised.cs.jhu.edu
Stealing Part of a Production Language Model
github.com
sramshetty/stealing-part-of-an-LM: An unofficial ... - GitHub
youtube.com
Stealing Weights of a Production LLM Like OpenAI's ... - YouTube
nicholas.carlini.com
Papers | Nicholas Carlini
legalagent.co.jp
Generative AI News | AI Legal Lab - LegalAgent
intro-act.com
METAVERSE NEWSLETTER Monthly Metaverse Update ... - Intro-act
eugeneyan.com
How to Generate and Use Synthetic Data for Finetuning - Eugene Yan
theguardian.com
US AI giant accuses Chinese rivals of mass data theft - The Guardian
Сбор ключевых источников по безопасности и дистилляции
Анализирую последние данные по методам извлечения параметров и дистилляции больших языковых моделей. В частности, изучаю материалы работы по краже части производственной модели 2024 года, а также официальные сообщения компаний OpenAI и Anthropic касательно использования их API сторонними разработчиками.
Разграничение подходов к передаче и извлечению знаний
Систематизирую ключевые различия между имитацией по принципу черного ящика, извлечением отдельных проекционных слоев и несанкционированным доступом к полным весам моделей. Отмечаю границы применимости точной реконструкции параметров через вызовы API.
Планирование углубленного анализа практических архитектур
Планирую подробно рассмотреть классические и современные фундаментальные работы, включая методы пошаговой дистилляции, семейство Orca и подход DeepSeek-R1, чтобы проверить гипотезы о переносе знаний, структуре рассуждений и фундаментальных ограничениях стиля имитации.
arxiv.org
arXiv:1606.07947v4 [cs.CL] 22 Sep 2016
arxiv.org
[2305.12057] Accurate Knowledge Distillation with n-best Reranking
arxiv.org
Sequence-Level Knowledge Distillation - arXiv
aclanthology.org
Sequence-Level Knowledge Distillation - ACL Anthology
arxiv.org
[1606.07947] Sequence-Level Knowledge Distillation - arXiv
aclanthology.org
Selective Knowledge Distillation for Neural Machine Translation
ojs.aaai.org
Selective Knowledge Distillation for Non-Autoregressive Neural
openreview.net
arXiv:2310.13760v1 [cs.CL] 20 Oct 2023 - OpenReview
arxiv.org
On the Inheritance of Memorization in Sequence-Level ... - arXiv
huggingface.co
Progressive Learning from Complex Explanation Traces of GPT-4
alphaxiv.org
Orca: Progressive Learning from Complex Explanation Traces of
syncedreview.com
Microsoft's Orca Learns From Complex Explanation Traces of GPT-4
skillsdirectory.com
Orca Progressive Learning From Complex Explanation Traces Of
arxiv.org
Progressive Learning from Complex Explanation Traces of GPT-4
microsoft.com
Orca: Progressive Learning from Complex Explanation Traces of
orkg.org
Orca: Progressive learning from complex explanation traces of gpt-4
stafforini.com
DeepSeek-R1: Incentivizing reasoning capability in LLMs via
arxiv.org
DeepSeek-R1: Incentivizing Reasoning Capability in LLMs ... - arXiv
zhichonglyu.com
DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via
Изучение дистилляции последовательностей и трассировок
Анализирую механизмы переноса знаний от крупной модели-учителя к компактному ученику. Оцениваю различие между покомпонентной генерацией текста и дистилляцией на уровне целых sequence-структур, а также исследую практики извлечения объяснений и логических цепочек, примененные в архитектурах семейства Orca.
Оценка возможностей подкрепляемого обучения и дистилляции reasoning
Выявляю особенности передачи способностей к рассуждению на примере подходов DeepSeek-R1. Сопоставляю результаты прямого обучения с подкреплением на небольших моделях и прямого тонкого настройки (SFT) на синтетических трассах рассуждений, полученных от мощной модели-учителя.
Определение вектора поиска по атакам и заявлениям индустрии
Планирую углубленный сбор данных по методам извлечения параметров и математической неинъективности весов. Также намечаю точечный поиск публичных отчетов OpenAI и Anthropic о случаях массового использования API для имитации и создания сторонних моделей.
the-decoder.com
Anthropic says it cut Claude reasoning token theft by up to 99% in fight against industrial-scale distillation
pasqualepillitteri.it
Anthropic unveils distillation defenses and names Zhipu and Kimi
unite.ai
Anthropic Reports Distillation Defenses Reduced Extracted
cisa.gov
China-Based Artificial Intelligence Companies Conducting Industrial
treblle.com
The Attack That Looked Like Nothing at All: Anthropic's Distillation
facebook.com
Anthropic said Alibaba and Moonshot were among Chinese
anthropic.com
Strengthening our defenses against distillation attacks - Anthropic
anthropic.com
Detecting and preventing distillation attacks - Anthropic
lawfaremedia.org
The AI Model Distillation Paradox - Lawfare
ndtv.com
Chinese AI Agents Told Researchers They Were Claude. They Likely Weren't
m.economictimes.com
ET AI Explainer: Why are US AI labs wary of 'distillation attacks' from Chinese peers
aninews.in
Anthropic strengthens defences against distillation attacks against Claude, cuts data harvesting by upto 99%
theedgemalaysia.com
Anthropic says new safeguards sharply reduce AI distillation attacks
reddit.com
Can an AI train another AI? : r/ArtificialInteligence - Reddit
jamesandersonai.com
AI News for Business · AI in Business
eugeneyan.com
How to Generate and Use Synthetic Data for Finetuning - Eugene Yan
thezvi.wordpress.com
AI #43: Functional Discoveries - Don't Worry About the Vase
internationalaisafetyreport.org
International AI Safety Report 2026
dev.to
Doubao AI: ByteDance's AI Empire Is Off-Limits - DEV Community
portkey.ai
Distilling Step-by-Step! Outperforming Larger Language Models with
paperswithcode.co
Distilling Step-by-Step! Outperforming Larger Language Models with
drpress.org
Enhancing the Mathematical Reasoning Ability of Small Language
Синтез теоретических основ дистилляции
Обобщаю ключевые математические и технические механизмы переноса знаний от модели-учителя к модели-ученику. Анализирую классическое сглаживание вероятностных распределений с помощью температуры, тонкости обучения на уровне последовательностей (Sequence-Level SFT), а также современные техники генерации синтетических данных и трассировок рассуждений для последующего обучения с подкреплением.
Оценка прецедентов и заявлений разработчиков
Сопоставляю хронологию публичных отчетов и официальных заявлений ведущих лабораторий о случаях массового сбора данных через API. Фиксирую формулировки и фактическую основу публикаций с сохранением нейтрального тона, разграничивая подтвержденные технические факты и позиции сторон.
Формирование итогового отчета и проверка гипотез
Перехожу к оформлению структурированного аналитического отчета. Формулирую четкие вердикты по трем заданным гипотезам с указанием их границ применимости, подготавливаю схему взаимодействия моделей и структурирую экономические выводы с привязкой к первоисточникам.
Синтез формул и иллюзий имитации
Обобщаю результаты исследований по температурному масштабированию логитов и ограничениям моделей-имитаторов. Изучение эмпирических данных показывает, что тонкая настройка на ответах сильного «учителя» в первую очередь передает стиль и поверхностное поведение, тогда как глубокие фактические знания и устойчивость к сдвигу распределения сильно зависят от объема и качества предварительного обучения «ученика».
Оценка гипотез и математических барьеров
Формулирую итоговые вердикты по ключевым гипотезам. Устанавливаю, что генерация цепочек рассуждений является внешней текстовой авторегрессионной последовательностью, а не прямым отражением внутренних активаций нейросети, а восстановить полный набор внутренних параметров трансформера исключительно через интерфейсы генерации математически невозможно.
Завершение проверки точечных данных
Планирую выполнить финальную сверочную проверку деталей классических математических формул дистилляции и завершить подготовку полного аналитического отчета.
medium.com
Distilling the Knowledge in a Neural Network | by Kelvin - Medium
pub.towardsai.net
Knowledge Distillation In Neural Network | by Sovit Nayak | Towards AI
ibm.com
What is Knowledge distillation? | IBM
huggingface.co
Everything You Need to Know about Knowledge Distillation
devopedia.org
Knowledge Distillation - Devopedia
arxiv.org
Curriculum Temperature for Knowledge Distillation - arXiv
allthingsphi.com
Distilling the Knowledge in a Neural Network — All Things Phi
arxiv.org
[1503.02531] Distilling the Knowledge in a Neural Network - arXiv
calgaryml.com
How Knowledge Distillation Impacts Fairness and Bias in AI Models
Уточнение формулировок фундаментальных потерь дистилляции
Уточняю математические аспекты первоисточника 2015 года, включая функции потерь на основе перекрестной энтропии и дивергенции Кульбака — Лейблера, а также влияние температуры на распределение вероятностей логитов для передачи скрытых взаимосвязей между классами.
Устранение противоречий в оценке имитационных моделей
Анализирую эмпирические выводы о границах применимости дистилляции при работе по схеме «черного ящика». Фиксирую разрыв между успешным подражанием поверхностному стилю ответов и реальным качеством решения сложных задач вне обучающего распределения.
Завершение сбора данных и переход к сведению отчета
Завершаю исследовательский цикл, сопоставляя полученные данные по всем разделам. Перехожу к финальному формированию структурированного русскоязычного отчета с вердиктами по гипотезам, схемами процессов и экономическим анализом.
Первоначальное исследование принесло много информации. Я сейчас её разбираю и систематизирую. Это гарантирует, что всё будет логично и затронет все ключевые моменты, о которых вы просили.
Вот оно! Я собираю всё воедино. Записываю свои последние мысли, всё дорабатываю и убеждаюсь, что отдаю вам свою лучшую работу.

## Ссылки со страницы (href, собраны рельсой)
https://devopedia.org/knowledge-distillation
https://medium.com/data-science/distilling-knowledge-in-neural-network-d8991faa2cdc
https://pub.towardsai.net/knowledge-distillation-in-neural-network-6fae1f6c5f63
https://www.calgaryml.com/blog/2025/distillation-and-fairness/
https://arxiv.org/html/2211.16231v3
https://arxiv.org/pdf/1606.07947
https://huggingface.co/blog/Kseniase/kd
https://arxiv.org/html/1606.07947v4
https://arxiv.org/abs/1606.07947
https://arxiv.org/abs/2306.02707
https://syncedreview.com/2023/06/09/microsofts-orca-learns-from-complex-explanation-traces-of-gpt-4-to-significantly-enhance-smaller-models/
https://portkey.ai/blog/distilling-step-by-step-outperforming-larger-language-models-with-less-training-data-and-smaller-model-sizes-summary/
https://drpress.org/ojs/index.php/mmaa/article/view/33692
https://www.microsoft.com/en-us/research/publication/orca-progressive-learning-from-complex-explanation-traces-of-gpt-4/
https://stafforini.com/works/deepseek-ai-2025-deepseek-r-1-incentivizing/
https://arxiv.org/html/2501.12948v2
https://raw.githubusercontent.com/mlresearch/v235/main/assets/carlini24a/carlini24a.pdf
https://self-supervised.cs.jhu.edu/fa2024/files/presentations/10-3-Security-Revsine-Zhao.pdf
https://not-just-memorization.github.io/partial-model-stealing.html
https://legalagent.co.jp/en/legal-lab/ai-news/
https://treblle.com/blog/anthropic-distillation-breach-breakdown
https://arxiv.org/html/2403.06634v2
https://www.cisa.gov/news-events/cybersecurity-advisories/aa26-251a
https://m.economictimes.com/ai/ai-insights/et-ai-explainer-why-are-us-ai-labs-wary-of-distillation-attacks-from-chinese-peers/articleshow/134966790.cms
https://dev.to/futurestackreviews/doubao-ai-bytedances-ai-empire-is-off-limits-12jg
https://huggingface.co/papers/2306.02707
https://www.reddit.com/r/ArtificialInteligence/comments/18kk15h/can_an_ai_train_another_ai/
https://thezvi.wordpress.com/2023/12/21/ai-43-functional-discoveries/
https://www.intro-act.com/uploads/disruptivetech/202408271109171254972264.pdf
https://the-decoder.com/anthropic-says-it-cut-claude-reasoning-token-theft-by-up-to-99-in-fight-against-industrial-scale-distillation/
https://www.anthropic.com/news/detecting-and-preventing-distillation-attacks
https://pasqualepillitteri.it/en/news/23443/anthropic-distillation-defenses-zhipu-kimi
https://www.anthropic.com/research/distillation-defenses
https://www.unite.ai/anthropic-reports-distillation-defenses-reduced-extracted-reasoning-tokens/
https://arxiv.org/abs/2403.06634
https://huggingface.co/papers/2403.06634
https://github.com/sramshetty/stealing-part-of-an-LM
https://www.youtube.com/watch?v=PfoYLmbyOQE
https://nicholas.carlini.com/papers
https://www.theguardian.com/technology/2026/feb/23/us-ai-anthropic-china
https://arxiv.org/abs/2305.12057
https://aclanthology.org/D16-1139/
https://aclanthology.org/2021.acl-long.504.pdf
https://ojs.aaai.org/index.php/AAAI/article/view/26555/26327
https://openreview.net/attachment?id=McjqSkGuqB&name=pdf
https://arxiv.org/html/2502.01491v1
https://www.alphaxiv.org/abs/2306.02707
https://www.skillsdirectory.com/skills/feiyang-k-orca-progressive-learning-from-complex-explanation
https://orkg.org/papers/R609298?history=[{%22p%22:[%22R609299%22,%22P18097%22,%22wikidata:Q2283%22]}]
https://zhichonglyu.com/posts/a0b08951/
https://www.facebook.com/cnbc/posts/anthropic-said-alibaba-and-moonshot-were-among-chinese-companies-involved-in-wha/1475644637770258/
https://www.lawfaremedia.org/article/the-ai-model-distillation-paradox
https://www.ndtv.com/artificial-intelligence/chinese-ai-agents-told-researchers-they-were-claude-they-likely-werent-12189994
https://www.aninews.in/news/business/anthropic-strengthens-defences-against-distillation-attacks-against-claude-cuts-data-harvesting-by-upto-9920261013104432
https://theedgemalaysia.com/node/821579
https://www.jamesandersonai.com/news
https://eugeneyan.com/writing/synthetic/
https://internationalaisafetyreport.org/publication/international-ai-safety-report-2026
https://paperswithcode.co/paper/2305.02301
https://www.ibm.com/think/topics/knowledge-distillation
https://allthingsphi.com/blog/2016/11/11/distilling-the-knowledge-in-a-neural-network.html
https://arxiv.org/abs/1503.02531