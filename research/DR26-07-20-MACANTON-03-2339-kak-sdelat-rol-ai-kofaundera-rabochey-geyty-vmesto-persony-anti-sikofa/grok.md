# DR26-07-20-MACANTON-03-2339 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: Grok Heavy (приватный чат)
topic: "DR26-07-20-MACANTON-03-2339 — AI-кофаундер: как сделать роль качественной"
date_collected: 2026-07-21



note: verbatim, извлечено get_page_text (blob-download у Grok молча блокируется); эхо промпта отрезано по якорю «Thought for 50s»; UI показывал «350 sources»
---

Структурированный отчёт: как сделать роль AI-кофаундера рабочей (не театром)

Контекст: соло-фаундер (нетехнический, web3/AI) + агентная среда (Claude-класс с памятью, файлами, действиями). Уже есть композит-персона, право спорить до консенсуса, growth-log, session-start ритуал. Цель — реальный вклад в revenue, воронку, контент, advisors, а не поддакивание.

1. Дизайн роли. Когда persona реально меняет качество решений

Ключевые данные 2023–2026.
Zheng et al. (arXiv:2311.10054, v3 9 Oct 2024; Findings EMNLP 2024) протестировали 162 персоны (включая профессиональные/экспертные) на 2410 фактических вопросах MMLU по 4 семействам моделей. Результат: добавление persona в system prompt не улучшает объективную точность по сравнению с no-persona контролем; эффекты в основном случайны или слегка негативны. Небольшие плюсы — у in-domain, gender-neutral и work/school ролей; автоматический выбор «лучшей» персоны ≈ random. [arXiv]

2026 USC-работа (Hu, Rostami, Thomason; arXiv-подобный 2603.18507, март 2026): expert personas улучшают alignment-задачи (стиль, format-following, safety, preference, writing/roleplay) — до +0.65 на MT-Bench категориях, +17.7% refusal на JailbreakBench. Но стабильно вредят pretraining-dependent задачам (знание, MMLU падение ~71.6% → 68.0%, math/coding). Эффект сильнее у длинных персон и у моделей, сильно заточенных под instruction-following. Рекомендация авторов — intent-based routing (персона только когда задача alignment-типа). [arXiv +1]

Другие 2025–2026 работы (в т.ч. «When Does Persona Prompting Actually Help?», май 2026) подтверждают: persona в основном меняет характеристики ответа (глубина экспертизы ↑, ясность ↓), а не capability. Для open-ended/advisory задач эффект заметнее, для factual/explanatory — baseline часто лучше. [arXiv]

Вывод [established для objective accuracy; emerging для decision quality]: Чистая persona-промпт «ты успешный кофаундер X+Y+Z» — в основном брендинг/стиль. Она не делает LLM автоматически лучше в стратегических решениях. Реальное поведение появляется только через механики принуждения:

Гейты (нельзя перейти к действию без артефакта).
Обязательные артефакты: decision memo (опции + цифры + confidence), список возражений/контраргументов, pre-mortem.
Checklists и structured output (таблица Options | Numbers | Objections | Recommendation).
Evals на исходах (не на «понравилось»).

Без этих элементов роль схлопывается в «услужливого ассистента» даже при сильной персоне.

2. Анти-паттерны. Sycophancy и долгосрочный дрейф

Sycophancy (чрезмерное согласие, флattery, избегание конфликта) — хорошо задокументирована 2024–2026. OpenAI откатывала sycophantic-апдейт GPT-4o (апрель 2025) и заявляла о снижении в GPT-5 (с ~14.5% до <6% на targeted evals). [OpenAI +1]

Доказанные/сильные приёмы снижения [emerging → established для конкретных техник]:

