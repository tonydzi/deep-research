# DR26-09-22-HUB-03-1433 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Карта интервью-лупов топ-лабораторий ИИ для ролей Research Scientist / Member of Technical Staff / Research Engineer (2025–2026)

Отчёт для Anton Dziatkovskii (Tony Dzi), Palo Alto AI Research Lab. Дата: 22 сентября 2026.

## 1. Краткое резюме

- **Единого «лупа» не существует — рынок раскололся на два лагеря по коду и ИИ-ассистентам.** «Классические» лаборатории (OpenAI, DeepMind, Apple, Amazon, Microsoft, Nvidia) сохраняют LeetCode-подобный алгокодинг и запрещают ИИ на живых сессиях; Anthropic официально перешёл на AI-collaborative формат (можно Claude/Cursor/Copilot на кодинг-раунде), Scale AI официально отказался от LeetCode, а Meta и Google в 2025–2026 сами запустили ИИ-ассистированные кодинг-раунды (см. §7). Программу подготовки надо строить в две ветки: «код без ИИ + ML-кодинг с нуля» и «работа с ИИ-агентом под наблюдением».
- **Технический бар и ML-fundamentals важнее публикаций.** По свидетельству Alisa Liu (11 компаний, 57 интервью, июнь 2026): «technical skills and knowledge are evaluated much more than research experience». Самый частый формат — ML-кодинг (реализация attention, декодинга, тренировочного цикла в PyTorch). Для кандидата без PhD реально открыты роли Research Engineer / MTS и программы типа OpenAI Residency (нет формальных требований к образованию, но «технический бар на уровне full-time сотрудника»).
- **Рекомендуемый порядок прохождения компаний:** сначала «тренировочные» луупы средней жёсткости (Cohere, Scale AI, Perplexity, Mistral), затем целевые фронтир-лаборатории (Anthropic, OpenAI, DeepMind), а Meta/Amazon/Apple/Microsoft/Nvidia — как параллельный «большой tech»-трек с предсказуемым форматом. SSI / Thinking Machines / Reflection AI — точечно: данных по процессу почти нет.

## 2. Сводная таблица по компаниям

