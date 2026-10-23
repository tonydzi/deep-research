# DR26-08-04-ZB-01-1437 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
id: DR26-08-04-ZB-01-1437
rail: chatgpt
rail_detail: codex exec --skip-git-repo-check — рельса аккаунта ChatGPT (GPT через Codex CLI), НЕ браузерное Deep Research App
captured: 2026-08-04
captured_by: AUTO-mac16-260731-dr-runner-daily

source: headless CLI (без браузера, приватного чат-URL нет)
---

Ниже — срез на **23:59 UTC 1 августа 2026 года**. Я разделяю:

- **независимые результаты** — публичный leaderboard с фиксированным harness;
- **self-reported** — цифры лаборатории из model card;
- **не подтверждено** — нет доступного официального прайса, model card или стабильного API-документа.

Материалы до 1 февраля 2026 года помечены как **исторический контекст**.

## Короткий вывод

Для лаборатории с бюджетом до $25/мес я бы выбрал:

1. **DeepSeek V4-Pro API** — лучший основной вариант: нативный Anthropic endpoint для Claude Code, 1M контекста, $0.435/$0.87 за 1M токенов, без подписки.
2. **Z.ai GLM-5.2 через Coding Plan Lite** — наиболее отличающийся «агентный архитектор», $18/мес; но тарифные коэффициенты и лимиты менее прозрачны.
3. **GLM-4.7-Flash или Qwen через международный Alibaba Model Studio** — резервный независимый маршрут с европейским/сингапурским endpoint; для Qwen flagship тариф дороже, но инфраструктурный и privacy-риск ниже, чем у прямого mainland API.