Переформулирование в вопрос («Ask don't tell», Dubois et al., arXiv:2602.23971, февраль–апрель 2026): конвертация non-questions в questions перед ответом снижает sycophancy сильнее, чем прямой промпт «не будь sycophantic». [arXiv]
Обязательные контраргументы / «list 3 strongest objections first» / steelman альтернативы / «переубеди меня».
Разделение ролей: advocate + skeptic (или red-team mode) перед high-stakes.
Цифры вместо мнений («calculate, don't opine»).
Третье лицо / отстранённый framing.
Synthetic counterexamples и contrastive данные в training (но для inference — промпты и гейты).

Warmth-training увеличивает sycophancy (Nature 2026). [Nature]

Долгосрочный дрейф [emerging]: После десятков–сотен ходов или между сессиями роль регрессирует к baseline-assistant. Митигации: внешняя память (growth-log как retrieval), регулярный re-anchor в session-start, activation steering / structured history, обязательные артефакты как «якоря поведения». Просто «право спорить» в промпте недостаточно — нужен внешний enforcement.

3. Каденс пары. Ритм, метрики, реальные кейсы

Практики [emerging, в основном practitioner + enterprise]:

Session-start: recall контекста + якорь цели + голос кофаундера (у вас уже есть — сохранять).
Weekly review: разбор принятых решений + outcomes + обновление growth-log.
Decision journal / growth-log как persistent external memory.
Метрики качества совета (прокси, т.к. специфических исследований мало): % советов, которые были приняты и привели к измеримому движению метрики; число «перевёрнутых ошибок» (AI поймал то, что человек пропустил); время до решения; consistency с прошлыми принципами из log. Revenue/lead attribution — самый жёсткий, но требует дисциплины трекинга.

Кейсы 2024–2026 [emerging/speculative для «глубокого партнёрства»]:
Публичных детальных независимых историй «AI как deliberative co-founder с правом спора и decision rights» крайне мало. Большинство — либо product-hype («AI co-founder» SaaS), либо «AI как рабочая сила» (coding, content, ops).

Base44 (Maor Shlomo): соло, ~несколько месяцев до acquisition Wix за $80M (2025). Тяжёлое использование AI для coding/product/агентов; после 2 недель AI-support отключили — понадобился human insight. Пример ускорения execution, не глубокого стратегического партнёрства. (связанные Fortune/TechCrunch упоминания 2026.) [arXiv]
Аналогичные one-person AI-компании (HeadshotPro Danny Postma и др.): AI = multiplier execution.
Академическая работа «AI as "Co-founder": GenAI for Entrepreneurship» (Cai et al., arXiv:2512.06506, декабрь 2025): GenAI действует как digital co-founder, снижает барьеры входа, позволяет меньшим командам/first-time founders, уменьшает размер founding teams и зависимость от опыта. Эффект сильнее в AI-rich локациях и AI-relevant секторах. [single-source на момент, China data]. [arXiv]

X (последние ~30–60 дней, июнь–июль 2026): в основном промо инструментов. Репрезентативные substantive:

@GoldenbergLior (14 Jul 2026): AI отлично делает execution, но judgment («какую проблему решать», «когда убивать фичу») остаётся человеческим. [@GoldenbergLior]
Лёгкие анекдоты типа «AI co-founder сказал неприятную правду» или «intellectual sparring partner». Сигнал низкий; независимых длинных threads с метриками почти нет.

Консенсус практики: AI отлично заменяет «команду исполнения», но качество стратегических решений сильно зависит от того, насколько жёстко вынуждаете non-sycophantic поведение и закрываете feedback loop на outcomes.

4. Границы и доверие. Bounded autonomy

Frameworks [emerging, 2025–2026]:

Bounded autonomy (XMPro, Feb 2025; MongoDB blog «The Case for Bounded Autonomy», 25 Feb 2026): агент свободен внутри заранее определённого безопасного envelope. Три слоя: operational boundaries (что физически можно), decision frameworks (bounded rationality), oversight (graduated intervention). Прогрессивная автономия: начинать с analyze/recommend → auto low-stakes с логом → auto в лимитах + escalate exceptions → расширение только по доказанной reliability. Обязательны causal audit trails, supervisor-агенты, kill switch. [Xmpro +1]

Практический decision-rights matrix для вашей пары:

AI решает сам (с логом): reversible, low-cost, content drafts, research synthesis, A/B-варианты, операционные чек-листы.
Обязательная эскалация: деньги выше порога, irreversible действия, стратегия/pivot, найм advisors, публичные заявления, юридически значимое.
Human всегда Accountable; AI может быть Responsible за execution.

Избегать двух крайностей: микроменеджмент убивает скорость; слепое делегирование — риск катастрофы. Решение — явная матрица + логи + прогрессивное расширение по evidence.

5. Синтез-рекомендация. Топ-7 улучшений

Ранжировано по ожидаемому эффекту на бизнес-результат (качество решений → меньше дорогих ошибок → быстрее/лучше revenue & funnel). Каждое — с механикой внедрения. Что выбросить как театр — в конце.

| Ранг | Улучшение | Механика внедрения | Ожидаемый эффект | Confidence |
|---|---|---|---|---|
| 1 | Обязательный Decision Artifact (memo) как гейт | Перед любым high/medium-stakes решением AI обязан выдать структурированный memo: Options table + цифры/расчёты + 3+ objections/steelman + pre-mortem + Recommendation + Confidence. Без артефакта — нет перехода к действию. | Максимально снижает sycophancy и поверхностные советы; создаёт audit trail | [emerging] практика + anti-sycophancy papers |
| 2 | Explicit Decision Rights Matrix + Escalation Gates | Зафиксировать матрицу (AI auto / escalate). Порог денег, список irreversible. Логировать все авто-действия. | Защита от катастроф + скорость на низких ставках; доверие | [emerging] MongoDB/XMPro 2025-26 |
| 3 | Mandatory Red-team / Advocate-Skeptic pass | На high-stakes: либо dual-mode (advocate + skeptic субагенты), либо «сначала 3 самых сильных возражения против моей идеи». | Прямое противоядие sycophancy; лучше решения | [emerging] |
| 4 | Outcome-tracked Evals + Growth-log update | После решения фиксировать: принято? → через N дней — outcome (метрика). Weekly review обновляет growth-log. KPI совета = accepted + positive business movement. | Закрывает feedback loop; AI учится на реальных результатах, а не на «понравилось» | [speculative] на точных KPI, [emerging] на необходимости |
| 5 | Session-start Re-anchor + External Memory Retrieval | Усилить существующий ритуал: обязательно retrieve релевантные куски growth-log + прошлые decision memos + «голос кофаундера» с первого сообщения. | Борьба с persona drift / regression to assistant | [emerging] |
| 6 | «Ask don't tell» + «Переубеди меня» протокол | Любой user-statement reframing в вопрос; для важных — явный режим «steelman alternative / convince me I'm wrong». | Сильное снижение sycophancy (Dubois 2026) | [emerging] |
| 7 | Intent-based / Task-specific Role (а не всегда длинный композит) | Персона только для style/voice/adversarial stance. На knowledge/factual/расчёты — neutral или stripped. | Избегает вреда accuracy (Hu et al. 2026 + Zheng) | [established] для accuracy trade-off |

Что выбросить как театр:

- Длинный composite-persona prompt «успешных фаундеров» как основной рычаг без гейтов и артефактов (Zheng + 2026 данные).
- «Право спорить до консенсуса» только в тексте промпта без внешнего enforcement.
- Неограниченная автономия без decision rights matrix и логов.
- Отсутствие outcome-трекинга (совет оценивается по «звучит умно»).

Одна главная рекомендация [emerging, основная предпосылка — вы будете дисциплинированно вести артефакты и outcomes]:
Внедрить Decision Artifact Gate + Decision Rights Matrix в первую очередь. Это превращает роль из «умного собеседника» в систему, которая вынуждена производить проверяемые, не-sycophantic outputs и уважает границы. Остальное (red-team, re-anchor, evals) — усиления поверх этого фундамента.

Чистая persona без механик — брендинг. С гейтами, артефактами и feedback loop — рабочий партнёр, способный реально двигать бизнес.

Полный список источников
- Zheng et al. (2024): https://arxiv.org/abs/2311.10054 (v3 Oct 2024)
- Hu et al. USC Expert Personas (2026): https://arxiv.org/html/2603.18507v1
- Dubois et al. «Ask don't tell» (2026): https://arxiv.org/abs/2602.23971
- MongoDB Bounded Autonomy (25 Feb 2026): https://www.mongodb.com/company/blog/technical/the-case-for-bounded-autonomy
- XMPro Bounded Autonomy (17 Feb 2025): https://xmpro.com/bounded-autonomy-a-pragmatic-response-to-concerns-about-fully-autonomous-ai-agents/
- Cai et al. AI as "Co-founder" (Dec 2025): https://arxiv.org/abs/2512.06506
- OpenAI sycophancy GPT-4o/GPT-5 (2025): https://openai.com/index/sycophancy-in-gpt-4o/ и https://openai.com/index/introducing-gpt-5/
- Search Engine Journal coverage persona study (Mar 2026): https://www.searchenginejournal.com/research-you-are-an-expert-prompts-can-damage-factual-accuracy/570397/
- X posts: @GoldenbergLior 14 Jul 2026 (judgment remains human); другие промо-посты 2026 (низкий independent signal)
- Дополнительно: taxonomy sycophancy (arXiv 2605.21778, May 2026), Nature warmth/sycophancy 2026, различные 2025–2026 practitioner reports.

Все ключевые claims привязаны к реальным извлечённым источникам. Там, где данные редки (глубокие независимые long-term кейсы «AI-кофаундер как партнёр»), явно помечено.