| Компания | Роли | Последовательность этапов | Длительность этапов / всего | Ключевые источники |
|---|---|---|---|---|
| **OpenAI** | RS, MTS, RE | Recruiter (30 мин) → тех. телефонный скрин (1 ч, CoderPad) → 2-й скрин/ассессмент (take-home / HackerRank / pair-coding) → онсайт 4–6 ч (coding, system design, презентация, 2 behavioral) | Скрины 1 ч; онсайт 4–6 ч; весь процесс ~6–8 недель (по одному отчёту — до 4 месяцев) | interviewing.io, igotanoffer, Exponent |
| **Anthropic** | RS, RE, MTS/ML Eng | Recruiter (30 мин, отсеивающий) → online coding (CodeSignal 90 мин take-home ИЛИ live 60 мин) → hiring-manager → онсайт (2 coding / ML-systems, research presentation или take-home) → values/culture round | 4–6 недель (RE), дольше для research; take-home окно ~48 ч | anthropic.com, interviewing.io, Glassdoor, techinterview.org |
| **Google DeepMind** | RS, RE | Recruiter (30 мин) → 2 gated coding-скрина (CoderPad) → 2 ML-раунда (ML depth + ML system design/math) → онсайт (TL-интервью + behavioral) → hiring/team-match → offer | Раунды ~1 ч; всего ~6–7 недель (avg 45 дней по Glassdoor); 7–9 раундов | Blind (firsthand), Glassdoor, deepmind.google |
| **Meta (MSL / FAIR)** | RS (PhD), RE | Recruiter (LinkedIn) → phone screen (1 coding) → full loop (2 coding, 1 systems design, 1 behavioral; для RS + research presentation / ML fundamentals) → hiring committee → offer | Раунды 45 мин; весь процесс ~2 месяца (firsthand Aidan Lakshman) | ahl27.com (firsthand), Glassdoor, interviewquery |
| **xAI** | MTS, RE | CV + «Statement of Exceptional Work» → 15-мин intro call → 4 тех. интервью: 3 coding/eng discussion + «Meet the Team» (презентация) | Цель — закрыть основной процесс за 1 неделю | greenhouse.io/xai (official), Exponent, Glassdoor |
| **Mistral AI** | RS, RE, SWE | Recruiter → тех. quiz/assessment → live coding + system design + LLM-квиз (45–60 мин) → take-home (пишется как мини-статья) → culture round | 4–5 стадий, ~2–4 недели | dataford, techinterview.org, jobsbyculture, Glassdoor |
| **Cohere** | RS, MTS, ML Eng | Recruiter/HM screen → coding screen → тех. интервью (ML/задача) → research talk (для research) → system design → «coding with AI assistant» → senior mgmt | Несколько недель | Glassdoor (firsthand, Aug 2026), Taro, dataford |
| **Scale AI (SEAL)** | RS, RE | Recruiter → HM call → тех. скрин (OOP / PyTorch) → онсайт: ML deep dive, paper-reading, ML-fundamentals, behavioral | ~7+ ч онсайт; медиана ~14 дней; **без LeetCode** | scale.com (official), github/AI-Engineer-Questions, oavoservice |
| **Perplexity** | RE, AI Eng, SWE | Phone screen → тех. ассессмент (стандартный programming) → онсайт 4–5 интервью (+ deep dive с HM) → финальное интервью с founder/лидером → team match | Решение в течение недели после онсайта; ~13 дней в среднем | perplexity.ai (official), Glassdoor, finalroundai |
| **Nvidia** | RS, DL Eng | Recruiter (30 мин) → тех. скрин (coding + ML + обсуждение статей) → take-home/тех. задача → онсайт: job talk (~1 ч) + 3–5 раундов (coding, ML, behavioral) | Job talk — центр процесса | interviewquery, dataford, datainterview, Glassdoor |
| **Apple (AIML)** | ML Eng, ML Research, Residency | Recruiter → 1–2 тех. phone screen → онсайт 5–8 раундов (coding, classical ML, DL, probability/stats, ML system design, behavioral) | 4–6 недель; наименее стандартизированный луп в big tech | Exponent, datainterview, Blind, linkjob |
| **Amazon (AGI/AWS AI)** | Applied Scientist, RS | Recruiter → 1–2 тех. phone screen (coding + ML breadth + behavioral) → онсайт 4–6 раундов: ML Breadth, ML Depth, Live Coding, Science Application/System Design, Bar Raiser | ~3–6 недель; avg 25 дней (Glassdoor) | amazon.jobs (official), Glassdoor (firsthand Apr 2025), dataford |
| **Microsoft Research** | Researcher, Applied Scientist | Recruiter → phone screen (research background) → онсайт: 1-ч job talk/лекция + 4+ интервью (coding LC medium, ML theory, research presentation, «As-Appropriate» раунд) | ~4–8 недель; интерн — 3 интервью | microsoft.com/research (official), dataford, Glassdoor |
| **Meta FAIR** | RS, RE | FAIR теперь внутри Meta Superintelligence Labs (MSL); для pure-research треков — сильнее упор на публикации, research presentation | ~2 месяца | wikipedia (MSL), interviewquery, cleverprep |
| **Thinking Machines Lab** | RS, RE, MTS | Video screen → тех. раунд с team leads (иногда project / paid work trial) → финальные беседы с founders | ~19 дней в среднем (Glassdoor, 17 отзывов); ~3 недели | Glassdoor (firsthand), techinterview.org, designgurus |
| **SSI (Safe Superintelligence)** | RS, RE | Не публикует процесс; по реконструкции — recruiter → тех. скрин → research-разговор про «вкус» и постановку задач | «не нашёл» точных длительностей | techinterview.org (реконструкция), presenc.ai |
| **Reflection AI** | MTS (pre/post-training), RE, SWE | **«не нашёл»** — публичных firsthand-описаний лупа нет; роли MTS подтверждены на Ashby | «не нашёл» | Ashby (official roles), Blind (только вопросы), wikipedia |

