# DR26-08-01-MACANTON-02-659 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-01-MACANTON-02-659
rail: chatgpt (codex exec (gpt-5.6-sol, подписка ChatGPT a2))
session_uuid: 019fbdac-9d72-7b92-b12e-ee63e01b681e
public_chat_url: НЕТ (CLI-рельса, публичного URL не существует)
collected_at: 2026-08-01T14:20:27Z
proof_of_work: 25 уник. URL, 14 веб-поисков — гейт пройден
note: чистый прогон, tool-mix = только web search
---

## 1. Вердикт по-русски — 246 слов

Это не воспроизводимая «карьерная воронка», а редкий стратегический исход, вероятность которого можно повысить, но нельзя запланировать. Компании покупают не GitHub-звёзды и не просто хорошего автора. Они покупают уже возникший контрольный пункт: инфраструктуру, через которую проходит их продукт; нейтральный стандарт, который опасно отдавать конкуренту; или команду, закрывающую срочный roadmap gap.

Лучшие сигналы — Bun уже обслуживал инфраструктуру Claude Code; llama.cpp стал фактическим runtime локального AI; Astral контролировал быстрорастущий Python toolchain; Promptfoo имел 130 тыс. активных пользователей в месяц и клиентов среди более 25% Fortune 500. OpenClaw — исключительный вирусный случай, а не типовой benchmark.

Наблюдаемого порога в звёздах нет. Реальный порог звучит так: «если этот проект исчезнет, форкнется к конкуренту или изменит направление, нам придётся срочно строить замену». Обычно до сделки существует сотрудничество: использование внутри компании, интеграция, общий клиент, финансирование или несколько знакомых core contributors.

Для двух человек из ЕС рациональная стратегия — не «делать проект для продажи», а создать нейтральный, permissively licensed слой в недостающем месте стека одной или нескольких лабораторий; измерять реальное использование; сохранять переносимость между вендорами; пройти security/IP due diligence заранее. Инкорпорация нужна при первых договорах или найме, foundation — лишь когда нейтральность сама является частью ценности.

Честная оценка: для случайного AI-OSS проекта шанс такого исхода значительно ниже 1%; даже среди заметных проектов — вероятно единицы процентов. Улучшить шансы можно, став незаменимым upstream-компонентом. Но строить финансовый план на acqui-hire нельзя: базовый план должен предполагать, что его не будет.

---

## 2. Подтверждённый case corpus

Я включал только случаи, где первичный источник подтверждает одновременно OSS-проект и переход команды/автора либо формальное финансирование. Метрика «на дату первого контакта» почти никогда не публикуется; ниже используется ближайшая публично датированная величина. Это важное ограничение, а не повод подставлять сегодняшние числа.

