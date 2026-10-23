# DR26-09-23-ZB-01-0631 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!-- rail: claudeai | Fable 5.1 Max + Research mode (381 sources) | https://приватный чат | collected 2026-09-23 07:20 via live Chrome-MCP, API dump of assistant text + artifacts -->



[TEXT]
Запускаю глубокое исследование. Допущение: рассматриваю вопрос и как рыночный анализ (кто из китайских лабораторий реально выходит в Долину и что им там нужно), и как практическую проверку гипотезы — нужен ли им именно локальный ecosystem/DevRel-лид, кого они уже нанимают, и как под это позиционироваться.

[TEXT]
Your research report on the viability of a Silicon Valley-based DevRel/Ecosystem lead for Chinese LLM labs is ready. It stress-tests the hypothesis against market data (OpenRouter/Vercel adoption figures showing Chinese models at 46% of routed tokens), pricing dynamics, and the regulatory landscape (Entity List, state/federal bans, CAISI evaluations, and Anthropic's Claude-distillation report).

The report includes a detailed lab-by-lab breakdown (DeepSeek, Moonshot/Kimi, Zhipu/Z.ai, MiniMax, Alibaba/Qwen, ByteDance/BytePlus, plus a second-tier survey), a function-vs-structure fit matrix, an ideal candidate profile with 2025–2026 compensation benchmarks, and a prioritized target list with pitch angles and candidate-side risks.

Bottom line: the hypothesis holds only partially. Expansion into the Western developer market is real but largely "office-less"—driven through US inference providers, open-weight distribution, and coding plans rather than a physical Bay Area presence. A Valley-based lead makes most sense for MiniMax and Alibaba/Qwen (ideally via a Singapore/remote structure, not a US entity), while it's unlikely or untenable for Z.ai (Entity List) and DeepSeek (no GTM function).

[ARTIFACT Silicon Valley DevRel Lead for Chinese LLM Labs: Hypothesis Validation and Targeting Strategy cmd=create]
# Silicon-Valley DevRel/Ecosystem-лид для китайских LLM-лабораторий: проверка гипотезы (по состоянию на 23 сентября 2026)

## Executive Summary — Вердикт

Гипотеза «Valley-based ecosystem/DevRel-лид — это именно то, что нужно этим лабораториям» **верна лишь частично и сильно зависит от конкретной лаборатории**. Главный вывод: экспансия китайских LLM-лабораторий на западный рынок разработчиков — **свершившийся факт**, но она в подавляющем большинстве случаев идёт **через посредников (inference-провайдеры, агрегаторы, IDE) и open-weight-дистрибуцию, а не через физическое присутствие в Кремниевой долине**. Ни одна из лабораторий первого эшелона не имеет полноценного DevRel-офиса в Bay Area по состоянию на сентябрь 2026.

**TL;DR — три пункта:**

- **Экспансия реальна, но структурно «безофисная».** По данным CNBC (расследование от 7 июля 2026, на базе OpenRouter), китайские модели «сидят выше 30% каждую неделю с 8 февраля 2026», достигнув пика в 46% против среднего 11% за предыдущие 12 месяцев и всего 4,5% в первой половине 2025. Западное принятие DeepSeek, Qwen, Kimi, GLM и MiniMax произошло органически — через Hugging Face, OpenRouter, Together AI / Fireworks / DeepInfra / Baseten и Claude-Code-совместимые coding-планы. Де-факто «ecosystem partner» — американские inference-провайдеры.
- **Valley-based штатный DevRel-лид нужен меньшинству лабораторий и почти никому — под американским юрлицом.** Геополитика (Entity List, законопроекты о запрете DeepSeek, CAISI-оценки, обвинения в дистилляции Claude) делает физическое присутствие в США репутационно и юридически токсичным. Оптимальная структура для большинства — **Singapore-хаб + ремоут-DevRel + амбассадорские программы + партнёрства с US-инфраструктурой**, а не W-2-сотрудник в SF.
- **Реальный спрос на «человека в Долине» концентрируется у 2–3 игроков** — прежде всего у тех, кто (а) не в Entity List, (б) строит платящую западную выручку и (в) уже засветился в SF: Alibaba/Qwen (реальная US-инфраструктура с 2015 + амбассадорская программа), MiniMax (Singapore-домицилированный, ~9% штата в США, была вакансия GTM Manager в SF Bay Area) и Moonshot/Kimi (глобальная амбассадорская программа с 29 июля 2026). Z.ai активно строит экосистему (Zixuan Li — Head of Global Ecosystem), но он в Entity List и сознательно избегает рынка США.

---

## Key Findings

1. **Западное принятие уже случилось — без DevRel.** По данным OpenRouter (via CNBC, 7 июля 2026), доля US-моделей на платформе упала с ~70% (июнь 2025) до ~30% (июнь 2026). Китайские модели заняли 46,4% маршрутизируемых токенов против 35,7% у US-моделей; Anthropic — лишь 14,8%. DeepSeek — **крупнейший отдельный вендор: 17,6% токенов, ~5,13 трлн еженедельно**; Alibaba Qwen следом с 13,9% (2,77 трлн). Это произошло за счёт цены и open-weight-лицензий, а не маркетинга.

2. **Ценовое преимущество — движущая сила.** Justin Summerville (OpenRouter) в CNBC (7 июля 2026): open-source китайские модели работают «на 60–90% дешевле» лидирующих моделей Anthropic и OpenAI (напр., DeepSeek V4 Flash $0,14 против GPT-5.5 $5,00 за 1M input-токенов; MiniMax M3 $0,60/M — примерно в 8 раз дешевле Claude Opus 4.8 при $5,00/M).

3. **Inference-провайдеры США = де-факто экосистемные партнёры.** Together AI, Fireworks, DeepInfra, Baseten, Groq хостят DeepSeek, Qwen, Kimi, GLM, MiniMax с day-zero-поддержкой. Baseten's Frontier Gateway позволяет лабораториям white-label-хостинг. Kimi K3 вышел на AWS Bedrock с revenue-share (переговоры о ~30% выручки с AWS/Microsoft/Google, по Reuters). Это снимает необходимость в собственной US-инфраструктуре и частично в DevRel.

4. **Coding-планы — главный западный GTM-канал.** Z.ai GLM Coding Plan (list-цены $18 Lite / $72 Pro / $160 Max в мес., промо от $3; ZCode запущен 2 июля 2026), плюс Kimi/Qwen/MiniMax coding-планы — все Claude-Code / Cline / Cursor / OpenCode / Kilo Code / Roo Code-совместимые. Anthropic блокирует материковый Китай, а GLM/Kimi предоставляют Anthropic-совместимые эндпоинты — «китайский бэкенд к западному фронтенду». Vercel (Harpreet Arora, глава agentic-инфраструктуры, CNBC 7 июля 2026): GLM 5.2 показал «самое быстрое принятие любой модели», отслеженной Vercel в 2026 — «за первую полную неделю после запуска дневной объём токенов вырос примерно в 27 раз, а число клиентов — примерно в 80 раз».

5. **Геополитический потолок.** Zhipu/Z.ai в Entity List с 16 января 2025. Законопроекты (No DeepSeek on Government Devices Act, No Adversarial AI Act), запреты на уровне штатов (Texas, New York и др.), CAISI-оценки DeepSeek и обвинения в дистилляции Claude создают токсичную среду для физического присутствия в США.

6. **Кадры и структура.** Прецеденты: Manus/Butterfly Effect и HeyGen полностью редомицилировались (Singapore / LA), «отмыв» китайские корни («Singapore washing»). Первый эшелон лабораторий этого не делает — они держат R&D в Китае, используют Singapore-юрлица (MiniMax, BytePlus) и амбассадоров/ремоут для Запада.

---

## Детальная таблица по лабораториям

| Лаборатория | US / Bay Area присутствие | Singapore / др. хабы | Международный лид | DevRel/ecosystem-найм (даты) | Западные GTM-каналы | Регуляторная экспозиция | Вероятность найма Valley-based ecosystem-лида (6–12 мес) | Confidence |
|---|---|---|---|---|---|---|---|---|
| **DeepSeek** | Нет юрлица, нет офиса, нет DevRel-функции | Нет | Нет (research-first; Liang Wenfeng контролирует ~90%) | Никаких DevRel/GTM-ролей; найм только research в Ханчжоу/Пекине | Органическая виральность; open weights на HF; хостинг Together/Fireworks/DeepInfra; #1 на OpenRouter (17,6%) | Максимальная: федеральные/штатные запреты, CAISI-оценки, обвинения в дистилляции (GTG-16001) | **Очень низкая** — идеологически против коммерциализации/маркетинга | Высокая |
| **Moonshot (Kimi)** | Нет US-юрлица/офиса | Нет постоянного хаба | Нет публично названного DevRel/международного лида | Kimi Global Ambassador Program с 29 июля 2026; Kimi K3 на AWS Bedrock | Ambassador-программа, OpenRouter, X, AWS Bedrock, coding-план | Средняя: Bessent грозил Entity List; обвинения в дистилляции (GTG-16002); реструктуризация под HK IPO | **Средняя** — но ставка на амбассадоров/ремоут, не на SF-штат | Средняя |
| **Zhipu / Z.ai** | Нет US-юрлица; только event-визиты (AI Engineer World's Fair) | Офисы: Middle East, Singapore, UK, Malaysia; innovation-центры в Indonesia, Vietnam | **Zixuan Li — Head of Z.ai Global Ecosystem** (базируется в Cambridge, MA; MIT/CMU; активен на X); Carol Lin (VP); Cara Li (роль неясна, единичный источник) | Публичная экосистемная активность (GLM Coding Plan, ZCode); event-присутствие в SF | GLM Coding Plan, ZCode, HF (MIT-лицензия), OpenRouter, Cline/Claude Code | **Максимальная: в Entity List с 16 января 2025** (presumption of denial); обвинения в дистилляции (GTG-16006) | **Низкая для США** (Entity List); высокая для не-US хабов | Высокая |
| **MiniMax** | ~9% штата в США (оценка Revelio, март 2026, ~40 чел.); вакансия **GTM Manager, SF Bay Area** (закрыта) | HQ Singapore (MINIMAX PTE. LTD.); операционно в Шанхае | Нет публично названного DevRel-лида | GTM Manager SF (закрыта, 2026) | minimax.io Open Platform, OpenRouter (M-серия), Talkie/Hailuo, дешёвый API ($0,60/M) | Средняя: обвинения в дистилляции (GTG-16003, proxy через shell) | **Выше среднего** — Singapore-домициль + реальный US-штат + консюмер-продукты | Средняя |
| **Alibaba (Qwen)** | **Реальная US-инфраструктура: дата-центры US-West (Silicon Valley, с 2015) и US-East (Virginia); US API-эндпоинт Model Studio** | Singapore-эндпоинт; глобальная Alibaba Cloud | Публичное лицо ВАКАНТНО: Junyang Lin (@JustinLin610) ушёл 3 марта 2026; преемник не назван (OfficeChai, март 2026) | **Qwen Ambassador Program** (Dev + Event треки) с 11 мая 2026 (qwen.ai/ambassador); утечка команды Q1 2026 | HF (600M+ загрузок / 1B+ по ряду оценок, ~40% деривативов), OpenRouter, Model Studio, амбассадоры, Discord | Средняя: не в Entity List; обвинения в дистилляции (крупнейшая кампания GTG-16005, 151M) | **Высокая** — есть US-инфра, деньги, экосистема; но культура «Gemini-fication» и утечка кадров | Средняя |
| **ByteDance (Seed/BytePlus)** | Нет отдельного SV AI-офиса; TikTok USDS отдельно | **BytePlus HQ Singapore** (100+ чел., 3 континента); ноды Singapore/London | Нет публично названного DevRel-лида | Найм в Singapore (BytePlus); нет верифицированной US DevRel-роли | BytePlus ModelArk (USD-биллинг), fal.ai/PiAPI, Seedance 2.x глобально | Средняя-высокая: наследие TikTok/CFIUS; обвинения в дистилляции | **Средняя** — но через BytePlus/Singapore, не через SF | Средняя |

**Второй эшелон (кратко):** Tencent (Hunyuan; Hy3 в топ-10 OpenRouter), Baidu (ERNIE), StepFun (Step 3.7 Flash в топ-10 OpenRouter), Xiaomi (MiMo — до 18–22% coding-токенов на OpenRouter в июне 2026; обвинения в дистилляции GTG-16008), Meituan (LongCat), Kuaishou (Kling), 01.AI, Baichuan, iFlytek (Spark), Shengshu (Vidu), Ant (Ling/Ring), Huawei (Pangu). Все ориентированы прежде всего на внутренний рынок и open-weight-дистрибуцию; собственный SF-DevRel маловероятен ни у кого в ближайшие 12 месяцев. SenseTime также фигурирует в отчёте о дистилляции Claude.

---

## Таблица: функция × структурная альтернатива

Оценка (1–5, где 5 = лучший фит) того, насколько каждая структура покрывает нужную функцию:

| Нужная функция | (1) US W-2 под US-субсидиаром | (2) Singapore-employee, покрывает US | (3) China-based DevRel в US-часы | (4) Аутсорс US-inference/агрегаторам | (5) Fractional advisor/ambassador | (6) Acqui-hire/US-shell |
|---|---|---|---|---|---|---|
| Developer community & DevRel (docs, X/Discord, хакатоны) | 5 (но юр. риск) | 4 | 3 | 2 | 4 | 4 |
| Ecosystem-партнёрства (inference, IDE, cloud-marketplace) | 5 | 4 | 3 | **5** | 3 | 4 |
| Enterprise sales / trust / compliance | 4 | 3 | 1 | 4 (US-hosted снимает data-residency) | 2 | 5 |
| Policy / GR / нарратив | 3 (но exposure) | 2 | 1 | 2 | 3 | 4 |
| Benchmark & research-community (evals, leaderboards) | 4 | 4 | **5** (research в Китае) | 2 | 3 | 3 |
| Open-source governance & license-strategy | 3 | 3 | **5** | 2 | 3 | 2 |
| Recruiting US-talent | 5 | 3 | 1 | 1 | 2 | 5 |

**Интерпретация:** ecosystem-партнёрства и enterprise-trust лучше всего закрываются аутсорсом на US-инфраструктуру (столбец 4) или acqui-hire (6); community/DevRel — Valley-человеком (1), но с юридическим риском, поэтому fractional/ambassador (5) — компромисс; research/open-source — китайской командой (3). Единственная структура, где Valley-based штатный лид явно доминирует, — recruiting US-talent и «живое» комьюнити-присутствие. Для большинства прочих функций более дешёвые и менее рискованные альтернативы конкурентоспособны — именно поэтому лаборатории пока их и выбирают.

---

## Регуляторные / геополитические ограничения

- **Entity List:** Zhipu AI (все дочки — Beijing Zhipu Huazhang и др.) в Entity List **с 16 января 2025** (90 FR 4618–4619, Section 744.11 EAR, presumption of denial для всех items). DeepSeek, Alibaba, Moonshot, MiniMax, ByteDance — **не в списке** на сентябрь 2026, но BIS предупреждал (май 2025), что участников AI-training могут добавить «даже без нарушения EAR». Treasury Secretary Scott Bessent публично намекал на возможное внесение Moonshot.
- **Законодательные запреты:** No DeepSeek on Government Devices Act (H.R.1121, 119-й Конгресс, внесён Gottheimer/LaHood + 16 co-sponsors, февраль 2025); No Adversarial AI Act (Moolenaar/Torres); Senate-версия (Cassidy/Rosen) с требованием публичного списка «adversarial»-моделей от Federal Acquisition Security Council. Штаты: Texas (первый, февраль 2025), New York (10 февраля 2025), затем Virginia, Iowa, South Dakota, North Carolina, Alabama (запрет DeepSeek + Manus). 21 генпрокурор штатов призвали Конгресс к федеральному запрету. Федеральные агентства (Commerce, Navy) уже запретили DeepSeek на устройствах.
- **CAISI/NIST-оценки:** Отчёт от 30 сентября 2025 — DeepSeek R1-0528 «ответил на 94% откровенно злонамеренных запросов при использовании распространённой jailbreak-техники, против 8%» у US-моделей; агенты «в 12 раз чаще следовали hijacking-инструкциям»; модели «воспроизводили в 4 раза больше неточных и вводящих в заблуждение нарративов КПК»; загрузки выросли «почти на 1000% с января 2025». Оценка DeepSeek V4 Pro (май 2026, оценка проведена в апреле 2026) — отставание от фронтира ~8 месяцев; V4 назван самой способной PRC-моделью на тот момент.
- **Дистилляция Claude:** Threat-intelligence-отчёт Anthropic (сентябрь 2026, ~154 стр., выпущен ~10 сентября) приписывает ~190 млн обменов с Claude (май–июль 2026) семи лабораториям: Alibaba (GTG-16005, 151M), Moonshot (GTG-16002, 23M), DeepSeek (GTG-16001, 12,1M), Zhipu (GTG-16006, 3,4M через 273 фейковых аккаунта), Xiaomi (GTG-16008, 400K), плюс SenseTime и MiniMax (GTG-16003, proxy через shell-компанию). Прямой репутационный риск для любого, кто публично представляет эти бренды в США.
- **Outbound-investment / CFIUS:** Executive Order (действует с января 2025) ограничивает US-инвестиции в китайский AI. Именно поэтому Manus/HeyGen редомицилировались. Treasury рассматривал раунд Manus ($75M, Benchmark). Сделка Meta–Manus ($2B, закрыта 29 декабря 2025) была заблокирована регуляторами в январе 2026.
- **Прецеденты структур:** Alibaba Cloud — реальные US-дата-центры (Silicon Valley us-west-1 с марта 2015, Virginia us-east-1). TikTok/ByteDance — сегрегация данных, US-исполнители, USDS Joint Venture LLC (создана 22 января 2026). Manus — полный «Singapore washing» (40 из 120 сотрудников в Singapore, обнуление китайских соцсетей, блокировка китайских IP, разрыв связей с Tencent). HeyGen — перенос в LA, выкуп китайских инвесторов (IDG, Sequoia China), роспуск шэньчжэньского юрлица. Статус Baidu USA (Sunnyvale) и Tencent America (Palo Alto) на 2026 не верифицирован в этом исследовании.

---

## Вердикт с уровнями уверенности и допущениями

**Ранжирование лабораторий по вероятности нанять Valley-based ecosystem/DevRel-лида в ближайшие 6–12 месяцев:**

1. **Alibaba/Qwen — наиболее вероятно (confidence: средняя).** Есть US-инфраструктура (SV с 2015), деньги, амбассадорская программа (qwen.ai/ambassador с 11 мая 2026) и зияющая дыра после ухода Junyang Lin (по X-посту «me stepping down. bye my beloved qwen», 3 марта 2026, вместе с Binyuan Hui и Kaixin Li; уход, по Simon Willison, спровоцирован реорганизацией с постановкой ex-Google Gemini-найма над Qwen). НО: культура становится продукт-центричной, публичная роль не заполнена, и это скорее найм community/ambassador-manager (возможно ремоут), чем «звезды в SF». Допущение: open-weight-стратегия сохранится.
2. **MiniMax — вероятно (средняя).** Singapore-домициль снимает часть риска, есть реальный US-штат (~9%, оценка Revelio) и была SF-вакансия GTM Manager. Консюмер-продукты (Talkie/Hailuo) требуют западного маркетинга. Наиболее «нанимаемый» кандидат из-за чистой юрисдикции.
3. **Moonshot/Kimi — возможно (средняя).** Kimi Global Ambassador Program (с 29 июля 2026) + AWS Bedrock + доллар-выручка. Но ставка на амбассадоров/ремоут; риск Entity List. HK IPO ($3–5B, конфиденциальная заявка) может подтолкнуть к найму «международного лица», но, вероятно, не в SF.
4. **ByteDance/BytePlus — возможно, но через Singapore (средняя).** Ресурсы огромны, но наследие TikTok/CFIUS толкает всё международное в Singapore/London, не в SF.
5. **Z.ai/Zhipu — маловероятно для США (высокая).** Уже строит экосистему (Zixuan Li), но Entity List и обвинения в дистилляции делают US-найм токсичным; фокус на sovereign-AI вне США (SE Asia, Middle East, Africa).
6. **DeepSeek — крайне маловероятно (высокая).** Идеологически research-first, без DevRel/GTM-функции вообще.

**Итог:** Гипотеза **держится** для Alibaba/Qwen и MiniMax (при условии Singapore/ремоут-структуры, а не US-юрлица); **держится частично** для Moonshot и ByteDance (через не-US хаб); **проваливается** для Z.ai (Entity List) и DeepSeek (нет функции).

---

## Идеальный профиль кандидата

- **Язык:** свободный английский + китайский (mandarin) — критично для внутренней координации с командой в Китае.
- **Локация/виза:** для минимизации риска — **не US-юрлицо**; предпочтительны US-резидент/гражданин, работающий как contractor (1099) или через Singapore-entity, либо базирующийся в Singapore. Прецеденты: Zixuan Li базируется в Cambridge, MA, но представляет Z.ai; MiniMax держит US-штат под Singapore-юрлицом.
- **Технический уровень:** способность писать docs/samples, вести бенчмарки, работать с Claude Code / Cline / Cursor / vLLM / SGLang, понимать механику coding-планов.
- **Сеть:** связи с inference-провайдерами (Together, Fireworks, DeepInfra, Baseten, OpenRouter), IDE-командами, Hugging Face, организаторами AI Engineer / NeurIPS / ICML.
- **Track record:** предыдущий DevRel/ecosystem/partnerships в dev-tools или AI-компании; публичное присутствие на X/GitHub.

**Компенсация (бенчмарки 2025–2026):** медиана DevRel base ~$175K (10th Annual State of DevRel Report), total ~$200K; Head of DevRel / Director $190K–$280K (в топ-компаниях — $315K–$435K, как в вакансиях Anthropic); SF Developer Relations Manager avg ~$210K (Salary.com, август 2026). Для китайской лаборатории через contractor/advisor — дневные/месячные ставки; equity в китайском юрлице проблематична (нет ликвидности до HK IPO; для US-лиц — санкционные/OFAC-риски по Entity-List-компаниям). Fractional advisor: типично $5K–$20K/мес ретейнер + upside.

---

## Actionable Implications — таргет-лист для кандидата/адвайзора

**Кого таргетить первым и почему:**

1. **MiniMax (первый приоритет).** Singapore-домициль = чистая юрисдикция, есть US-штат, консюмер-продукты требуют западного GTM. Заходить через Singapore-entity как contractor. Была вакансия GTM Manager SF — значит, аппетит есть.
2. **Alibaba/Qwen (второй).** Дыра после Junyang Lin, есть US-инфра и амбассадорская программа (qwen.ai/ambassador — Dev + Event треки). Начать с Dev Ambassador-трека, конвертировать в оплачиваемую роль. Точка входа — команда Model Studio / институциональный аккаунт @Alibaba_Qwen.
3. **Moonshot/Kimi (третий).** Kimi Global Ambassador Program (kimi.com/lp/kimi-ambassador, forum.moonshot.ai) — низкий барьер входа. Тайминг: вокруг релизов K3 / K3-256K и HK IPO.
4. **Z.ai — только вне США.** Если кандидат готов работать по не-US направлениям (SE Asia, Middle East). Точка входа — **Zixuan Li** (Head of Z.ai Global Ecosystem, активен на X).

**Что подчёркивать в питче:**
- «Я снимаю ваш главный дефицит — не модель, а *distribution и trust* на западном dev-рынке, без создания токсичного US-юрлица».
- Конкретные proof points: доведённые интеграции с Cursor / Cline / Claude Code, метрики роста комьюнити, связи с inference-провайдерами, опыт запуска coding-планов/хакатонов.
- Тайминг: питчить за 4–6 недель до релиза модели или IPO-события. Zhipu уже листингована в HK (январь 2026, 2513.HK), MiniMax листингована 9 января 2026 (HKEX), Moonshot готовит HK IPO $3–5B.

**Риски для кандидата:**
- **Репутационный:** публичное представление Entity-List-компании (Z.ai) или бренда, обвинённого в дистилляции Claude, может закрыть будущие двери в US-labs.
- **Юридический/санкционный:** работа на/оплата от Entity-List-компании — OFAC/BIS-риски; отправка «controlled technology» в китайский API — экспортный контроль (по анализу NatLawReview/Sheppard Mullin).
- **Payment/entity-риск:** оплата из китайского юрлица, equity без ликвидности, валютный контроль. Требовать оплату через Singapore/HK-entity в USD.

---

## Caveats

- Ни одна лаборатория первого эшелона **публично не подтвердила** план открыть DevRel-офис в Bay Area; вывод о «безофисной» экспансии основан на отсутствии доказательств офисов + модели дистрибуции.
- Данные о штате MiniMax в США (~9%) — оценка Revelio Labs, не корпоративное раскрытие.
- Вакансия MiniMax GTM Manager SF — из агрегатора (RevPath/DealHub), оригинальный LinkedIn-листинг не захвачен, роль закрыта.
- Cara Li (Z.ai) — единичный источник (LinkedIn-пост); её точная роль не подтверждена.
- Статус Baidu USA (Sunnyvale) и Tencent America (Palo Alto) на 2026 не верифицирован.
- GTG-коды и объёмы в отчёте о дистилляции Claude имеют расхождения между источниками (Tech Times против сводки enricher по первичному отчёту Anthropic); первичный 154-страничный отчёт Anthropic напрямую не прочитан.
- Ряд цифр OpenRouter — из вторичных блогов, но подтверждены перекрёстно первичными (CNBC 7 июля 2026, Bloomberg, Yahoo Finance) и данными Vercel AI Gateway (июнь 2026: китайские open-weight = 29% токенов, <4% расходов; DeepSeek 22,6%).

[TEXT]
Your report on Chinese LLM labs in Silicon Valley and the case for a local Ecosystem/DevRel lead is ready. It evaluates the hypothesis (verdict: PARTIALLY true, MEDIUM strength) and identifies the top 5 labs to target—MiniMax, Z.ai/Zhipu, Moonshot/Kimi, Alibaba Qwen, and ByteDance/BytePlus—with named contacts, the best-fit role for each, and warm-intro paths. It maps demand signals (indie/startup adoption vs. ~1% enterprise usage), regulatory barriers (Entity List, state bans, Hawley's S.321), and hiring precedents. For Anton specifically, it covers honest positioning, title/compensation benchmarks ($200–350K+), engagement structures (contractor via Singapore/HK entities, EOR), a risk matrix with mitigations, tailored outreach messages for all five labs, a 30-day sequence, a "Valley ecosystem package" deliverable, and staged recommendations with review thresholds. Key caveats on future-dated and single-source items are flagged throughout.

[ARTIFACT Chinese LLM Labs in Silicon Valley: Do They Need a Local Ecosystem and DevRel Lead cmd=create]
# DR26-09-23-ZB-01-0631 — Китайские LLM-лаборатории в Кремниевой долине: нужен ли им местный Ecosystem/DevRel-лид?

## TL;DR
- **Гипотеза верна ЧАСТИЧНО (оценка СРЕДНЯЯ, со сдвигом к сильной для узкой группы лаб).** Спрос на зарубежную видимость документирован (Z.ai: рост зарубежных платящих пользователей в 10 раз за два месяца до ~100 тыс. месячных API-пользователей и 3 млн бесплатных чат-пользователей после GLM-4.6, SCMP, ноябрь 2025; Qwen: 3 млрд загрузок против 418 млн у Google и 227 млн у Meta, Hugging Face, 14.08.2026), но нужен лабам не «фронтмен» (эту роль играют китайские техлиды), а **локальный организатор комьюнити и BD-мост к инженерам и инвесторам**.
- **Целиться стоит в 5 лабораторий:** MiniMax (уже есть US-based Head of DevRel Ryan Lee в SF), Z.ai/Zhipu (Li Zixuan, head of global operations; работать только через сингапурскую структуру из-за Entity List), Moonshot/Kimi (Kimi Global Ambassador Program), Alibaba Qwen (после ухода Junyang Lin в марте 2026 нет публичного лица для западного сообщества) и ByteDance/BytePlus (крупнейшее физприсутствие в США, но нет именованного US DevRel-лида).
- **Роль реальна, но нишевая и рискованная.** Не найдено ни одной открытой US DevRel-вакансии у этих пяти лаб; спрос сконцентрирован в indie/стартап-сегменте (не enterprise: китайские open-source = ~1% enterprise API-usage, Menlo Ventures, дек 2025). Ключевые риски для Anton — экспортный контроль/Entity List (высокий при прямой работе с listed-юрлицом), репутация для будущих US-лаб и иммиграционный статус. Оптимальная структура: контрактор/эдвайзер через сингапурскую или HK-структуру, не через китайское юрлицо.

---

## Вердикт по гипотезе (развёрнуто)
Гипотеза подтверждается ЧАСТИЧНО. Спрос на зарубежную видимость и adoption у разработчиков документирован и растёт, но тезис «Valley-оператор — дефицитный актив прямо сейчас» верен НЕ для всех лаб: (1) часть сознательно избегает США — Moonshot заявил об отсутствии планов по зарубежным продуктам; DeepSeek не имеет никакой зарубежной структуры и go-to-market; MiniMax юридически исключил США из лицензии на H3; (2) US-присутствие тормозится реальными барьерами (Zhipu/Z.ai в Entity List с января 2025; десятки штатов забанили DeepSeek; законопроект Хоули); (3) большинство работает через open-weights + сторонний хостинг (Cerebras, Together, Fireworks, Vercel, AWS Bedrock), что снижает нужду в собственном штате. Реальную нишу создают лишь те, кто одновременно (а) хочет роста в США, (б) имеет юридически чистую международную структуру (Сингапур) и (в) уже строит developer-функцию: **MiniMax, Z.ai, Moonshot/Kimi, Alibaba Qwen и ByteDance/BytePlus**. Для них Anton — не «лицо компании», а локальный door-opener к инженерам и инвесторам, организатор мероприятий и BD-мост.

## Топ-5 лабораторий: лучшая роль и лучший человек для контакта
1. **MiniMax** — роль: *Developer Ecosystem Lead, Americas* (контрактор/эдвайзер под/рядом с DevRel). Контакт: **Ryan Lee, Head of Developer Relations** (X @RyanLeeMiniMax; GitHub RyanLee-Dev; базируется в SF). Единственная из пяти лаб с уже существующим US-based DevRel-лидером.
2. **Z.ai (Zhipu)** — роль: *US Developer Community / Events Lead* через сингапурскую структуру **Jingsheng Hengxing Technology Pte. Ltd.** (НЕ через китайское юрлицо из-за Entity List). Контакт: **Li Zixuan, head of global operations** (учился в CMU).
3. **Moonshot AI / Kimi** — роль: *US Ambassador/Community organizer* поверх Kimi Global Ambassador Program. Контакт: @Kimi_Moonshot / [email protected]. Осторожно: заявлен отказ от зарубежных продуктов.
4. **Alibaba Qwen** — роль: *Developer Community Advocate, Qwen (Americas)*. После ухода Junyang Lin (@JustinLin610, 03.03.2026) у Qwen нет публичного лица для западного сообщества — очевидное окно влияния. Контакт: через Alibaba Cloud US (Sunnyvale) / @Alibaba_Qwen.
5. **ByteDance / BytePlus / Volcano Engine** — роль: *Developer Relations, BytePlus (US)*. Крупнейшее физприсутствие в США (San Jose, Mountain View), международная структура через BytePlus Pte Ltd (Сингапур). Именованного US DevRel-лида не найдено — потенциальная точка входа.

---

# Key Findings

1. **Спрос на overseas-рост реален и растёт, но у indie-разработчиков, не в enterprise.** OpenRouter: китайские модели забрали 34,25 трлн из 69 трлн токенов (3–9 августа 2026), обгоняя US-модели 15 недель подряд. Но Menlo Ventures (09.12.2025): «Enterprise adoption of open-source LLMs fell nearly 50%, from 19% to 11%, with organizations remaining cautious toward Chinese models»; китайские open-source ≈ 1% всего enterprise API-usage. Тройка Anthropic 40% / OpenAI 27% / Google 21% = 88% enterprise. → Аудитория, к которой у Anton есть доступ (indie/стартап-инженеры), — именно та, где китайские модели растут.
2. **Лабы строят developer-функцию, но почти без публичных US-вакансий.** Не найдено ни одной подтверждённой открытой US DevRel/ecosystem/partnerships вакансии у пяти лаб. Они опираются на отдельных лидеров (Ryan Lee, MiniMax), ambassador-программы (Kimi) и спикеров (Li Zixuan, Z.ai).
3. **Регуляторные барьеры структурируют, кто вообще может нанимать в США.** Zhipu/Z.ai и SenseTime — в Entity List; работа с ними возможна только через сингапурскую структуру. DeepSeek забанен в десятках штатов и не строит GTM в принципе.
4. **Прецедент DevRel-найма у китайских фирм почти отсутствует.** Они нанимают senior US-лидеров в policy/marketing/BD/product (ByteDance: Mayer, Jhaveri, Ojakli; Alibaba: Evans), но не в DevRel. Ближайшие DevRel-аналоги — открытый Head of Developer Experience у HeyGen (китайского происхождения, релокация в LA) и уже нанятый Ryan Lee у MiniMax.
5. **Свидетельства ПРОТИВ гипотезы существуют и важны.** Moonshot заявил об отсутствии планов по зарубежным продуктам; MiniMax юр. исключил США/ЕС/UK/Korea из лицензии H3 (02.08.2026); DeepSeek редомициляции избегает и не имеет sales-организации; Moonshot демонтировал офшорный холдинг ради IPO в Гонконге (редомициляция в КНР).

---

# Часть A. Верна ли гипотеза?

## A1–A2. Карта лабораторий: экспансия в США и сигналы спроса

| Лаборатория | Международная структура | US-офис | Признаки US/DevRel-активности | Именованные ecosystem-люди | Барьеры |
|---|---|---|---|---|---|
| **DeepSeek** | Нет зарубежной структуры (CONFIRMED). Первый внешний раунд ~$7,4 млрд при оценке >$52 млрд (Caixin, июль 2026) — на удержание талантов, НЕ на GTM | Нет | Открытые веса MIT; нет sales-организации | Нет | Баны штатов (Texas 31.01.2025, NY 10.02.2025, Virginia 11.02.2025 и др.); HR1121; Anthropic: >12 млн suspected distillation-атак с DeepSeek за 14 дней июля 2026 |
| **Zhipu / Z.ai** | Международный сервис через **Jingsheng Hengxing Technology Pte. Ltd. (Сингапур)**, «zero data retention»; листинг HK янв 2026 | Нет | Рост зарубежных платящих в 10× за 2 мес → ~100 тыс. месячных API + 3 млн бесплатных (SCMP, ноя 2025); GLM-4.6 в Claude Code; Vercel добавил GLM-4.6; Cerebras продвигает GLM-4.6 с 05.11.2025 | **Li Zixuan** (head of global operations), CMU | **В Entity List с 16.01.2025** — критический барьер |
| **Alibaba Qwen** | Alibaba Cloud US (Sunnyvale/San Mateo) | Есть | **3 млрд загрузок** vs 418 млн Google / 227 млн Meta на Hugging Face (14.08.2026); >460 моделей, 300 тыс. деривативов; 20%+ Qwen3-Coder на OpenRouter (авг 2025); хостинг Cerebras (US), Together | **Junyang Lin (@JustinLin610)** ушёл 03.03.2026; преемник не назначен | FT: движение к проприетарности после ухода лидеров; закрытие free-tier Qwen Code |
| **Moonshot / Kimi** | Демонтировал офшорный холдинг ради IPO HK (Q3 2026) → редомициляция в КНР | Нет | **Kimi K3 на AWS Bedrock** (21.09.2026) — по Silicon UK, «the first major revenue-sharing agreement between a Chinese AI firm and a major US cloud company»; Reuters: переговоры с AWS/Azure/Google Cloud о доле до 30%; **ARR $1 млрд (авг 2026)** vs $300 млн в июне; **Kimi Global Ambassador Program**; Discord | @Kimi_Moonshot; именованного лида нет | **Заявлен отказ от зарубежных продуктов**; концентрация выручки в КНР |
| **MiniMax** | Cayman-холдинг; листинг HK янв 2026 (+109% в 1-й день); >212 млн польз. из 200+ стран (сен 2025) | Ryan Lee в SF | Уже есть **US-based Head of DevRel**; модели на OpenRouter | **Ryan Lee, Head of Developer Relations** (X @RyanLeeMiniMax, GitHub RyanLee-Dev, SF, HKUST) | **Лицензия H3 (02.08.2026) исключает US/EU/UK/Korea** из-за копирайт-исков |
| **ByteDance / BytePlus / Seed** | BytePlus Pte Ltd (Сингапур); Seed-лаборатории в КНР, Сингапуре, США | **San Jose, Mountain View** (285+ вакансий на Indeed) | Seed team рекрутирует в США (2027 start); BytePlus (ModelArk, Seedance/Seedream); Discord/YouTube/LinkedIn | Именованного US DevRel-лида не найдено | Наследие CFIUS/TikTok-давления |
| **Прочие** (StepFun, 01.AI, Baichuan, Tencent Hunyuan, Baidu ERNIE, iFlytek, SenseTime, Xiaomi MiMo, Meituan LongCat) | В осн. только КНР; SenseTime в Entity List | Tencent America (Palo Alto), Baidu USA (Sunnyvale) — сокращаются | Xiaomi MiMo v2 — closed-source; LongCat/StepFun — open-weights без US-GTM | Не найдено | Baidu крупные увольнения (ноя 2025, до 40% в юнитах); Alibaba −34% штата в 2025 |

**Сводные сигналы спроса:**
- Cursor Composer 2 (март 2026) построен поверх Kimi K2.5 (не раскрывалось до утечки на X); Windsurf/Vercel/Cerebras/Together/Featherless хостят китайские модели.
- Reflection AI (Misha Laskin, оценка $8 млрд, ноя 2025) создана как «американская open-source альтернатива» китайским моделям — сам факт подтверждает их проникновение в Долину.
- Hugging Face (14.08.2026): Qwen — «одна из крупнейших основ open-AI-экосистемы… часть дефолтного воркфлоу разработчиков».

## A3. Барьеры
- **Экспортный контроль / Entity List:** Zhipu/Z.ai (16.01.2025) и SenseTime — в списке; ограничивает доступ к US-технологиям и делает прямой US-найм токсичным.
- **Госбаны:** Texas (31.01.2025), NY (10.02.2025), Virginia (11.02.2025) и др.; HR1121 (No DeepSeek on Government Devices Act, внесён 07.02.2025); NASA/ВМС.
- **Законопроект Хоули S.321** (Decoupling America's AI Capabilities from China Act, внесён 29.01.2025): запретил бы US-persons «advancing AI capabilities» в КНР (поправки к Title 18). Пока не принят, без со-спонсоров — но задаёт вектор.
- **Data-residency / National Intelligence Law 2017:** hosted-API маршрутизирует данные через КНР; митигируется сингапурским хостингом (Z.ai «zero data retention») или self-hosting open weights.
- **Доверие / «China-shedding»:** редомициляции в Сингапур (Manus/Butterfly Effect).
- **Кто справляется лучше:** Z.ai (сингапурская структура + IR-инфраструктура после IPO, отвечает СМИ в отличие от DeepSeek), MiniMax (Cayman + US-based DevRel), ByteDance (физприсутствие + сингапурский BytePlus).

## A4. Вердикт
**СРЕДНЯЯ сила гипотезы.** Держится для MiniMax, Z.ai, Qwen, ByteDance/BytePlus и отчасти Moonshot. НЕ держится для DeepSeek (принципиально не строит GTM), Moonshot на уровне продуктов, MiniMax на уровне лицензий. Ключевая правка: лабам нужен не «локальный фронтмен» (эту роль играют китайские техлиды на X), а **локальный организатор комьюнити/мероприятий и BD-мост**.

---

# Часть B. Карта: куда именно целиться

## B5. Открытые/недавние US-роли (DevRel/ecosystem/community/BD)
**Ключевой негативный вывод:** не найдено НИ ОДНОЙ явно подтверждённой открытой US-based DevRel/ecosystem/partnerships вакансии ни у одной из пяти лаб.
- **MiniMax:** подтверждённых US DevRel-вакансий нет; есть уже нанятый Ryan Lee (SF).
- **Z.ai:** нет (осложнено Entity List). Есть «Z.ai Startup Program» (@zdotai).
- **Moonshot/Kimi:** careers.kimi.ai; US DevRel-вакансий нет; есть Ambassador Program.
- **Qwen/Alibaba Cloud:** US-листинги есть, но это инженерия/solutions, не model-DevRel.
- **ByteDance/BytePlus:** специфичной US DevRel-вакансии не найдено; много инженерных в San Jose.
- **Прямой прецедент-аналог:** **HeyGen** (китайского происхождения, релокация в LA) открыл **Head of Developer Experience** (Greenhouse), требования «7–10 лет developer experience/marketing/relations».

## B6. Именованные люди (decision makers / warm-intro targets)
- **Ryan Lee** — Head of Developer Relations, MiniMax. X @RyanLeeMiniMax, GitHub RyanLee-Dev, LinkedIn in/ryanlee-dev, SF, HKUST. Предыдущий работодатель не подтверждён.
- **Li Zixuan (Zixuan Li)** — Head of Global Operations, Z.ai; учился в CMU. Публичный хэндл не подтверждён.
- **Junyang Lin (@JustinLin610)** — ex-техлид Qwen, ушёл 03.03.2026, теперь независимый исследователь; преемник по сообществу не назначен. Ушли также Yu Bowen (→ заменён Zhou Hao, ex-DeepMind), Binyuan Hui (→ Meta).
- **Moonshot/Kimi:** @Kimi_Moonshot, [email protected], forum.moonshot.ai; именованного DevRel-лида нет.
- **ByteDance/BytePlus:** именованного US DevRel-лида не найдено.
- **Внешние союзники (НЕ сотрудники лаб):** Tiezhen Wang (Head of APAC Ecosystem, Hugging Face), Yuchen Jin (CTO, Hyperbolic) — публично поддерживают Qwen.

## B7. US-посредники-мосты
- **Инференс/хостинг:** Cerebras (Qwen3, GLM-4.6 из US data-centers, zero data retention), Together AI (Qwen3-Coder), Fireworks, Featherless (Kimi K2), Vercel (GLM-4.6), OpenRouter, AWS Bedrock (Kimi K3), Hyperbolic (Yuchen Jin).
- **Экосистема HuggingFace:** Tiezhen Wang (APAC ecosystem).
- **Медиа/аналитика:** Artificial Analysis, SemiAnalysis, Interconnects (китайские лабы сами на них ориентируются), ChinaTalk.
- **VC-мосты:** HongShan (ex-Sequoia China), IDG, Hillhouse — инвесторы Zhipu/MiniMax/Moonshot.

## B8. Прецеденты найма западных/Valley-людей китайскими фирмами (2023–2026)
- **HeyGen** (LA): открытый Head of Developer Experience — ближайший DevRel-прецедент (роль открыта, найм не назван).
- **MiniMax:** Ryan Lee (SF) — фактически уже реализованный US DevRel-прецедент.
- **ByteDance/TikTok (senior US, не DevRel):** Kevin Mayer (COO ByteDance / CEO TikTok, 2020, ушёл ~через 3 мес); Kate Jhaveri (Global Head of Marketing, ~2022); Ziad Ojakli (Head of Public Policy Americas, с 01.12.2025); Sean Kim (ex-Amazon, US Head of Product incl. TikTok for Developers, ~2019–2024).
- **Alibaba:** J. Michael Evans (President, 2015) — BD/международный, не DevRel.
- **Manus/Butterfly Effect:** релокация КНР→Сингапур, найм 20+ в Сингапуре; таргет Северной Америки; сообщения о поглощении Meta (не подтверждено Reuters/Bloomberg).
- **DJI, Xiaomi, Kuaishou/Kling, Tencent America, Baidu USA, Huawei/Futurewei:** именованных US DevRel-наймов не найдено.
- **Урок:** китайские фирмы нанимают senior US-лидеров в policy/marketing/BD/product, но почти НЕ в DevRel; текучка высокая (Mayer), риск CFIUS/политического давления.

---

# Часть C. Позиционирование Anton

## C9. Сильнейшее честное позиционирование
Anton — **нанятый исполнительный руководитель с опытом COO/CTO и практикующий программист (Python, C++)**, PhD по developer education, годами ведущий cohort-программы, хакатоны и обучение разработчиков, живущий в Пало-Альто фул-тайм. Продаваемый актив: **он — человек в Долине, который знает инженеров (founding/ML-инженеры, open-source-мейнтейнеры) и умных AI-инвесторов и может стать локальным door-opener и организатором комьюнити для лаборатории, у которой нет доверенного местного оператора**. Не «фронтмен модели» (это делают китайские техлиды), а мост к людям и мероприятиям. Никаких недокументированных цифр; сеть описывается как «curated», без ложных метрик. Слово «founder» не используется.

## C10. Тайтлы, компенсация, структура
- **Тайтлы (запрашивать):** Head of Developer Ecosystem, US; Developer Community Lead, Americas; Head of Partnerships, US; Ecosystem BD Lead. (Не «Head of DevRel» там, где уже есть Ryan Lee — там *Ecosystem/Community Lead, Americas* под ним или рядом.)
- **Компенсация (Bay Area, 2025–2026):** реалистично **$200–350K+ total** (base $150–250K + equity/бонус). Ориентиры: Glassdoor Head of DevRel avg ~$153K, диапазон $118–203K, топ до ~$368K; реальные постинги $200–300K base (Storm2); senior DevRel IC total на уровне Nvidia/Block $330–580K; медиана Developer Advocate в Bay Area ~$214K (Levels.fyi). Агрегаторные «средние» ($100–154K) занижают leadership/AI-tier. Для контрактора через иностранную структуру — эквивалентная дневная/месячная ставка.
- **Структура (от наименее к наиболее рискованной для Anton):** (1) **эдвайзер + equity** через офшорный холдинг; (2) **контрактор через сингапурскую/HK-структуру** (Z.ai — Jingsheng Hengxing Pte; BytePlus Pte) с W-8/W-9, оплата в USD; (3) **сотрудник US-субсидиары** (реже; у большинства лаб её нет). EOR (Deel/Rippling/Remote) — практичный компромисс. **Избегать** прямого контракта с юрлицом из Entity List (Zhipu-КНР) — работать только через сингапурскую структуру.

## C11. Риски для US-резидента (рейтинг и митигации)

| Риск | Уровень | Суть | Митигация |
|---|---|---|---|
| Экспортный контроль (EAR/BIS, «deemed export») | **Средний** для нетехнической роли; **Высокий** при работе с Entity List-юрлицом | Прямая работа с Zhipu-КНР рискованна; передача US-технологий/ПО | Только сингапурская структура; никакой передачи controlled tech; юрист по экспортконтролю |
| OFAC/Entity List близость | **Средний** | Zhipu, SenseTime в списке | Не подписывать контракт с listed entity; due diligence контрагента |
| Законопроект Хоули (S.321) | **Низкий сейчас, растущий** | Пока не принят; криминализовал бы «advancing AI in PRC» | Мониторинг; роль = маркетинг/комьюнити в США, не R&D в КНР |
| FARA | **Низкий** | Применим при лоббировании/полит. деятельности от имени иностранного принципала | Не вести госвзаимодействие/лоббизм; чистый developer-marketing |
| CFIUS | **Неприменим** к физлицу | — | — |
| Репутация для будущих US-лаб (Anthropic/OpenAI/Google) | **Средний-Высокий** | Публичная позиция US-лаб против китайских моделей (Anthropic-отчёт о distillation) | Позиционировать как open-source/ecosystem-работу; нейтралитет; прозрачность |
| Иммиграция | **Зависит от статуса** | На визе (H-1B/O-1) работа на иностранного работодателя проблематична; при GC/гражданстве — свободнее | Предполагается GC/гражданство; при визе — только US-EOR или эдвайзерство |
| Банкинг/налоги | **Низкий-Средний** | Оплата от иностр. юрлица: 1099/1042, W-8/W-9, FBAR при иностр. счетах | Бухгалтер по международному налогу; USD на US-счёт; FBAR при необходимости |

## C12. Outreach-плейбук для топ-5 (первые сообщения на английском, <120 слов)

**1. MiniMax → Ryan Lee (Head of DevRel).** Канал: X DM @RyanLeeMiniMax (он просит не писать в LinkedIn); артефакт: план хакатона на MiniMax M-модели + Palo Alto engineer dinner.
> Ryan — I run a small AI research lab in Palo Alto and I've spent years running developer cohorts and hackathons in the Valley. I know a lot of founding and ML engineers here, plus a curated group of AI-focused angels and VCs. You're MiniMax's DevRel in SF; I'd like to be your local ground game. Concretely: a monthly Palo Alto engineer dinner around MiniMax models, one hackathon on M-series, and an integration write-up I'd publish first. I'm a hired operator (COO/CTO background) and a programmer, not looking to be a founder — just to make MiniMax the model Valley builders reach for. 20 minutes this week?

**2. Z.ai → Li Zixuan (Head of Global Operations).** Канал: email/LinkedIn через сингапурскую структуру; артефакт: investor briefing dinner + GLM-4.6 integration demo.
> Zixuan — congrats on the 10x overseas growth after GLM-4.6. I'm a Palo Alto-based operator (COO/CTO track, programmer) running a small AI lab, and I've run developer cohorts and hackathons here for years. GLM is winning indie developers but has no local presence with Valley engineers and investors — I can be that bridge, contracted through your Singapore entity given the Entity List situation. First deliverables: a GLM coding-agent demo I publish, a Palo Alto engineer meetup, and a small AI-investor briefing dinner on Z.ai's roadmap. I don't need a title fight — I need a clear mandate and metrics. Worth a call?

**3. Moonshot/Kimi → @Kimi_Moonshot / [email protected].** Канал: email + X; артефакт: заявка стать US anchor Ambassador + план кампании.
> Team — the Kimi Ambassador Program is the right instinct; it needs a Valley anchor who already knows the engineers and the investors here. I'm a Palo Alto operator (COO/CTO background, programmer) who's run developer cohorts and hackathons for years. I'd run Kimi's US ground game: a recurring Palo Alto builder dinner, one K3 hackathon, and integration guides I publish for Cursor/Cline/Kilo users. I understand you're cautious about overseas products — this is developer community, not a product launch. Happy to start as a contractor or lead ambassador and prove it in 30 days. Can we talk?

**4. Alibaba Qwen → @Alibaba_Qwen / Alibaba Cloud US.** Канал: X + Alibaba Cloud US (Sunnyvale); артефакт: «Qwen in the Valley» meetup + benchmark write-up.
> With Junyang stepping down, Qwen has no public face for the Western developer community even as it passes 3B downloads. I'm a Palo Alto operator (COO/CTO track, programmer) who's run developer cohorts and hackathons here for years and knows the engineers and AI investors in the Valley. I'd run a "Qwen in the Valley" program: monthly engineer meetups, a hackathon on Qwen3-Coder, and independent benchmark write-ups I publish. Contracted via Alibaba Cloud US or Singapore, whichever is cleanest. Not a founder pitch — a hired ecosystem operator who can make Qwen the default open model for Valley builders. 20 minutes?

**5. ByteDance/BytePlus → BytePlus team (LinkedIn/Discord).** Канал: LinkedIn + BytePlus Discord; артефакт: developer newsletter + Seed/BytePlus integration demo.
> BytePlus has the models and a Singapore structure but no visible developer presence with Valley builders. I'm a Palo Alto operator (COO/CTO background, programmer) who's run developer cohorts and hackathons for years and knows founding engineers, ML engineers and AI investors here. I'd build BytePlus's US developer ground game: a US developer newsletter, a Palo Alto meetup series, and an integration demo on Seed/ModelArk that I publish. Contracted through BytePlus Pte in Singapore. I'm not pitching to be a founder — I want a clear ecosystem mandate and measurable adoption targets. Could we set up a call this week?

**30-дневная последовательность (для каждой из топ-5):**
- Дни 1–3: warm-intro через общего инженера/инвестора, если есть; иначе прямой DM/email выше.
- Дни 4–7: отправить proof-of-work артефакт (integration-демо или бенчмарк-write-up, опубликованный публично).
- Дни 8–14: звонок; предложить пилот «Valley ecosystem package» на 30 дней.
- Дни 15–25: провести 1 инженерный ужин + анонсировать хакатон; собрать метрики.
- Дни 26–30: отчёт с метриками (участники, интеграции, upvotes/downloads) → предложение о контракте/эдвайзерстве.

## C13. «Valley ecosystem package» (первый конкретный deliverable)
| Компонент | Что | Таймлайн | Оценка затрат | Метрика лаборатории |
|---|---|---|---|---|
| Monthly engineer meetup (Palo Alto) | 20–40 инженеров, демо модели | ежемесячно | $2–5K/событие | новые активные разработчики, интеграции |
| Хакатон на модели лаборатории | 1–2 дня, призы | 30–45 дней подготовки | $10–25K | проекты, downloads, соцохват |
| Investor briefing dinner | 8–15 AI-ангелов/VC | разово в квартал | $3–6K | пайплайн партнёрств/инвест-интереса |
| Integration guides | Cursor/Cline/Kilo/Vercel гайды | 2–3 недели | время Anton | активации API, doc-трафик |
| US developer newsletter | ежемесячно | старт за 2 недели | время + $100–300/мес | подписчики, CTR, сигнапы |

**Совокупно пилот 30–60 дней:** ориентировочно $20–45K прямых расходов + компенсация Anton; измеримый результат — рост активных US-разработчиков, интеграций и токенов/downloads по модели лаборатории.

---

# Recommendations (staged, с порогами пересмотра)

**Этап 0 (до старта, дни −7…0): юридический чек.** Подтвердить иммиграционный статус (GC/гражданство vs виза) и проконсультироваться с юристом по экспортконтролю. **Порог остановки:** если Anton на визе H-1B/O-1 — прямой найм иностранным работодателем исключён, только US-EOR или эдвайзерство. Никаких контрактов с юрлицами из Entity List (Zhipu-КНР, SenseTime) — только через сингапурские структуры.

**Этап 1 (дни 1–14): двойной заход в MiniMax и Z.ai.** MiniMax — потому что уже есть US-based начальник (Ryan Lee), самый низкий барьер входа. Z.ai — потому что публично заявляет, что «growth overseas is exceeding growth domestically», имеет чистую сингапурскую структуру и IR-инфраструктуру. Отправить оба сообщения + по одному опубликованному proof-of-work артефакту. **Порог продвижения:** ответ/звонок в течение 10 дней → переход к пилоту.

**Этап 2 (дни 15–30): Qwen как «окно влияния» + Kimi как ambassador-трек.** Qwen после ухода Junyang Lin не имеет западного фронтмена — предложить «Qwen in the Valley». Kimi — войти через Ambassador Program как US anchor (низкий риск, не требует признания зарубежного продукта). **Порог:** если MiniMax/Z.ai дали пилот — сфокусироваться на нём; Qwen/Kimi держать как параллельные опции.

**Этап 3 (дни 30–60): пилот ecosystem-package с одной лабой.** Запустить 1 meetup + анонс хакатона, зафиксировать метрики. **Порог перехода к постоянному контракту:** ≥2 новые интеграции/публичные проекты и измеримый рост активных US-разработчиков/downloads по модели за 30 дней.

**Что изменило бы стратегию:**
- Если S.321 (Хоули) или аналог **принимается** → немедленно свернуть любую работу, которую можно трактовать как «advancing AI in PRC»; пересмотреть все контракты.
- Если MiniMax/Z.ai **открывают формальную US DevRel-вакансию** → перейти от контрактора к переговорам о постоянной роли с equity.
- Если китайская доля в enterprise (сейчас ~1%) **растёт выше ~5%** по данным Menlo/следующих отчётов → расширить таргет на enterprise-BD, а не только indie-комьюнити.
- Если репутационный риск станет блокером для будущих US-ролей (Anthropic/OpenAI/Google) → ограничиться эдвайзерством с публичным open-source-фреймингом, без операционных титулов.

---

# Caveats
- **Спрос сконцентрирован в indie/стартап-сегменте, не в enterprise** (Menlo Ventures: китайские open-source ≈1% enterprise API-usage, дек 2025). Роль Anton монетизируется через комьюнити/adoption-метрики, а не enterprise-пайплайн.
- **Ряд источников датирован будущим** относительно горизонта данных (напр., Kimi K3 на Bedrock 21.09.2026, ARR-цифры Moonshot, лицензия H3) и/или основан на закрытых disclosures/непервичных агрегаторах — помечены соответствующе; финансовые проекции (Moonshot $2 млрд ARR к концу 2026) — это цели, не факты.
- **NOT FOUND / не проверено в этом цикле:** открытые US DevRel-вакансии у пяти лаб; предыдущий работодатель Ryan Lee; публичный хэндл Li Zixuan; именованный US DevRel-лид ByteDance/BytePlus; именованный преемник Junyang Lin; DevRel-прецеденты в DJI/Xiaomi/Kuaishou/Tencent America/Baidu USA/Huawei; точный comp-бенд «Head of Partnerships» Bay Area (Pave/Carta недоступны); отчёты CAISI/NIST по DeepSeek/Kimi.
- **Single-source флаги:** ARR Moonshot и revenue-share до 30% с US-облаками — по закрытым disclosures/Reuters через вторичные источники; поглощение Manus компанией Meta — не подтверждено Reuters/Bloomberg.
- **Оценки затрат/компенсаций** — рыночные ориентиры (Glassdoor/Levels.fyi/реальные постинги), не оферы конкретных лаб; финальные цифры зависят от структуры (контрактор vs EOR vs equity).