## 3. Форматы этапов с примерами реальных вопросов

| Формат | У каких компаний встречается | Примеры реальных вопросов | Источник |
|---|---|---|---|
| **ML-кодинг (с нуля, PyTorch)** | OpenAI, Anthropic, DeepMind, Mistral, Scale, Nvidia, Cohere | Реализовать attention/multi-head attention с нуля; декодинг (top-k/top-p/beam); тренировочный цикл; backward pass на numpy; кастомные loss/слои | Alisa Liu blog; Anthropic guide (linkjob); Scale (dataford) |
| **Алгокодинг / LeetCode** | OpenAI (практичнее LC), Meta (LC medium, 2 задачи/45 мин), DeepMind (LC medium+hard), Amazon, Apple, Nvidia | topological sort, merge sorted arrays, «Minimum Remove to Make Valid Parentheses», rotated sorted array, pow(x,n) | ahl27.com (firsthand Meta); Blind (DeepMind) |
| **Technical breadth / rapid-fire** | DeepMind, Mistral, OpenAI | «What is 5D parallelism? Разница PPO vs GRPO? Способы кодирования позиционной информации?» | Alisa Liu blog |
| **Research discussion (свои работы)** | Все research-роли (DeepMind, Meta, Nvidia, MSR, Anthropic, Cohere) | «Расскажите о прошлом проекте», follow-up по статьям из CV, дизайн экспериментов | Alisa Liu; MSR (Danyel Fisher blog) |
| **Job talk / презентация** | Nvidia (~1 ч), MSR (1 ч лекция), xAI («Meet the Team»), OpenAI (презентация проекта) | Доклад по одной работе/направлению; панель senior-scientists допрашивает по опубликованной работе | datainterview (Nvidia); interviewing.io (OpenAI); microsoft.com |
| **Математика / деривации** | DeepMind (Bayes, L1/L2 gradient sparsity), Mistral (math intuition), Apple (probability), Amazon | «Статистические импликации distance-метрики в высокой размерности»; probability, linear algebra, calculus | Blind (DeepMind); Glassdoor (Mistral) |
| **ML system design** | Amazon, Apple, Mistral, Perplexity, Nvidia, DeepMind | «Design conversational agent over knowledge base» (RAG); inference at scale (KV-cache, quantization, batching) | interviewcoder (Mistral); dataford |
| **Поведенческое / values** | Все; особо Anthropic (values round — главный фильтр), Amazon (Leadership Principles / Bar Raiser), Scale (ownership) | «Difficult ethical decision»; «tradeoffs long-term vs short-term»; STAR-истории | igotanoffer (Anthropic); amazon.jobs |

## 4. Различия между компаниями