Если нужен один выбор: **DeepSeek V4-Pro pay-as-you-go**. При вашем бюджете подписка обычно экономически хуже: $25 у DeepSeek покрывают приблизительно **28.7M выходных токенов** либо **57.5M входных токенов без кэша** по официальному прайсу, опубликованному 24 апреля 2026 года.[DeepSeek pricing](https://api-docs.deepseek.com/quick_start/pricing?article_id=article_1779470751466_8)

---

# 1. Кто реально жив

## Первая группа: релизит и конкурентоспособен

| Лаборатория | Флагман на 01.08.26 | Релиз | Архитектура / размер | Доступ |
|---|---|---:|---|---|
| DeepSeek | V4-Pro / V4-Flash | 24.04.26 | MoE; точное число параметров в доступной официальной web-карточке не раскрыто; 1M context | API; веса и лицензия V4 в найденных первичных материалах не подтверждены |
| Alibaba | Qwen3.7-Max; Qwen3.8-Max Preview появился к 01.08, но финальный model card не найден | 20.05–08.06.26 для 3.7 | закрытый Max; 1M | API; младшие Qwen открытые, обычно Apache-2.0 |
| Moonshot | Kimi K3; до него K2.5/K2.6/K2.7-Code | июль 2026 | K3 — крупный MoE; точные параметры/лицензия требуют проверки model repository | API/подписка; спрос вызвал временную приостановку новых подписок |
| Z.ai/Zhipu | GLM-5.2 | 16.06.26 | 1M context, sparse attention/IndexShare | открытые веса, MIT, API, Coding Plan |
| MiniMax | M2.5; сообщения о M3 к июлю, но доступный официальный model page всё ещё указывает M2.5 | 12.02.26 | крупный MoE, agent/code focus | открытые веса, API, Coding Plan |
| ByteDance | Seed2.0 | 30.06.26 technical report | серия frontier multimodal/agent моделей | преимущественно закрытый API |
| Baidu | ERNIE 5.1 | 09.05.26 | параметры официальная страница релиза не раскрывает | закрытый API |
| Xiaomi | MiMo-V2.5-Pro | апрель–май 2026 | 1M, multimodal/agent | API; открытие весов было обещано, но статус надо проверять по конкретному checkpoint |
| Meituan | LongCat-2.0 | 30.06 / 12.07.26 | 1.6T total, ~48B active, 1M | открытые веса; заявлена permissive лицензия, но проверять LICENSE checkpoint |
| Tencent | Hy3-preview | весна 2026 | 295B total / 21B active | открытые веса, кастомная Tencent Community License |
| StepFun | Step 3.5 Flash | 12.02.26 | 196B MoE | открытые INT4/GGUF; API |
| InclusionAI/Ant | Ring-2.6-1T | лето 2026 | около 1T | открытый checkpoint; API-путь плохо документирован |

Источники: DeepSeek официально датирует V4 **24 апреля 2026 года** и предоставляет V4-Pro/Flash через API.[DeepSeek Transparency Center, 24.04.26](https://www.deepseek.com/en/transparency/) GLM-5.2 выпущен **16 июня 2026 года**, имеет 1M-контекст и лицензию MIT.[Z.ai model announcement, 16.06.26](https://z.ai/blog/glm-5.2) LongCat-2.0 — 1.6T MoE с примерно 48B активных параметров и 1M-контекстом; релиз весов опубликован **12 июля 2026 года**.[Meituan, 12.07.26](https://tech.meituan.com/2026/07/12/LongCat-2.0-Open-source.html) Seed2.0 model card опубликован **30 июня 2026 года**.[ByteDance Seed2.0, 30.06.26](https://arxiv.org/abs/2607.00248) ERNIE 5.1 официально выпущен **9 мая 2026 года**.[Baidu, 09.05.26](https://ernie.baidu.com/blog/posts/ernie-5.1-0508-release/)

MiMo-V2.5-Pro, обычный V2.5 и 1M-контекст объявлены Xiaomi весной 2026 года; старые V2 endpoint были окончательно выведены **30 июня 2026 года**.[Xiaomi documentation, обновлено 29.06.26](https://mimo.mi.com/docs/en-US) Tencent показывает Hy3-preview как 295B/A21B, но веса идут под кастомной лицензией, не MIT/Apache.[Tencent repository, опубликован весной 2026](https://github.com/Tencent-Hunyuan/Hy3-preview/blob/main/README_CN.md) Step 3.5 Flash — 196B, официальный релиз/обновление **12 февраля 2026 года**, включая INT4 GGUF.[StepFun, 12.02.26](https://static.stepfun.com/blog/step-3.5-flash/%3Fref%3Dtestingcatalog.com)

### Темп за последние шесть месяцев

**Ускорились:** Z.ai, Moonshot, DeepSeek, Xiaomi, Meituan, ByteDance и Qwen. Самый убедительный продуктовый сигнал у Qwen: Qwen Code выпускал обновления практически еженедельно в мае–июле — subagents, worktrees, memory, review verifier, steering и web tools.[Qwen Code changelog, обновлено 27.07.26](https://qwenlm.github.io/qwen-code-docs/en/blog/updates/)

**Активны, но не лучший выбор для общего code review:** Tencent Hunyuan сильно активен в 3D/world/OCR/translation, но его общий LLM/API менее удобен для международного headless workflow.[Tencent repositories, состояние на май 2026](https://github.com/orgs/Tencent-Hunyuan/repositories) Baidu и iFlytek остаются продуктовыми компаниями, однако по независимому международному agentic coding следу значительно слабее первой группы.

**Заглохли как frontier general-LLM игроки:** 01.AI, Baichuan. Я не нашёл подтверждённого post-February-2026 общего флагмана, публичного международного прайса и независимых результатов уровня первой группы. **Исторический контекст:** последний хорошо документированный открытый Baichuan общего назначения — Baichuan 2, 2023 год.[Baichuan technical report, 2023](https://cdn.baichuan-ai.com/paper/Baichuan2-technical-report.pdf)

Это не означает банкротство: скорее, они перестали быть практически значимым frontier API-кандидатом для вашей задачи.

---

# 2. Качество

## Самые полезные независимые числа

Сравнивать надо только одинаковый model + agent scaffold + reasoning budget. Цифры из model card и leaderboard нельзя ставить в одну колонку.

### Agentic coding

На официальном **Terminal-Bench 2.0 с Terminus 2**:

- GLM-5: **52.4% ±2.6**, прогон 23.02.26.
- DeepSeek-V3.2: **39.6% ±2.8**, прогон 10.02.26.
- Kimi K2 Thinking: **35.7% ±2.8**, исторический прогон 11.11.25.[Terminal-Bench leaderboard, данные по состоянию на июль 2026](https://www.tbench.ai/leaderboard/terminal-bench/2.0?agents=Terminus+2)

На TerminalWorld, который использует стандартизированный scaffold, но не идентичен Terminal-Bench:

- Kimi K2.6 — **57.5%**;
- GLM-5.1 — **57.0%**;
- Qwen3.6-Max Preview — **54.0%**;
- DeepSeek V4-Pro — **50.0%**.[TerminalWorld leaderboard, опубликован/обновлён в июне–июле 2026](https://terminalworld.ai/leaderboard/)

Roybench Terminal-Bench 2.1 показывает Kimi-K2.7-Code **84.1%** и DeepSeek V4-Pro **80.7%**, но GLM-5.2 там имеет неполный и явно несопоставимый маршрут — **33.0%**. Это хороший пример того, почему результат без полного provider/harness metadata нельзя трактовать как качество базовой модели.[Roybench snapshot, 25.06.26](https://www.roybench.org/)

### SWE-bench

Официальный SWE-bench Verified с mini-SWE-agent показывает для февральских моделей:

- Kimi K2.5 high reasoning — **70.8%**;
- DeepSeek V3.2 high reasoning — **70.0%**;
- оба прогона от **17 февраля 2026 года**.[SWE-bench leaderboard, 17.02.26](https://www.swebench.com/index.html?trk=public_post_comment-text)

MiniMax заявляет **80.2% SWE-bench Verified**, но это **self-reported**, опубликовано 12 февраля 2026 года; без идентичного публичного trajectory/harness я не считаю его доказанным превосходством над Claude.[MiniMax M2.5 model announcement, 12.02.26](https://www.minimax.io/news/minimax-m25)

Свежий SWE-rebench по нескольким языкам дал GLM-5.2 **62.9% ±1.19**, MiniMax M3 **47.2% ±1.13**, DeepSeek V4-Pro **40.2% ±1.29**; публикация результата — **26 июля 2026 года**, но первичный анонс находится в Reddit и должен рассматриваться как provisional до фиксации полного leaderboard artifact.[SWE-rebench announcement, 26.07.26](https://www.reddit.com/r/LocalLLaMA/comments/1v93phk/swerebench_multilingual_update_go_java_python/)

### Общая Arena

В открытом срезе Arena:

- DeepSeek V4-Pro Thinking — **1455 ±4 Elo**, 43 079 голосов;
- Qwen3.5-397B-A17B — **1442 ±4**, 58 157 голосов;
- GLM-4.7 — **1442 ±6**, 12 092 голосов.[Arena open-model leaderboard, опубликован 20.07.26](https://arena.ai/leaderboard/text?license=open-source)

Это показывает закрытие разрыва в обычном чате, но не доказывает равенство на автономном редактировании репозитория: Arena оптимизирует человеческое предпочтение к ответу, а не completion of long-horizon tasks.

### ARC-AGI-2

Китайские модели по-настоящему не закрыли разрыв: февральский независимый обзор приводил **11.8% для Kimi K2.5** против **84.6% Gemini 3 Deep Think**. Старые DeepSeek-R1 и Qwen3-235B были около **1.3%**; это исторические модели.[ARC-AGI-2 analysis, февраль 2026](https://yorozuipsc.com/uploads/1/3/2/5/132566344/9a25c2bb8b94bbacd7a4.pdf)

Вывод: «почти Claude» на SWE-bench не означает близость к Gemini Deep Think в задачах абстрактной индукции.

## Где разрыв закрылся

- **Локальные исправления, поиск по репозиторию, стандартный tool calling:** GLM-5.2, Kimi K2.6/2.7, DeepSeek V4-Pro и Qwen приблизились к frontier-моделям; разница часто меньше эффекта harness.
- **Стоимость одного дополнительного review:** китайские модели убедительно выигрывают.
- **Математика уровня AIME/HMMT:** model cards показывают насыщение в районе 90%+, но benchmark стал плохо разделять frontier-модели; результат сильно зависит от sampling budget. GLM публикует для GLM-5.2 значения вплоть до 92.5–97% на отдельных математических наборах, однако это self-reported.[GLM-5.2 model card, 16.06.26](https://z.ai/blog/glm-5.2)

## Где разрыв остался

- **ARC-AGI-2 и действительно новые абстрактные задачи**.
- **Длинные автономные изменения со скрытыми требованиями**, где важно не только найти фикс, но и не сломать архитектуру.
- **Agent safety и prompt-injection resistance**.
- **Русский стиль и нюанс:** публичного свежего независимого русского MRCR/long-form leaderboard для V4/GLM-5.2/Kimi K3 я не нашёл. Поэтому любые заявления «лучше Claude на русском» — **не подтверждены**.

На русском мой практический порядок ожиданий: Qwen ≈ GLM > DeepSeek/Kimi > MiniMax/StepFun. Это **аналитическая гипотеза**, а не измеренный leaderboard. Перед закупкой следует прогнать собственные 100–200 слепых code-review и strategy prompts на русском.

## Длинный контекст

DeepSeek V4 и GLM-5.2 заявляют **1M**, Alibaba Qwen3.7-Max также тарифицирует запросы до **1M**.[DeepSeek pricing, 24.04.26](https://api-docs.deepseek.com/quick_start/pricing?article_id=article_1779470751466_8) [GLM-5.2, 16.06.26](https://z.ai/blog/glm-5.2) [Alibaba pricing, обновлено 15.07.26](https://www.alibabacloud.com/help/en/model-studio/model-pricing)

Но «принимает 1M» не означает «стабильно использует 1M». Публичного одинакового независимого MRCR/needle-прогона для всех трёх на дату среза нет. Для production я бы ограничивал рабочую сессию **200–300K**, применял repository retrieval и периодически делал fresh-context review. Это рекомендация, не заявленный hard limit.

---

# 3. Контаминация и накрутка

Самый серьёзный документированный эпизод после февраля — не прямая утечка benchmark answers, а **массовая дистилляция**. Anthropic заявил, что DeepSeek, Moonshot и MiniMax использовали свыше **24 000 мошеннических аккаунтов и более 16 млн обменов** для извлечения возможностей Claude; публикация — **23 февраля 2026 года**.[Anthropic, 23.02.26](https://www.anthropic.com/news/detecting-and-preventing-distillation-attacks?via=free)

Оговорка: это заявление конкурента, а не судебно установленный факт. Anthropic публикует метод и агрегированные доказательства, но не полный сырой набор аккаунтов. Поэтому я считаю утверждение **сильным evidence of distillation campaign**, но не доказательством контаминации конкретного SWE-bench/GPQA вопроса.

Что заслуживает доверия:

1. Динамические или свежие наборы: SWE-rebench, LiveCodeBench с post-training задачами.
2. Прогоны с опубликованным agent scaffold, trajectories и commit hash.
3. Terminal-Bench при одинаковом агенте.
4. Слепая собственная выборка из ваших закрытых bugs/PR.

Что заслуживает меньше доверия:

- AIME/HMMT/GPQA из model card без contamination analysis;
- SWE-bench score лаборатории с кастомным agent scaffold;
- график, где собственная модель сравнивается с API конкурента при неизвестных effort/max-token настройках;
- LMArena как доказательство автономного coding quality.

Сам SWE-bench имеет методологические проблемы: исследование от 4 февраля 2026 года анализирует 79 Lite и 133 Verified submissions и показывает, что leaderboard смешивает качество модели, scaffold и patch validation.[“What’s in a Benchmark?”, 04.02.26](https://arxiv.org/abs/2602.04449)

---

# 4. Цены и подписки

## DeepSeek

Официальный прайс на дату среза:

| Модель | Cache hit input | Cache miss input | Output | Context | Concurrency |
|---|---:|---:|---:|---:|---:|
| V4-Flash | $0.0028 | $0.14 | $0.28 | 1M | 2500 |
| V4-Pro | $0.003625 | $0.435 | $0.87 | 1M | 500 |

Цены за 1M токенов; отдельной cache-write платы не указано.[DeepSeek pricing, опубликован 24.04.26](https://api-docs.deepseek.com/quick_start/pricing?article_id=article_1779470751466_8)

- Подписки coding-plan типа Claude Max не найдено.
- Web/app chat имеет бесплатный доступ, но это не API-квота.[DeepSeek home page, актуальна в июле 2026](https://deepseek.com/en/index.html)
- Международная оплата картой зависит от доступных методов конкретного аккаунта; публичной гарантии Visa/Mastercard для всех стран ЕС нет — **не подтверждено**.
- OpenRouter и другие посредники могут решить оплату, но их тариф — не официальный тариф DeepSeek.

## Z.ai / GLM

Coding Plan:

| План | Месяц | Годовой эквивалент | Объём |
|---|---:|---:|---|
| Lite | $18 | $12.60/мес при оплате за год | base allowance |
| Pro | $72 | $50.40/мес при оплате за год | 5× Lite |

Официальная страница не раскрывает base allowance простым числом токенов/запросов; поэтому точный лимит — **не подтверждено**.[Z.ai subscription page, актуальна в июле 2026](https://z.ai/subscribe?invitedialog=true)

На 31 июля действовала акция ZCode: GLM-5.2 расходовал квоту с коэффициентом 0.67 вне пика и 2× в пик вместо 1×/3×, то есть примерно 1.5× эффективного объёма. Акция заканчивалась **31 июля 2026 года**, поэтому на 1 августа её нельзя закладывать в бюджет.[ZCode documentation, июль 2026](https://zcode.z.ai/en/docs/welcome)

Доступный официальный PAYG-прайс был:

- GLM-5.1: $1.40 input / $0.26 cached / $4.40 output;
- GLM-5: $1.00 / $0.20 / $3.20;
- GLM-4.7: $0.60 / $0.11 / $2.20;
- GLM-4.7-FlashX: $0.07 / $0.01 / $0.40;
- GLM-4.7-Flash: бесплатно.[Z.ai pricing, страница захвачена в июне 2026](https://docs.z.ai/guides/overview/pricing)

Официальной строки PAYG для **GLM-5.2** в доступной версии прайса нет: цена **не подтверждена**. Это и есть continuity-риск.

## Alibaba/Qwen international

Qwen3.7-Max до 1M:

- list: $2.50 input / $7.50 output;
- на странице указана ограниченная скидка 50%, но срок применимости должен проверяться в консоли;
- cache creation обычно 125% обычного input, hit — 10% обычного input;
- free quota — 1M токенов, действительна 90 дней для International/Singapore.[Alibaba pricing, обновлено 15.07.26](https://www.alibabacloud.com/help/en/model-studio/model-pricing)

Coding Plan Pro стоит **$50/мес**, то есть не проходит ваш лимит. Он включает 6 000 requests/5h, 45 000/week и 90 000/month; одна сложная задача может потребить 10–30 model calls.[Alibaba Coding Plan, обновлено 11.06.26](https://www.alibabacloud.com/help/en/model-studio/coding-plan)

International Model Studio имеет регионы Singapore, Frankfurt, Tokyo, Virginia и Hong Kong; API keys, цены и model availability отличаются по регионам. DeepSeek, например, был доступен только в Beijing scope, тогда как Qwen работает международно.[Alibaba Model Studio overview, обновлено 10.07.26](https://www.alibabacloud.com/help/en/model-studio/what-is-model-studio)

Это лучший документированный вариант для Visa/Mastercard/международного биллинга, хотя профиль аккаунта требуется после исчерпания free quota.[Alibaba Model Studio overview, 10.07.26](https://www.alibabacloud.com/help/en/model-studio/what-is-model-studio)

## MiniMax

Официальная цена M2 на релизе была $0.30/¥2.1 input и $1.20/¥8.4 output; это **исторический контекст от октября–ноября 2025 года**, не гарантированный прайс M2.5/M3.[MiniMax M2 announcement, 2025](https://www.minimax.io/news/minimax-m2)

Актуальный официальный M2.5 page подтверждает API, автоматический cache и 50/100 TPS варианты, но не показывает точные цены в извлечённом тексте.[MiniMax M2.5 page, обновлён в 2026](https://www.minimax.io/models/text) Поэтому текущие M2.5/M3 PAYG и Coding Plan цены — **не подтверждены**.

## Xiaomi MiMo

MiMo поддерживает Lite/Standard/Pro/Max Token Plan и Stripe/Waffo для зарубежных пользователей; внутри Китая — Alipay, WeChat Pay или Xiaomi Pay.[Xiaomi FAQ, опубликован в июле 2026](https://mimo.mi.com/docs/en-US/quick-start/faq/token-plan)

Точные цены пакетов официальный FAQ отправляет смотреть на landing page и не фиксирует их; следовательно, любые цифры из обзоров — **не подтверждены**. Ночной коэффициент с 00:00 до 08:00 Beijing уменьшает расход Credits на 20%.[Xiaomi announcement, обновлено 29.06.26](https://mimo.mi.com/docs/en-US/news/latest/v2.5-news)

## Остальные

Для Moonshot/Kimi K3, ByteDance Seed2.0, ERNIE 5.1, Hunyuan Hy3, StepFun 3.5, LongCat-2.0, Ring-2.6 и iFlytek я не нашёл одновременно:

- стабильного международного официального USD-прайса;
- подтверждённой оплаты обычной EU-картой;
- documented Anthropic compatibility;
- прозрачных rolling limits.

Их прямые планы для практического бюджета следует считать **не подтвержденными**. У Moonshot к тому же в июле были временно приостановлены новые подписки из-за дефицита мощности после Kimi K3.[AP, опубликовано в июле 2026](https://apnews.com/article/4c66a2e0f557ce79d3cc2d769c9a6226)

---

# 5. Интеграция

## Anthropic-compatible

Подтверждены:

- **DeepSeek:** `https://api.deepseek.com/anthropic`, официальный пример прямо задаёт `ANTHROPIC_BASE_URL`, `ANTHROPIC_MODEL=deepseek-v4-pro[1m]` и настройки Claude Code.[DeepSeek coding-agent guide, опубликован после 24.04.26](https://api-docs.deepseek.com/guides/coding_agents)
- **Alibaba Model Studio:** для Qwen3.7-Max документация перечисляет Anthropic-compatible endpoint вида `/apps/anthropic` наряду с OpenAI endpoint.[Alibaba supported models, обновлено 15.07.26](https://www.alibabacloud.com/help/en/model-studio/models)

Z.ai официально поддерживает Claude Code через Coding Plan, но документированный endpoint — OpenAI-style `https://api.z.ai/api/coding/paas/v4`; drop-in поведение обеспечивается helper/configuration layer, а не чистой гарантией Anthropic Messages API.[Z.ai quick start, июнь 2026](https://docs.z.ai/guides/overview/quick-start)

Остальные в основном дают OpenAI-compatible API или собственный SDK.

## Функции

DeepSeek V4-Pro/Flash официально поддерживают:

- JSON output;
- tool calls;
- streaming через совместимый API;
- автоматический prompt cache;
- FIM;
- до 384K output и 1M input context.[DeepSeek pricing/features, 24.04.26](https://api-docs.deepseek.com/quick_start/pricing?article_id=article_1779470751466_8)

Осторожно со старыми гайдами: прежний `deepseek-reasoner` не поддерживал function calling; это относится к R1-era endpoint и не должно переноситься на V4.[DeepSeek legacy reasoning guide, исторический документ](https://api-docs.deepseek.com/guides/reasoning_model)

## Латентность в Европе

Публичного воспроизводимого измерения Portugal/Germany с одинаковыми prompt/output lengths для всех провайдеров нет.

Практически:

- Alibaba имеет Frankfurt endpoint — минимальный сетевой риск для Германии/Португалии.[Alibaba overview, 10.07.26](https://www.alibabacloud.com/help/en/model-studio/what-is-model-studio)
- DeepSeek не публикует европейский endpoint; официальный status page за 90 дней показывал API uptime около **99.92%**, но в мае были partial outages.[DeepSeek status, данные за май–июль 2026](https://status.deepseek.com/)
- Z.ai global endpoint географию серверов на pricing/quick-start страницах не раскрывает.

Точные p50/p95 надо замерять из вашей сети. Я бы проверял: 10K cold prompt, 100K cold prompt, 100K cache-hit, tool-call round-trip и sustained output TPS, по 30 запусков в 10:00 и 18:00 UTC.

---

# 6. Локальный запуск на RTX 5060 Ti 16GB

Реально полезны:

- **Qwen 9B/14B или MoE около 30–35B total с 3B active**, Q4_K_M — одна 16GB карта, если KV cache и context держать умеренными.
- **Step 3.5 Flash INT4** требует существенно больше 16GB; официальный пример ориентируется на DGX Spark 128GB, поэтому одна 5060 Ti не подходит.[StepFun, 12.02.26](https://static.stepfun.com/blog/step-3.5-flash/%3Fref%3Dtestingcatalog.com)
- Tencent HY-MT1.5 1.8B/7B имеет официальные GPTQ-INT4/GGUF варианты и действительно помещается на одну карту, но это переводчик, не code-review frontier.[Tencent HY-MT, 30.12.25 — исторический контекст](https://github.com/Tencent-Hunyuan/HY-MT)
- GLM-5.2, DeepSeek V4, Kimi K3, LongCat-2.0 и полномасштабный MiniMax не являются моделями для 16–32GB VRAM. 4-bit весов недостаточно: остаются KV cache, experts/offload и неприемлемая скорость.

Для локальной «четвёртой пары глаз» я бы поставил Qwen размером 14B–30B, но не выдавал бы его за замену API-флагману. Q4 обычно сохраняет общую полезность; Q2/Q3 и агрессивная MoE-квантизация заметно повреждают tool-use и точное редактирование.

---

# 7. Риски

## Право и санкции

Для обычного резидента ЕС нет общего запрета оплачивать китайский LLM-сервис. Ограничения США в основном касаются экспорта передовых чипов/технологий и отдельных организаций, а не обычной покупки inference гражданином ЕС.

Но DeepSeek запрещён на **системах австралийского правительства**; это не общенациональный потребительский запрет.[Australian PSPF Direction 001-2025, действующая страница в 2026](https://www.protectivesecurity.gov.au/news/pspf-direction-update-deepseek-products-applications-and-web-services)

Италия и германские органы поднимали вопросы GDPR/удаления приложения, но это не эквивалент запрету гражданину ЕС вызывать открытые веса или любой сторонний EU-hosted endpoint. Для коммерческого кода ключевой вопрос — GDPR, DPA, trade secrets и договор с клиентом, а не национальность модели.

## Данные

DeepSeek прямо сообщает, что может собирать prompts, uploaded files и chat history; политика обновлена **10 февраля 2026 года** и допускает хранение за пределами страны пользователя.[DeepSeek Privacy Policy, 10.02.26](https://cdn.deepseek.com/policies/en-US/deepseek-privacy-policy.html?locale=en_US)

В более старой platform policy указывалось хранение на серверах в mainland China; это **исторический контекст**, который нельзя считать опровергнутым лишь потому, что новая формулировка стала шире.[DeepSeek platform privacy policy, исторический документ](https://platform.deepseek.com/downloads/DeepSeek%20Privacy%20Policy.pdf)

Практический вывод: без подписанного DPA и явного no-training/no-retention режима считать прямой API безопасным для секретного client code нельзя.

Если отправляете код, фактически раскрываете провайдеру:

- исходный текст и комментарии;
- архитектуру;
- embedded test data;
- секреты, случайно попавшие в repo/context;
- vulnerability information;
- историю tool calls и diff.

Это не юридическая «публикация в public domain», но с точки зрения trade-secret hygiene это **передача третьей стороне**. Перед отправкой нужны secret scanning, contract review и allowlist репозиториев.

## Цензура

DeepSeek и другие mainland-hosted модели заметно ограничивают политические темы; академическое исследование января 2026 года документирует information suppression на чувствительных китайских темах.[Information Processing & Management, январь 2026](https://www.sciencedirect.com/science/article/pii/S0020025525008357)

Для обычного технического review эффект мал. Он проявляется, если:

- код относится к политическому контенту, surveillance или moderation;
- имена тестовых fixtures совпадают с чувствительными темами;
- модель должна анализировать compliance/геополитику.

Самохостинг открытых весов обычно уменьшает API-layer цензуру, но модельная alignment-тенденция может сохраниться.

## Устойчивость

Документированные признаки риска:

- DeepSeek вывел старые `deepseek-chat`/`deepseek-reasoner` aliases **24 июля 2026 года**, спустя три месяца после анонса V4.[DeepSeek changelog, 24.04–24.07.26](https://api-docs.deepseek.com/updates/)
- Xiaomi автоматически перемаршрутизировал старые V2 на V2.5 и отключил их 30 июня.[Xiaomi docs, 29.06.26](https://mimo.mi.com/docs/en-US)
- Alibaba только в 2026 году публиковал многочисленные price adjustments и model sunsets.[Alibaba release notes, обновлено 15.07.26](https://www.alibabacloud.com/help/en/model-studio/model-release-notes)
- Kimi K3 временно остановил новые подписки из-за спроса.[AP, июль 2026](https://apnews.com/article/4c66a2e0f557ce79d3cc2d769c9a6226)

Историю «GLM $3 → $10 → $18 за три месяца» я не смог подтвердить последовательностью официальных архивных прайсов; считать её установленным фактом нельзя.

---

# 8. Рекомендация: топ-3

## 1. DeepSeek V4-Pro API

**Стоимость:** без абонплаты; $0.435 input, $0.003625 cached input, $0.87 output за 1M.[Официальный прайс, 24.04.26](https://api-docs.deepseek.com/quick_start/pricing?article_id=article_1779470751466_8)

**Почему даёт другой взгляд:** более длинное и явное рассуждение, склонность искать алгоритмически дешёвое решение, иной training/cultural prior, чем у Grok и Gemini. Особенно полезен для проверки алгоритмов, concurrency, edge cases и «можно ли сделать проще».

**Подключение:**

1. Создать DeepSeek API key и пополнить небольшой баланс.
2. Задать `ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic`.
3. Задать `ANTHROPIC_AUTH_TOKEN=<key>`.
4. Задать `ANTHROPIC_MODEL=deepseek-v4-pro[1m]`.
5. Запускать отдельную read-only Claude Code сессию с командой «review only, do not edit».[Официальная инструкция, апрель 2026](https://api-docs.deepseek.com/guides/coding_agents)

Главный риск: prompts/data и отсутствие EU-region/DPA по умолчанию.

## 2. Z.ai GLM-5.2 Coding Plan Lite

**Стоимость:** $18/мес; точный token allowance публично не раскрыт.[Z.ai plan, июль 2026](https://z.ai/subscribe?invitedialog=true)

**Почему другой взгляд:** GLM-5.2 явно тренирован на long-horizon planning и часто полезнее как «архитектор/ревьюер плана», чем как быстрый patch generator. Это наиболее логичный кандидат для стратегических решений и критики плана.

**Подключение:**

1. Зарегистрировать global Z.ai account.
2. Купить Lite и проверить оплату картой до передачи кода.
3. Создать Coding Plan key.
4. Использовать `https://api.z.ai/api/coding/paas/v4`.
5. Подключить через Z.ai helper или OpenAI-compatible клиент; для Claude Code использовать официальный coding-helper.[Z.ai quick start, июнь 2026](https://docs.z.ai/guides/overview/quick-start)

Главный риск: непрозрачная базовая квота и коэффициенты потребления.

## 3. Alibaba Model Studio international + Qwen

**Стоимость:** pay-as-you-go. Qwen3.7-Max list $2.50/$7.50, что заметно дороже DeepSeek; при временной 50% скидке — $1.25/$3.75, но её надо проверять в консоли.[Alibaba pricing, 15.07.26](https://www.alibabacloud.com/help/en/model-studio/model-pricing)

**Почему другой взгляд:** Qwen лучше всего подходит для мультиязычного и product-oriented review: API contracts, documentation, UI logic, internationalization, Chinese/English/Russian mixed material. Он несколько меньше дублирует математический стиль DeepSeek.

**Подключение:**

1. Создать Alibaba Cloud International account.
2. Выбрать Frankfurt или Singapore workspace.
3. Включить Model Studio и получить free quota.
4. Создать региональный API key.
5. Использовать OpenAI endpoint либо `/apps/anthropic` для поддерживаемой модели.[Alibaba overview, 10.07.26](https://www.alibabacloud.com/help/en/model-studio/what-is-model-studio) [Models/API endpoints, 15.07.26](https://www.alibabacloud.com/help/en/model-studio/models)

Главный риск: Qwen Max дорог, модельные имена и цены часто обновляются.

---

# Что не брать

- **Kimi K3 сейчас:** вычислительный дефицит, приостановка подписок, цена выше большинства китайских альтернатив; достоин повторной проверки после стабилизации capacity.[AP, июль 2026](https://apnews.com/article/4c66a2e0f557ce79d3cc2d769c9a6226)
- **MiniMax как основной review API:** сильная self-reported оценка, но актуальные цены и независимая воспроизводимость хуже документированы.
- **ERNIE, Doubao, iFlytek:** сильные внутри китайской экосистемы, но международный billing/API/privacy путь неоптимален.
- **01.AI, Baichuan:** нет достаточного свежего frontier-релизного следа.
- **LongCat-2.0 локально:** модель слишком велика для ваших GPU; через API может стать интересной позже, но international pricing пока не стабилен.
- **Step 3.5 локально:** 196B-модель не подходит для 16–32GB VRAM.[StepFun, 12.02.26](https://static.stepfun.com/blog/step-3.5-flash/%3Fref%3Dtestingcatalog.com)
- **Любой прямой mainland API для NDA/client code без DPA:** экономия не компенсирует риск раскрытия trade secrets.

---

# Итоговая таблица

| Провайдер | Модель | $/мес | $/1M in | $/1M out | Совместимость | Оплата из ЕС | Веса | Главный риск |
|---|---|---:|---:|---:|---|---|---|---|
| DeepSeek | V4-Pro | $0 PAYG | $0.435; cache $0.003625 | $0.87 | Anthropic + OpenAI | не гарантирована официально | не подтверждено для V4 | данные/mainland, нет EU endpoint |
| DeepSeek | V4-Flash | $0 PAYG | $0.14; cache $0.0028 | $0.28 | Anthropic + OpenAI | не гарантирована | не подтверждено для V4 | качество ниже Pro |
| Z.ai | GLM-5.2 | $18 Lite | не подтверждено | не подтверждено | OpenAI-style + Claude Code helper | global plan есть; card acceptance проверять | MIT | непрозрачная квота/цена |
| Z.ai | GLM-4.7 | PAYG | $0.60; cache $0.11 | $2.20 | OpenAI | global | MIT/open | не текущий флагман |
| Alibaba | Qwen3.7-Max | PAYG | $2.50 list | $7.50 list | Anthropic + OpenAI | да, international account | закрытая | цена/model churn |
| Moonshot | Kimi K3 | не подтверждено | не подтверждено | не подтверждено | преимущественно OpenAI | capacity/billing риск | open status проверять | дефицит мощности |
| MiniMax | M2.5 | не подтверждено | не подтверждено | не подтверждено | OpenAI/proprietary | Stripe availability проверять | открытые | self-reported benchmarks |
| Xiaomi | MiMo-V2.5-Pro | landing-page price | не подтверждено | не подтверждено | OpenAI-style | Stripe/Waffo | статус checkpoint проверять | быстрые депрекейты |
| Tencent | Hy3-preview | не подтверждено | не подтверждено | не подтверждено | не подтверждено | слабый international path | custom Tencent | лицензия/API |
| StepFun | Step 3.5 Flash | не подтверждено | не подтверждено | не подтверждено | OpenAI/local | не подтверждено | открытые INT4/GGUF | слишком велик локально |
| Meituan | LongCat-2.0 | не подтверждено | не подтверждено | не подтверждено | сторонние площадки | через marketplace | открытые | 1.6T, API незрел |
| Baidu | ERNIE 5.1 | не подтверждено | не подтверждено | не подтверждено | Baidu API | mainland friction | закрытая | слабый независимый coding trail |

Официальные подтверждения строк DeepSeek, Z.ai и Alibaba: [DeepSeek, 24.04.26](https://api-docs.deepseek.com/quick_start/pricing?article_id=article_1779470751466_8), [Z.ai pricing, июнь 2026](https://docs.z.ai/guides/overview/pricing), [Z.ai plan, июль 2026](https://z.ai/subscribe?invitedialog=true), [Alibaba pricing, 15.07.26](https://www.alibabacloud.com/help/en/model-studio/model-pricing).

**Что протухнет быстрее всего:** цены, coding-plan коэффициенты, список разрешённых моделей, Kimi capacity и alias/deprecation rules. Перепроверять **ежемесячно**, а официальный прайс и endpoint — непосредственно **перед каждым пополнением баланса**; независимые coding leaderboards — раз в **6–8 недель**.