| Проект | Мейнтейнеры | Получатель / покупатель | Дата | Форма и судьба проекта | Метрика около сделки | Через 12 месяцев / сейчас | Источник |
|---|---|---|---|---|---|---|---|
| Pyroscope | Ryan Perry, Dmitry Filimonov, команда | Grafana Labs | 16.03.2023 | Покупка компании; Pyroscope объединён с Grafana Phlare, OSS продолжен | Не раскрыта | Стал Grafana Pyroscope | [Grafana](https://grafana.com/blog/2023/03/15/pyroscope-grafana-phlare-join-for-oss-continuous-profiling/) |
| Airoboros, TheBloke quantizations и др. | Jon Durbin, Tom Jobbins, Eric Hartford, Jeremy Howard и др. | a16z OSS AI Grants | 30.08.2023 | Безвозвратные гранты, не инвестиция/SAFE; IP и независимость сохранялись | Получатели уже имели заметное использование в Local LLM ecosystem; сопоставимых единых чисел нет | Независимые проекты; результаты неодинаковы | [a16z](https://a16z.com/supporting-the-open-source-ai-community/) |
| Optic | Aidan Cunniffe и команда | Atlassian | 29.04.2024 | Полное приобретение OSS-компании; интеграция API tooling; цена и точный IP schedule не раскрыты | Atlassian указывает enterprise adoption, без чисел | Команда внутри Atlassian; независимое управление проектом не обещано | [Atlassian](https://www.atlassian.com/blog/company-news/optic-acquisition) |
| Argilla | Daniel Vila Suero, David Ruíz и команда | Hugging Face | 13.06.2024 | Acquisition; вся команда перешла в HF, OSS Argilla продолжился | 1,5 года совместных интеграций; сотни участников Data Is Better Together | Argilla 2.x выпускался внутри HF и оставался OSS | [Argilla](https://argilla.io/blog/argilla-joins-hugggingface/), [HF follow-up](https://huggingface.co/blog/dvilasuero/argilla-2-0) |
| XetHub / Git-Xet | Yucheng Low, Ajit Banerjee, Rajat Arya, команда | Hugging Face | 08.08.2024 | Покупка компании и технологии; команда стала HF Xet Team; backend внедрён в Hub | Строился для TB-scale repositories; публичная adoption-метрика не дана | Xet стал storage layer Hugging Face Hub | [Hugging Face](https://huggingface.co/blog/xethub-joins-hf), [migration follow-up](https://huggingface.co/blog/migrating-the-hub-to-xet) |
| Grep | Dan Fox | Vercel | 20.11.2024 | Покупка проекта/компании, автор вошёл в Vercel AI team | Не раскрыта | 12-месячного публичного отчёта о независимом governance нет | [Vercel](https://vercel.com/blog/vercel-acquires-grep) |
| Bun | Jarred Sumner и команда | Anthropic | 03.12.2025 | Acquisition; Bun остаётся MIT и OSS; команда присоединилась | 7 млн загрузок/месяц, 82 тыс. stars; уже использовался инфраструктурой Claude Code | 12 месяцев ещё не прошло | [Anthropic](https://www.anthropic.com/news/anthropic-acquires-bun-as-claude-code-reaches-usd1b-milestone) |
| OpenClaw, ранее Clawdbot/Moltbot | Peter Steinberger и contributors | OpenAI | 14–15.02.2026 | Steward-hire: Steinberger нанят; объявлена независимая foundation, OpenAI — спонсор; проект не приобретён | 9 тыс. stars в первые сутки; около 180–214 тыс. к найму, но последние числа вторичные | 12 месяцев не прошло; точная юрисдикция/устав foundation публично не подтверждены | [TechCrunch](https://techcrunch.com/2026/02/15/openclaw-creator-peter-steinberger-joins-openai/); метрика — [CSA research note](https://labs.cloudsecurityalliance.org/wp-content/uploads/2026/05/CSA_research_note_openclaw-claw-chain-cve_20260517-csa-styled.pdf) |
| ggml / llama.cpp | Georgi Gerganov и GGML team | Hugging Face | 20.02.2026 | Команда стала full-time HF; обещаны техническая автономия и сохранение OSS; передача IP не объявлена | 94,5 тыс. stars 06.02.2026; длительное сотрудничество и HF core contributors upstream | 12 месяцев не прошло | [Hugging Face](https://huggingface.co/blog/ggml-joins-hf); архивная метрика — [06.02 snapshot](https://cve.imfht.com/ti_screenshot/2026-02-06/webpage-2026-02-06T23-07-21.787Z-37a4f365d54aade64505.pdf) |
| Promptfoo | Ian Webster, Michael D’Angelo, команда | OpenAI | 09.03.2026 | Согласованное приобретение; OSS обещано сохранить, enterprise-tech интегрируется в Frontier | 350 тыс. пользователей всего, 130 тыс. MAU, более 25% Fortune 500 | 12 месяцев не прошло; закрытие сделки и цена не раскрыты | [Promptfoo](https://www.promptfoo.dev/blog/promptfoo-joining-openai/), [OpenAI](https://openai.com/index/openai-to-acquire-promptfoo/) |
| Astral: Ruff, uv, ty | Charlie Marsh и команда | OpenAI | 19.03.2026 | Согласованное acquisition; команда в Codex; OSS обещано поддерживать | uv 126 млн, Ruff 179 млн, ty 19 млн monthly downloads — вторичный снимок около сделки | 12 месяцев не прошло; closing зависел от approvals | [OpenAI](https://openai.com/index/openai-to-acquire-astral/); метрики — [Ars Technica](https://arstechnica.com/ai/2026/03/openai-is-acquiring-open-source-python-tool-maker-astral/) |
| VoidZero: Vite, Vitest, Rolldown, Oxc | Evan You и команда | Cloudflare | 04.06.2026 | Acquisition; команда продолжает вести проекты; MIT, vendor-neutral и community-driven обещаны; $1 млн ecosystem fund | Vite ≈130 млн загрузок/нед.; Cloudflare plugin ≈13,9 млн/нед. | Слишком рано оценивать | [Cloudflare release](https://www.cloudflare.com/press/press-releases/2026/cloudflare-acquires-voidzero-to-build-the-future-of-the-ai-native-web/), [team post](https://blog.cloudflare.com/voidzero-joins-cloudflare/) |
| Continue | Ty Dunn, Nate Sesti / Continue team | Cursor | ≈18.06.2026 | Покупка; standalone service закрыт, repo стал read-only — противоположность steward-hire | Около 34 тыс. stars — вторичный источник | Слишком рано; continuity проекта уже нарушена | [Continue announcement](https://www.continue.dev/); дата/метрика — [secondary report](https://buttondown.com/ponderoai/archive/pondero-brief-2026-06-23-continue-is-dead-you/) |

### Что не является таким исходом

По состоянию на 01.08.2026 я не нашёл первичных подтверждений поглощения вместе с мейнтейнером для vLLM, SGLang, Ollama, LangChain, LlamaIndex, Instructor, DSPy, Aider, Cline, OpenRouter, Zed, Marimo либо типичного MCP-сервера. У многих есть VC, sponsors, foundation hosting или собственная компания — это не acqui-hire.

Zed, например, остаётся самостоятельной компанией с $32 млн Series B и OSS-моделью, а не поглощённым проектом. [Zed](https://zed.dev/blog/sequoia-backs-zed)

vLLM в Linux Foundation — передача нейтрального governance, а не найм авторов. Аналогично пожертвование MCP foundation не является приобретением мейнтейнеров.

---

## 3. Типология сделок

| Форма | Кто обычно начинает | Компенсация | Что отдаёт мейнтейнер | Что покупает компания |
|---|---|---|---|---|
| Полное acquisition / acqui-hire | Corp-dev после уже существующей интеграции; иногда founder через инвесторов | Cash/equity за компанию; retention equity/RSUs; зарплаты. Почти всегда undisclosed | Акции, IP, trademark, продуктовый контроль; иногда независимый roadmap | Команду, IP, клиентов, enterprise contracts и время выхода на рынок |
| Steward-hire | Чаще стратегический buyer; возможна конкурентная hiring process | Зарплата/RSUs; отдельное sponsorship проекта; детали undisclosed | Рабочее время и часть фактического влияния; формально не обязательно IP | Авторитет steward, continuity, roadmap alignment и предотвращение ухода к конкуренту |
| Sponsorship без найма | Project подаёт заявку либо dependency-owner сам выходит на связь | Recurring donation, hardware/cloud credits, оплаченные milestones | Обычно только отчётность и sponsor recognition | Supply-chain stability, goodwill, security work без владения |
| Grant/fellowship | Почти всегда maintainer подаётся на open call | Non-dilutive grant, stipend, equipment/credits | Milestones, отчётность; иногда public-interest deliverables | Не эксклюзивный актив, а снижение ecosystem risk |
| Contractor → employee/vendor conversion | Любая сторона после пилота | Consulting fee → зарплата/RSUs или enterprise contract | Независимость, иногда assignment созданного по контракту IP | Проверенную совместимость и снижение hiring risk |
| Foundation transfer | Владелец проекта совместно с foundation/sponsors | Обычно не продажная цена; membership dues, earmarked funding | Trademark/repository governance передаются foundation | Нейтральность стандарта и более широкое принятие |

GitHub Accelerator 2024 дал 11 проектам по $40 тыс. non-dilutive sponsorship плюс credits/benefits; это funding funnel, не hiring funnel. [GitHub](https://github.blog/news-insights/company-news/2024-github-accelerator-meet-the-11-projects-shaping-open-source-ai/)

Alpha-Omega выдал почти $4,5 млн security grants в 2024 году. [Alpha-Omega](https://alpha-omega.dev/blog/alpha-omega-2024-annual-report/)

Sovereign Tech Fellowship оплачивает 12 месяцев maintenance, security и community work и прямо адресован физическим мейнтейнерам, а не их работодателям. [Sovereign Tech Agency](https://www.sovereign.tech/news/apply-to-the-fellowship-for-maintainers)

### Наблюдаемые trigger metrics

Единого star threshold нет:

- Grep был куплен без публично продемонстрированной массовой метрики.
- llama.cpp и Bun имели десятки тысяч stars и прямую инфраструктурную значимость.
- Astral и Vite имели сотни миллионов package downloads.
- Promptfoo имел enterprise penetration, гораздо более ценный сигнал, чем stars.
- OpenClaw был вирусным и вызвал конкурентную борьбу.

Устойчивый паттерн — сочетание хотя бы двух сигналов:

1. Покупатель уже использует проект или поддерживает прямую интеграцию.
2. У проекта есть ежедневная production-активность, которую трудно заменить.
3. Команда восполняет конкретную дыру roadmap.
4. Конкурент может получить тот же актив.
5. У автора есть доверие сообщества, не переносимое простым форком.

---

## 4. Conflict pathway

### Где friction, вероятно, помог visibility

OpenClaw — единственный хорошо заметный случай в этом корпусе, где trademark friction непосредственно предшествовал конкурирующему найму. Но формулировка исходного брифа чрезмерна:

- Это не подтверждённые «DMCA takedowns».
- Публичные свидетельства описывают письмо/требование по товарному знаку Clawd/Claude.
- Сам Steinberger говорил, что Anthropic был «really nice» и не начинал тяжёлую юридическую атаку.
- OpenAI нанял его из-за продукта и агентного roadmap, а не потому, что он выиграл конфликт.

То есть конфликт усилил внимание, но не создал ценность.

### Где friction убивает результат

Типовой неуспешный unofficial-client проект:

- обходит authentication, rate limits или payment;
- использует leaked/private API;
- копирует trademark/UI так, что создаёт confusion;
- требует постоянного cat-and-mouse обхода;
- не имеет самостоятельной архитектуры или пользователей вне одного vendor;
- реагирует на complaint эскалацией, harassment или evasive forks.

Такие проекты могут получить C&D, token revocation, store ban или DMCA и не становятся публичными acquisitions. Полного реестра «получили ничего» не существует — отсутствие сделки обычно не объявляется, поэтому нельзя честно вывести количественную success rate из публичных новостей.

### Разделяющий фактор

Unofficial wrapper повышает вероятность найма только если он одновременно:

- обнаруживает новый устойчивый workflow;
- использует законные публичные interfaces;
- переносим между несколькими providers;
- показывает demand, который vendor недооценил;
- имеет чистый IP и зрелую security posture.

Он понижает вероятность найма самим vendor, если его основная ценность — нарушение правил этого vendor. Rival может заинтересоваться, но только если workflow можно легально перенести.

---

## 5. Disqualifiers

- **Лицензия.** MIT/Apache/BSD упрощают интеграцию и dual licensing. GPL/AGPL не запрещают покупку — покупатель может приобрести copyright и перелицензировать только принадлежащие продавцу части, — но усложняют provenance и proprietary embedding. Zed успешно финансируется с GPL/AGPL, поэтому «GPL делает сделку невозможной» неверно.
- **Разрозненный copyright.** Сотни contributors без CLA/DCO и спорные assignments могут сделать эксклюзивную передачу IP невозможной.
- **Trademark collision.** Это прямой due-diligence red flag; OpenClaw показывает стоимость хаотичного rename.
- **AI-generated code без provenance.** Проблема не сам инструмент, а невозможность подтвердить review, origin, license compatibility и отсутствие скопированных фрагментов.
- **Предыдущая работа.** Employer invention-assignment, moonlighting ban, grant obligations и университетские права опаснее non-compete.
- **Security debt.** Отсутствие SECURITY.md, secret handling, signed releases, dependency policy, threat model и incident process особенно опасно для agents/MCP.
- **Bus factor = 1.** Не всегда disqualifier — OpenClaw опровергает это как абсолют, — но ухудшает continuity и bargaining position.
- **Токсичный tracker.** Покупается не только код, но и community legitimacy.
- **Юрисдикция.** ЕС не является дисквалификацией: Argilla — Испания, GGML связан с европейской командой, OpenClaw — Австрия, VoidZero распределён глобально. Но экспортный контроль, sanctions, employment entity и immigration могут изменить форму сделки.
- **Смешанные commercial/community assets.** Неясно, кому принадлежат домен, trademark, telemetry, customer contracts и contributor data.

---

## 6. Последовательность на 12–18 месяцев

### Месяцы 0–2: выбрать wedge

- Найти не «интересную AI-фичу», а повторяющуюся ручную работу внутри roadmap 2–3 лабораторий.
- Искать слой рядом с model serving, eval/security, agent runtime, context/data movement, reproducible environments или interoperability.
- Проверка: минимум пять публичных issues/docs/comments от потенциальных покупателей, указывающих на одну проблему.
- Не строить wrapper, зависящий от закрытого undocumented API одного vendor.

### Месяцы 2–4: создать clean-room ядро

- Apache-2.0 либо MIT; Apache предпочтительнее, когда важен patent grant.
- `LICENSE`, `NOTICE`, `CONTRIBUTING`, DCO/CLA decision, `SECURITY.md`, threat model, release signing, SBOM.
- `GOVERNANCE.md` с maintainer roles и правилами передачи trademark/repository.
- Журнал использования AI: модель/инструмент, human review, тесты, provenance для чувствительных участков.
- Компания пока необязательна; домен и trademark должны принадлежать явно определённому лицу/entity.

### Месяцы 4–8: доказать use, не attention

Checkpoint:

- 3–5 независимых production users;
- минимум один внешний recurring contributor;
- retention/weekly active installations, а не только stars;
- публичные benchmarks и воспроизводимые integration tests;
- два providers или два deployment targets;
- ни один клиент не даёт более 50% использования.

### Месяцы 8–12: стать upstream dependency

- Добиться упоминания в документации, issue или integration repo потенциального покупателя.
- Сделать официальный adapter совместно с его инженером.
- Провести security audit либо независимый review.
- Инкорпорироваться перед первым существенным контрактом, наймом или trademark registration.
- Вести IP ledger: автор, employer status, license, CLA/DCO, AI assistance.

### Месяцы 10–14: контакт

Inbound не обязателен. Правильный outbound — не «купите нас», а:

> Ваши пользователи делают X; наш OSS-компонент уже обрабатывает Y операций у Z независимых команд; вот benchmark и список upstream gaps. Предлагаем совместную интеграцию/оплачиваемый maintenance pilot.

Контактировать с engineer/product owner, затем OSS program office или partnerships. Corp-dev появляется позднее. Не создавать искусственную эксклюзивность; честно сообщать о других интеграциях.

### Месяцы 14–18: создать выбор форм

Подготовить три предложения:

1. sponsorship + публичные milestones;
2. paid upstream maintenance/contract;
3. team acquisition или steward-hire с OSS covenant.

Foundation создавать только если vendor neutrality действительно повышает adoption или если появляется несколько корпоративных sponsors. Foundation до product-market fit добавляет расходы и может осложнить продажу IP.

К моменту diligence должны существовать:

- cap table и invention assignments;
- contributor/IP ledger;
- список dependencies и licenses;
- security reports и incident history;
- anonymized adoption dashboard;
- governance и succession policy;
- trademark/domain ownership;
- перечень customer/vendor obligations;
- письменное обещание по будущей лицензии, если оно является условием сделки.

### Base rate

Публичного знаменателя «независимые AI-tooling maintainers» нет. GitHub Accelerator выбрал лишь 11 AI-проектов даже для грантового cohort, а подтверждённых project-plus-team acquisitions за 2023–01.08.2026 — низкие десятки при тысячах заметных AI repositories и гораздо большем числе мелких.

Разумный planning prior:

- произвольный новый AI OSS repo: намного меньше 0,1%;
- проект с устойчивыми production users: менее 1%;
- стратегический dependency с прямым использованием frontier company: несколько процентов, но данных для точной оценки нет.

Это оценка порядка величины, не статистически измеренная вероятность.

---

## 7. Три положения брифа, оказавшиеся ложными

1. **«Anthropic ответил DMCA takedowns».** Подтверждается trademark complaint/письмо о названии Clawd, а не DMCA против кода или серия takedowns. Сам автор характеризовал контакт как вежливый.

2. **«OpenClaw и ggml — одна и та же steward-hire структура».** OpenClaw объявлен как независимый foundation project со спонсорством OpenAI. Для ggml первичный пост говорит, что команда «joining HF» с автономией и сохранением OSS, но не объявляет независимый фонд или передачу проекта отдельному владельцу.

3. **«Есть широкий ряд аналогичных исходов среди vLLM, SGLang, Ollama, LangChain, LlamaIndex, Aider, Cline, Zed, Marimo и MCP servers».** Для большинства это независимые компании, foundation-hosted проекты, grants/sponsorships либо просто заметные OSS repositories. Они полезны как controls, но не являются доказанными project-plus-maintainer absorptions.