| Компания | LeetCode | Job talk | Take-home | Work trial | ИИ-ассистент на кодинге |
|---|---|---|---|---|---|
| OpenAI | Да (практичнее LC) | Презентация проекта | Иногда (HackerRank/take-home) | Нет | **Запрещён** (interviewing.io: «strictly prohibited») |
| Anthropic | Скорее нет (spec-based, refactoring) | Research presentation (research-роли) | Да (CodeSignal 90 мин) | Нет | **Разрешён** на take-home/coding, если указано; на live — нет |
| DeepMind | **Да** (LC medium+hard) | Research discussion (RS) | Нет (firsthand) | Нет | «не нашёл» firsthand; аггрегаторы говорят «запрет» (не подтверждено) |
| Meta | **Да** (LC medium, 2/45 мин) | Research presentation (RS) | Нет | Нет | **Смешанно:** firsthand фев 2025 — запрет («no AI code generation»); с окт 2025 Meta ввела отдельный ИИ-ассистированный кодинг-раунд (см. §7) |
| xAI | Coding, но «ship fast with AI» | «Meet the Team» презентация | Да (4-ч take-home, product-like) | Отчасти (systems hands-on) | Скорее разрешён (take-home «shipping with AI») |
| Mistral | Да (medium-hard Python/Rust) | Нет (culture round) | **Да** (как мини-статья) | Нет | Калиброванно (boilerplate можно) |
| Cohere | Да (не самый сложный) | Research talk | Возможен (OA 3 задачи) | Нет | **Да** — отдельный раунд «coding with AI assistant» (Glassdoor) |
| Scale AI | **Нет** («We will not ask any LeetCode-style questions») | Paper-reading round | Да (окно 5 дней, реально 4–8 ч) | Нет | «не нашёл» |
| Perplexity | Да (standard programming) | Deep dive + founder round | Иногда (CodeSignal/Colab) | Resident Program | Требуется знание ИИ-инструментов |
| Nvidia | Да | **Да (job talk ~1 ч)** | Да (тех. задача) | Нет | «не нашёл» |
| Apple | Да (team-dependent) | Нет (deep dive проектов) | Иногда | Нет | «не нашёл» |
| Amazon | **Да** (Live Coding раунд) | Нет (Science Application) | Нет | Нет | «не нашёл» |
| Microsoft Research | Да (LC medium) | **Да (1-ч лекция)** | Нет | Нет | «не нашёл» |
| Thinking Machines | «не нашёл» | Past-work deep dive | Иногда project | **Да (paid work trial по отчётам)** | «не нашёл» |
| SSI | «не нашёл» | Research-разговор | «не нашёл» | «не нашёл» | «не нашёл» |
| Reflection AI | «не нашёл» | «не нашёл» | «не нашёл» | «не нашёл» | «не нашёл» |

## 5. Критерии оценки и типичные ошибки

**Сигналы сильного найма:**
- **Непрерывная коммуникация рассуждений.** interviewing.io про инженера OpenAI: «he continuously communicated his technical reasoning as he worked» — прояснял ограничения, сравнивал tradeoffs.
- **Чистый, читаемый код + тестирование по шагам** (Meta firsthand: сначала прояснить задачу, описать решение без кода, потом писать, потом тестировать вручную).
- **Глубина по своим работам:** понимать «почему» и «как», а не только результаты.
- **Mission/values alignment** (Anthropic, Scale, OpenAI): у Anthropic values round — отдельный, ведут non-technical интервьюеры, и, по данным Preper (обновл. 24.06.2026), большинство кандидатов его проваливает; кандидаты описывают его как «feeling closer to a therapy session than a job interview, deeply personal, probing, and conversational».
- **Способность быстро «шипить» с ИИ** (xAI): «whether you can ship fast with AI and read unfamiliar production-style code under pressure».

**Что заваливает чаще всего:**
- Провал behavioral из-за отсутствия заготовленных историй (Alisa Liu: «I failed my first behavioral interview»).
- Недооценка coding для research-ролей (Glassdoor Meta: «wasn't expecting a coding interview for a Ph.D.-level Research Scientist»).
- Слабая математика/статистика (OpenAI stats-round «graduate-level information theory»; Mistral: «Didn't manage to answer the math questions»).
- Провал values-раунда у Anthropic; отсутствие «urgency/ownership» у Scale.
- Зависимость от ИИ-автодополнения там, где оно запрещено (Alisa Liu: тренироваться «with AI assistance completely off»).

## 6. Кандидат без PhD и без публикаций

**Признаваемые альтернативные сигналы:** сильные инженерные основы, OSS-вклады, собственные системы в проде, self-taught hacker / early-stage founder track record, математический фундамент (linear algebra, stats, probability).

| Компания | Реально открыто без PhD | Признаваемые сигналы |
|---|---|---|
| OpenAI | **Да** — Residency (нет формальных требований к образованию), MTS/RE | Strong programming, math foundation, независимые прототипы, math olympiad, physics/neuroscience |
| Anthropic | Да — RE/MTS (совет: «Anyone with an engineering background is told to apply as an engineer») | Инженерный опыт, fluency с ИИ-инструментами |
| DeepMind | RE (не требует PhD; «software engineers with deep understanding of ML») | Сильный код + ML depth |
| Meta | RS (PhD)-роли часто = mid-level SWE; RE без PhD | Инженерный опыт, LeetCode |
| xAI | **Да** — MTS не требует PhD | «Statement of Exceptional Work», умение шипить с ИИ |
| Perplexity | **Да** — «Don't worry about prestige, credentials, or titles»; Resident Program | Ownership, доказательства вклада в проекты |
| Scale AI | Да — FDE/Operations без RLHF-глубины; research требует SFT/DPO/PPO | Product sense, реплицировать ≥1 статью |
| Nvidia/Apple/Amazon/MSR | Applied/Eng-роли — да; pure research обычно требует PhD | Prod-опыт, C++/Python, distributed training |
| Thinking Machines | «hires for spikes, not checklists» | Глубокая экспертиза в одной области |

**OpenAI Residency подробно (главный вход без PhD):** по словам программ-менеджера Jackie Hehir (OpenAI Forum info session) и официальному постингу — «The Residency Program does not have any formal education or professional training requirements... but we do hold an extremely high technical bar. And honestly, the technical bar for a resident is at parity with a full-time member of staff». Программа шестимесячная, оплата $18,333/месяц. OpenAI ценит «trajectory over any particular archetype» и «early builders who are already doing meaningful work».

## 7. Изменения 2025 → 2026

- **Политика ИИ-ассистентов раскололась.** Anthropic (официальная страница «How to collaborate with Claude during our hiring process», обновл. 10.07.2025) разрешил Claude для подготовки и refine материалов: «Please create your first draft yourself, then use Claude to refine it»; но для take-home — «Complete these without Claude unless we indicate otherwise», для live — «no AI assistance unless we indicate otherwise». Fortune (21.07.2025) отмечает, что это развернуло прежний полный запрет ИИ в заявках. На AI-collaborative кодинг-раунде можно любой инструмент (Claude/Cursor/Copilot), оценивают prompt clarity, verification, decomposition.
- **Anthropic переделывает take-home под каждую модель Claude.** TechCrunch (22.01.2026), цитируя блог тимлида perf-команды Tristan Hume: «Each new Claude model has forced us to redesign the test... When given the same time limit, Claude Opus 4 outperformed most human applicants. That still allowed us to distinguish the strongest candidates — but then, Claude Opus 4.5 matched even those». TechCrunch затем уточнил, что на этом take-home использование ИИ прямо разрешено.
- **Meta с октября 2025 ввела ИИ-ассистированный кодинг-раунд** (60-мин CoderPad, выбор GPT/Claude/Gemini/Llama), заменив один из двух кодинг-раундов — это противоречит firsthand-отчёту фев 2025 (Aidan Lakshman: «no AI code generation»). **Расхождение по датам: до окт 2025 — запрет, после — отдельный ИИ-раунд.**
- **Google пилотирует Gemini-ассистированный «code comprehension» раунд** (объявлено VP-рекрутинга Brian Ong, начало мая 2026) для junior/mid SWE-ролей на отдельных US-командах (сначала Google Cloud и Platforms & Devices), раскатка во 2-й половине 2026; оценивают «AI fluency, including prompt engineering, output validation, and debugging skills».
- **Agentic / code-comprehension раунды по индустрии 2026:** чтение/дебаг незнакомого репозитория с ИИ-агентом (HackerRank ADE, Plan/Agent modes).
- **Живые сессии вместо take-home.** xAI сжал процесс до ~1 недели (4-ч take-home + 1 live).
- **OpenAI сохраняет запрет ИИ** на интервью (interviewing.io: «strictly prohibited»).

## 8. Разбор блога Alisa Liu и аналогичных публичных разборов

**Alisa Liu (alisawuffles.github.io/blog/job-search, 20 июня 2026)** — 6-летний PhD по NLP (UW), подавалась на RS/MTS, 11 компаний, 57 интервью + 46 recruiter-звонков + 16 post-offer. Ключевое:
- Категории интервью: ML coding (самый частый), general coding (LeetCode), technical discussion (два подтипа: extended experiment-design и rapid-fire breadth), research discussion, behavioral, math, job talk.
- «Being fluent in PyTorch is a must»; иногда только numpy (backward pass).
- Job talk короче академического, фокус на одной работе (у неё — токенизация).
- Подготовка: Stanford CS336 (Language Modeling from Scratch), HW1 (реализация/дебаг трансформера — «turn it into muscle memory»), LeetCode 75 / Neetcode Blind 75, свои LLM-notes и math-notes.
- Совет: тренироваться с полностью выключенным ИИ; «each interview is a slightly different math or CS class… ~3 days to cram».
- Про сон: провалила technical на 2 часах сна, 10 минут на off-by-one.

**Nathan Lambert (natolambert.com/writing/ai-phd-job-hunt)** — RL/RS job hunt (Berkeley PhD): найм в топ-лабах сильно завязан на нетворкинг («who you know»), интернатуры и публичную видимость.

**Aidan Lakshman (ahl27.com, фев 2025)** — Meta RS (Systems & Infra), фактически SWE-луп: recruiter → phone screen (1 coding) → full loop (3 coding + system design + behavioral); каждое интервью 45 мин, строгий тайминг, 2 задачи/интервью; **нет исполнения кода, нет ИИ** (на момент фев 2025); процесс ~2 месяца. Behavioral для RS ведёт человек с PhD.

**Danyel Fisher (Medium)** — интервьюер MSR: ожидают «what do you want to be famous for in a decade», траектория должна быть согласованной.

## 9. Приоритизированный план подготовки

**Приоритет 1 — ML-кодинг с нуля (самый частый формат, максимальный ROI):**
1. Реализация трансформера/attention (Stanford CS336 HW1) до автоматизма.
2. Декодинг: greedy, top-k, top-p (nucleus), beam search, temperature.
3. Тренировочный цикл в PyTorch: forward/backward, optimizer, loss.
4. Backward pass вручную (numpy), backprop по формулам.
5. KV-cache, токенизатор, sampling.
6. Распараллеливание: data/tensor/pipeline parallelism, 5D parallelism.

**Приоритет 2 — алгокодинг без ИИ:** Neetcode Blind 75 / LeetCode 75; тренироваться с выключенным автодополнением; проговаривать вслух; тестировать по шагам. Критично для DeepMind, Amazon, Apple, OpenAI (и Meta для «AI-off» части).

**Приоритет 3 — ML breadth / rapid-fire:** позиционные кодировки, PPO vs GRPO/DPO/RLHF, нормализации, MoE, inference-оптимизации (quantization, batching), scaling laws.

**Приоритет 4 — математика:** probability, linear algebra (SVD, eigenvalues), calculus, Bayes, concentration inequalities.

**Приоритет 5 — research pitch & job talk:** отрепетировать доклад по одной first-author работе; понимать «почему»; заготовить future directions.

**Приоритет 6 — ML system design:** RAG-пайплайн, inference at scale, KV-cache, latency/cost budgets, guardrails.

**Приоритет 7 — behavioral / values:** заготовить STAR-истории; для Anthropic — глубокие этические примеры и рефлексия; для Amazon — Leadership Principles; для Scale — ownership/urgency.

**Приоритет 8 — AI-collaborative практика:** для Anthropic/Cohere/xAI/Meta(new)/Google(pilot) — тренировать работу с Claude/Cursor: prompt clarity, verification, decomposition, review каждого diff.

**Порядок компаний:** (1) практика: Cohere, Scale AI, Perplexity, Mistral; (2) целевые фронтир: Anthropic, OpenAI, DeepMind; (3) параллельно big tech: Meta, Amazon, Apple, Microsoft, Nvidia; (4) точечно/по интересу: xAI, Thinking Machines, SSI, Reflection AI (данных по процессу мало). Бенчмарки для смены плана: если ML-кодинг «с нуля» ещё не автоматизирован — не начинать целевые фронтир-луупы; если провален ≥1 behavioral — доработать STAR-истории до Anthropic values round.

## 10. Список источников

1. alisawuffles.github.io/blog/job-search — firsthand blog, 20.06.2026 — «technical skills and knowledge are evaluated much more than research experience».
2. natolambert.com/writing/ai-phd-job-hunt — firsthand blog, 2022 — RS job hunt, нетворкинг.
3. ahl27.com/posts/2025/02/meta-interview — firsthand blog, 17.02.2025 — «two Coding, one Systems Design, one Behavioral».
4. ahl27.com/posts/2025/03/meta-rs-types — firsthand blog, 19.03.2025 — типы RS-ролей в Meta.
5. interviewing.io/openai-interview-questions — aggregator (инсайдеры), 2025/2026 — «AI use in OpenAI interviews is strictly prohibited».
6. igotanoffer.com/en/advice/openai-research-scientist-interview — aggregator — «two to four weeks from application to offer».
7. tryexponent.com/experiences/openai-machine-learning-engineer-interview — firsthand experience — stats-heavy round.
8. anthropic.com/candidate-ai-guidance — official, 10.07.2025 — «create your first draft yourself, then use Claude to refine it».
9. interviewing.io/anthropic-interview-questions — aggregator — CodeSignal 90-мин take-home / 60-мин live.
10. glassdoor.com Anthropic Research Engineer — community, Apr 2026 — «Not leetcode but more about writing code fast to a spec».
11. techcrunch.com/2026/01/22 (Anthropic revising test) — news, 22.01.2026 — «Claude Opus 4.5 matched even those».
12. techinterview.org Anthropic AI-collaborative — aggregator, 2026 — 4-dimension rubric.
13. deepmind.google/careers — official — 4-стадийный фрейм.
14. teamblind.com DeepMind RE (wp1gjgyb) — firsthand community — «In total the process took about 6-7 weeks».
15. glassdoor.com DeepMind Research Scientist — community — «two research and two coding interviews» (May 2026).
16. glassdoor.com Meta Research Scientist — community, Dec 2024 — «1 coding interview, 2 technical discussions, 1 ML fundamentals, behavioural and a research presentation».
17. greenhouse.io/xai (jobs) — official — «four technical interviews… Meet the Team».
18. tryexponent.com xAI MTS — firsthand, 2026 — «4 hour take-home… ship fast with AI».
19. scale.com/careers SEAL RS/RE — official — «We will not ask any LeetCode-style questions».
20. perplexity.ai/hub/careers/interview-guide — official — «Don't worry about prestige, credentials, or titles».
21. glassdoor.com Perplexity — community, 2026 — «2 coding rounds plus 4.5 hr onsite».
22. dataford.io Mistral Research Engineer — aggregator — стадии лупа.
23. glassdoor.com Mistral — community — «one round coding, one ML/AI QA, one math intuition».
24. interviewquery.com Nvidia Research Scientist — aggregator — job talk + coding.
25. datainterview.com Nvidia AI Researcher — aggregator — «1-hour job talk… panel interrogates your published work».
26. amazon.jobs applied-scientist-interview-prep — official — «four 55-minute interviews».
27. glassdoor.com Amazon Applied Scientist — community, Apr 2025 — Transformer/KV-cache вопросы.
28. microsoft.com/research COLM 2025 recruiting — official — «one-hour interview talk or lecture and interviews with 4 or more people».
29. medium.com Danyel Fisher (MSR) — firsthand — «what do you want to be famous for in a decade».
30. tryexponent.com Apple MLE guide — aggregator — 5–8 раундов, наименее стандартизированный.
31. glassdoor.com Thinking Machines — community, 2026 — «video screening followed by a technical round with the team leads».
32. techinterview.org SSI — reconstruction — research-разговор про «вкус».
33. openai.com/residency + forum.openai.com Residency info (Jackie Hehir) — official — «no formal education or professional training requirements… technical bar… at parity with a full-time member of staff»; $18,333/месяц.
34. customcareer.miami.edu (Exponent, Google AI-assisted coding 2026) — aggregator, May 2026 — Gemini «code comprehension» pilot; VP Brian Ong.
35. fortune.com/2025/07/21 (Anthropic AI hiring policy change) — news, 21.07.2025 — реверс запрета ИИ в заявках.
36. preper.app Anthropic behavioral guide — aggregator, 24.06.2026 — values round «that most candidates fail… closer to a therapy session».
37. en.wikipedia.org/wiki/Reflection_AI — reference — основатели Laskin, Antonoglou; interview loop не найден.

## 11. Proof of work

**Выполненные поисковые запросы (web_search):**
1. OpenAI research engineer interview process 2025
2. Anthropic research engineer interview take-home 2025
3. Google DeepMind research scientist interview process job talk 2025
4. xAI member of technical staff interview experience reddit
5. Meta FAIR research scientist interview loop 2025
6. Mistral AI interview process research engineer
7. Cohere machine learning interview process experience
8. Perplexity AI engineer interview process 2025
9. Scale AI SEAL research engineer interview process
10. Nvidia research scientist interview process deep learning
11. Apple machine learning research interview loop AIML experience
12. Amazon applied scientist interview loop 2025 PhD
13. Thinking Machines Lab SSI Reflection AI interview process
14. Anthropic interview candidate AI policy application Claude allowed
15. Microsoft Research interview process researcher job talk 2025
16. AI lab coding interview AI assistant allowed 2026 agentic task
17. OpenAI residency member technical staff no PhD requirements alternative signals
18. Nathan Lambert AI PhD job hunt research scientist blog
19. DeepMind research engineer interview experience reddit rounds coding (бюджет исчерпан)
20. Reflection AI interview process member technical staff (бюджет исчерпан)

**Реально открытые страницы (web_fetch):**
- https://alisawuffles.github.io/blog/job-search
- https://www.perplexity.ai/hub/careers/interview-guide
- https://www.ahl27.com/posts/2025/02/meta-interview/
- https://interviewing.io/openai-interview-questions

**Субагент** (DeepMind + Reflection AI) — реально открытые страницы: teamblind.com (DeepMind RE wp1gjgyb; DeepMind Interview 73684), glassdoor.com (DeepMind RS/RE), deepmind.google/careers, en.wikipedia.org/wiki/Reflection_AI, techcrunch.com (Reflection $2B), jobs.ashbyhq.com/reflectionai, teamblind.com (Reflection threads wkudku14, ud3sutck), techinterview.org / norahq.com / fdeinterviews.com (Reflection guides), igotanoffer.com (DeepMind RE), rajeevnaruka.com (DeepMind 2021 internship).

**Примечания по честности данных:** где данных не было — отмечено «не нашёл» (Reflection AI process, SSI длительности, ИИ-политика ряда big-tech). Расхождения показаны явно (Meta ИИ-политика до/после окт 2025; DeepMind «AI-ban 2026» — только аггрегаторы, firsthand не подтверждает). Firsthand-свидетельства (Alisa Liu, Aidan Lakshman, Blind DeepMind, Glassdoor с датами) отделены от prep-site аггрегаторов (dataford, techinterview.org, igotanoffer, datainterview), которые помечены как secondary.