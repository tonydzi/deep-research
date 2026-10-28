# DR26-10-19-HUB-05-2154 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Сначала сверюсь со скиллом deep-research и живыми первоисточниками по всем шести блокам.Контракт запрещает файлы — собираю первоисточники сам по всем шести вопросам.Дальше открываю первоисточники и закрываю пробелы по safety, API памяти и контексту.Проверяю ключевые первоисточники в этом заходе, затем сразу отдам полный отчёт.Добираю оставшиеся первоисточники по инцидентам, окнам контекста и защитам.Ещё несколько первоисточников по Orca, спекулятивному декодированию и памяти продуктов.Достаю оставшиеся страницы по context rot, Llama Guard и API истории.Сверяю ключевые первоисточники и добираю недостающие цифры, затем сразу отдам полный отчёт.Добираю оставшиеся первоисточники по изоляции, jailbreak-защите, памяти и контексту.Ещё несколько первоисточников по классификаторам, compaction и context rot — после них сразу отчёт.Проверяю первоисточники в этом заходе, затем сразу отдам полный отчёт.Добираю оставшиеся первоисточники по API, памяти, jailbreak-классификаторам и контексту.Сверяю точные цифры по уже открытым первоисточникам, затем сразу пишу полный отчёт.A. Как прод-сервис обслуживает тысячи пользователей с одной копией весов

Прод-чат (ChatGPT, Claude, Gemini) и открытые стеки (vLLM, SGLang, TensorRT-LLM) устроены как inference-ферма, а не как «отдельная нейросеть на каждого клиента».

На GPU лежит одна (или несколько шардированных) копия параметров. Веса на инференсе frozen: их читают, не пишут. Параллельные запросы делят эти веса в режиме read-only. Пер-запросное состояние — это активации текущего шага и KV-кэш (ключи и значения attention по уже увиденным токенам). Когда запрос закончился, его KV освобождается; веса остаются теми же.

Это ближе к одному большому read-only исполняемому файлу плюс куча приватных стеков, чем к «копии модели на пользователя». Throughput тысяч пользователей даёт не размножение весов, а (1) iteration-level / continuous batching, (2) виртуальная память для KV (PagedAttention), (3) разделение compute-bound prefill и memory-bound decode, (4) tensor/pipeline/expert parallelism, (5) speculative decoding, (6) prefix/prompt cache.

Источники архитектуры батчинга и KV: https://www.usenix.org/conference/osdi22/presentation/yu · https://arxiv.org/html/2309.06180 · https://www.anyscale.com/blog/continuous-batching-llm-inference

B. Одна копия весов, много пользователей: механика serving

B.1 Веса shared, KV — нет

Transformer на шаге t считает attention по всем предыдущим токенам. Чтобы не пересчитывать K и V прошлого, их кладут в KV-кэш. Формула на один токен (оценка по определению, совпадает с разбором vLLM для OPT-13B):

bytes/token ≈ 2 × L × H_kv × d_h × bytes_per_elem

где 2 = Key+Value, L = число слоёв, H_kv = число KV-голов (после GQA/MQA меньше, чем query-голов), d_h = размер головы.

Конкретное число из статьи vLLM (SOSP 2023): для OPT-13B в FP16 это 2 × 5120 × 40 × 2 = 800 KB на токен. На A100-40GB у 13B-модели авторы оценивают ~65% памяти на веса и ~30% на KV. В системах до PagedAttention реально использовалось только 20.4–38.2% зарезервированного KV из-за внутренней фрагментации и резерва под max length. Источник: https://arxiv.org/html/2309.06180

Оценка (не из той статьи, а вывод из публичной архитектуры Llama 3 70B: 80 слоёв, GQA 8 KV-голов, d_h=128, BF16): 2×80×8×128×2 = 327 680 байт ≈ 320 KiB/token. Это оценка; конфиг Hugging Face в этом прогоне не открывался.

B.2 Prefill vs decode

Prefill: весь промпт одним (или chunked) проходом. Много токенов сразу, матрицы большие, фаза compute-bound. Метрика — TTFT (time to first token).

Decode: по одному новому токену за шаг. Каждому токену нужно прочитать все веса плюс весь KV. Фаза memory-bound. Метрика — ITL / TPOT (inter-token latency).

Смешивать длинный prefill чужого запроса с decode вашего в одном GPU-батче без изоляции фаз портит хвост ITL: чужой prefill «вклинивается» в генерацию. Это центральный мотив SARATHI и DistServe.

B.3 Continuous / iteration-level batching (Orca, OSDI 2022)

Статический батч: ждать, пока все последовательности в батче закончатся. Короткие запросы простаивают, GPU недогружен.

Orca вводит iteration-level scheduling: на каждом шаге декодера в батч входят живые запросы, закончившиеся выходят, новые входят, не дожидаясь общего EOS. Selective batching применяет batch-матрицы там, где формы совпадают, и поэлементно там, где attention требует разных длин.

Заявленный результат Orca: до 36.9× throughput против FasterTransformer на GPT-3 175B при том же уровне latency. Источник: https://www.usenix.org/conference/osdi22/presentation/yu

Anyscale (июнь 2023, живой блог поверх Orca+vLLM) на OPT-13B / A100-40GB: naive continuous batching ≈ 8× против naive static; FasterTransformer ≈ 4× против naive static; vLLM + continuous batching — до 23× против naive static. Источник: https://www.anyscale.com/blog/continuous-batching-llm-inference

Расхождение: 36.9×, 23× и 2–4× (vLLM vs Orca/FT, см. B.4) — разные базы, модели и SLO. Это не три измерения одного эффекта.

B.4 PagedAttention / vLLM (SOSP 2023)

KV раньше выделяли одним сплошным куском на max length. Большая часть блока пустовала (резерв) плюс внешняя фрагментация между запросами.

PagedAttention режет KV на блоки фиксированного размера по аналогии с виртуальной памятью ОС. Блочная таблица на запрос, copy-on-write при fork (beam search / parallel sampling), почти без резерва. vLLM заявляет 2–4× throughput против FasterTransformer и Orca. Источник: https://arxiv.org/html/2309.06180

B.5 Chunked prefill (SARATHI)

SARATHI режет prefill на чанки и в каждом шаге планировщика смешивает чанк prefill с decode-токенами так, чтобы заполнить GPU, не устраивая stall decode. На LLaMA-13B / A6000: decode throughput до 10×, end-to-end 1.33×. На 33B / A100: e2e 1.25×, decode 4.25×. Pipeline-parallel «пузыри» уменьшаются в 6.29×, e2e 1.91×. Источник: https://arxiv.org/abs/2308.16369

B.6 Disaggregated prefill/decode

DistServe (Splitwise-класс): prefill и decode на разных GPU/инстансах, KV передаётся по сети. Заявление статьи: до 7.4× больше запросов, удовлетворяющих SLO, или 12.6× более жёсткий SLO. Источник: https://arxiv.org/abs/2401.09670

Документация vLLM (experimental, страница от 3 октября 2026) прямо пишет: disaggregated prefill DOES NOT improve throughput. Смысл — отдельно крутить TTFT и ITL и резать хвост ITL, потому что без разнесения планировщик может вставить prefill в чужой decode. Девять коннекторов: Nixl, Mooncake, LMCache, FlexKV, Offloading, MultiConnector и др. Источник: https://docs.vllm.ai/en/latest/features/disagg_prefill/

Расхождение DistServe vs vLLM docs: статья меряет «больше запросов под SLO» (часто TTFT+ITL совместно), vLLM меряет сырой throughput токенов. Оба утверждения совместимы, если читать метрики буквально, и противоречат друг другу, если смешать «throughput» в бытовой речи.

B.7 Параллелизм модели

Tensor parallelism (TP): матрицы одного слоя режутся по GPU, нужен быстрый interconnect (NVLink/IB) на каждый слой.

Pipeline parallelism (PP): слои по стадиям; без chunked prefill появляются пузыри (SARATHI как раз их уменьшает).

Expert parallelism (EP): у MoE каждый токен идёт в подмножество экспертов; отдельный all-to-all.

На инференсе это способ уместить одну копию весов (или MoE-экспертов) на кластер, а не способ изолировать пользователей. Пользователи по-прежнему делят те же шарды.

B.8 Speculative decoding

Черновик (маленькая draft-модель или головы Medusa/EAGLE) предлагает k токенов, большая модель проверяет их одним проходом. Принятый префикс становится «бесплатным» относительно авторегрессии по одному.

NVIDIA TensorRT-LLM (декабрь 2024): Llama 3.1 405B на 4×H200, draft 3B → 3.61× (120.75 vs 33.46 tok/s); draft 1B → 3.33×; draft 8B → 3.04×. Llama 3.3 70B на 1×H200, draft 1B → 2.86×. Источник: https://developer.nvidia.com/blog/tensorrt-llm-speculative-decoding-boosts-inference-throughput-by-up-to-3-6x/

B.9 Prefix sharing (SGLang)

SGLang (NeurIPS 2024) хранит KV префиксов в radix-дереве (RadixAttention): общий system prompt и общие префиксы многопользовательского кэша не дублируются. До 6.4× против vLLM/Guidance/LMQL на своих бенчмарках. Источник: https://arxiv.org/abs/2312.07104

Это уже стык serving и «изоляции»: выигрыш есть, только если префиксы реально совпадают. Кто видит чужой префикс по таймингу — вопрос секции C.

C. Изоляция: может ли один пользователь «сломать модель» другим

C.1 Что общее, что нет

Веса frozen. Вредный промпт не дописывает градиент в общую модель. Состояние запроса = KV + текущие активации. После EOS KV этого запроса должен быть освобождён.

Сломать другим «модель как функцию весов» одним запросом в корректном serving-стеке нельзя. Испортить им ответ, утечь данные или сделать вывод недетерминированным — можно другими слоями: баг кэша приложения, shared prompt cache, батч-инвариантность ядер, side channel по времени.

C.2 Реальный инцидент: ChatGPT, 20 марта 2023

The Verge, опираясь на постмортем OpenAI: гонка в redis-py (asyncio Redis cluster) отдавала чужие кэшированные ответы. Утекали заголовки чатов и первое сообщение; у 1.2% подписчиков ChatGPT Plus — last-4 карты, expiry, имя, email, адрес. Полный PAN OpenAI назвала не раскрытым. Источник, открытый в этом прогоне: https://www.theverge.com/2023/3/24/23655622/chatgpt-outage-payment-info-exposed-monday

Официальный URL, который цитируют: https://openai.com/blog/march-20-chatgpt-outage — тело страницы в этом прогоне не получено (пустой/недоступный ответ). Это баг слоя приложения (Redis), не баг transformer-весов.

C.3 Prompt-cache timing side channels (2024–2025)

Early Bird (arxiv 2409.20002): и KV-кэш, и semantic cache дают измеримую разницу latency. Через cache-hit можно красть system prompt и, при shared cache, чужие префиксы. Источник: https://arxiv.org/abs/2409.20002

Auditing Prompt Caching (ICML 2025, arxiv 2502.07776): статистический аудит API; global cache sharing across users обнаружен у семи провайдеров, включая OpenAI; по таймингу авторы заключают, что embedding-модель OpenAI — decoder-only Transformer. Источник: https://arxiv.org/abs/2502.07776

Расхождение с вендорными доками. Anthropic: prompt cache изолирован по workspace, не шарится между организациями. Источник: https://platform.claude.com/docs/en/build-with-claude/prompt-caching

OpenAI: caches are not shared across organizations; в кэше лежат KV-тензоры, не токены. Источник: https://developers.openai.com/api/docs/guides/prompt-caching

Академический аудит 2025 говорит о global sharing, включая OpenAI. Вендоры описывают org/workspace isolation. Возможные объяснения, ни одно не доказано этим прогоном: аудит видел внутриорганизационный шардинг как «global»; политика кэша сменилась между февралём 2025 и октябрём 2026; isolation есть на одних продуктах и нет на других (embeddings vs chat). Это место, где источники спорят.

C.4 Batch non-determinism (Thinking Machines, 2025)

Даже greedy (temperature=0) на одном и том же промпте даёт разные строки, если меняется состав батча. Причина — не «модель учится», а неассоциативность floating point: разные split/reduction в kernels, когда батч другой формы.

Thinking Machines: Qwen3-235B-A22B, 1000 greedy прогонов → 80 уникальных completion; первые 102 токена совпали, на токене 103 разошлись (Queens vs NYC как иллюстрация). С batch-invariant kernels: 1000 идентичных. Цена: vLLM default 26 с vs наивный детерминизм 55 с vs улучшенный attention 42 с. Источник: https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/

Это контаминация численная, не текстовая: ваш сосед по батчу не читает ваш промпт, но меняет порядок сложений и, следовательно, argmax.

D. Вредные запросы: слои защиты

Стек — не один «фильтр», а конвейер. Ни один слой не даёт нуля.

1. Фильтрация претрейна (документация конкретных пайплайнов ChatGPT/Claude/Gemini в этом прогоне не вскрывалась как первоисточник с цифрами; слой существует в литературе RLHF/CAI как предпосылка).
2. Посттрейн alignment: RLHF (InstructGPT) и Constitutional AI / RLAIF.
3. Внутренние представления: refusal direction (Arditi et al. 2024).
4. Внешние классификаторы входа/выхода: Constitutional Classifiers, Llama Guard, OpenAI Moderation.
5. Продуктовые лимиты, мониторинг абьюза, bug bounty.
6. Anthropic ASL-3: deployment measures поверх модели.

D.1 RLHF и Constitutional AI

InstructGPT: 1.3B InstructGPT предпочтительнее 175B GPT-3 у людей-оценщиков на инструкциях. Источник: https://arxiv.org/abs/2203.02155

Constitutional AI: supervised self-critique по конституции, затем RLAIF. Цель — harmless, но не evasive. Источник: https://arxiv.org/abs/2212.08073

D.2 Refusal direction (Arditi et al., 2024)

Отказ у 13 chat-моделей вплоть до 72B опосредуется одним направлением в residual stream. Стереть направление — отказ падает; добавить — отказ появляется даже на benign. Это не продакшен-гейт, это механика, которую jailbreak и white-box атаки эксплуатируют. Источник: https://arxiv.org/abs/2406.11717

D.3 Constitutional Classifiers (Anthropic, 2025) и CC++ (январь 2026)

Первое поколение: ASR (attack success rate) 86% → 4.4% относительно незащищённой модели (заблокировано 95% атак, которые проходили safety training); +0.38 п.п. refusal на безвредных; +23.7% compute. Живое демо: 1 универсальный jailbreak среди 339 jailbreakers / ~300k чатов. Источник: https://www.anthropic.com/research/constitutional-classifiers

CC++ (9 января 2026): линейный probe по внутренним активациям на весь трафик, эскалация в exchange-classifier (вход+выход вместе). Отказы 0.05% на harmless (падение 87% относительно CC); ~1% compute на трафике уровня Opus 4.0; 1700+ часов red team / 198 000 попыток; 1 high-risk уязвимость = 0.005 на 1000 запросов; универсальный jailbreak не найден. Остаются reconstruction (вред, нарезанный на innocuous куски) и output obfuscation (химикаты как «пищевые ароматизаторы»). Jailbreak сажает GPQA Diamond с 74% до 32% в некоторых режимах — атака часто тупит саму модель. Источник: https://www.anthropic.com/research/next-generation-constitutional-classifiers · бумага https://arxiv.org/abs/2601.04603

D.4 Llama Guard и OpenAI Moderation

Llama Guard (декабрь 2023): Llama-2-7B, instruction-tuned на таксономии рисков, классификация и промпта, и ответа; на OpenAI Moderation Eval и ToxicChat — на уровне или лучше доступных тогда модераторов; веса открыты. Источник: https://ai.meta.com/research/publications/llama-guard-llm-based-input-output-safeguard-for-human-ai-conversations/

Страница OpenAI Moderation в этом прогоне редиректила с https://platform.openai.com/docs/guides/moderation на https://developers.openai.com/api/docs/guides/moderation ; тело целевой страницы не прочитано (редирект отклонён клиентом). Цифр ASR по Moderation API в этом отчёте нет.

D.5 ASL-3

Anthropic 22 мая 2025 активировала ASL-3 вместе с Claude Opus 4. Фокус — CBRN. Меры: Constitutional Classifiers, bug bounty, 100+ security controls, ограничение egress bandwidth. Nation-state и sophisticated insider прямо выведены за скоуп ASL-3. Источник: https://www.anthropic.com/news/activating-asl3-protections

Итог по jailbreak: публичные до/после с цифрами есть у Anthropic CC/CC++. Сопоставимого «86% → 4.4%» от OpenAI и Google в открытых первоисточниках этого прогона нет. «Классификаторы закрыли jailbreak» как абсолют источники сами опровергают: universal в CC-1 найден, reconstruction/obfuscation живут в CC++.

E. История чата и «память»: API vs продукт

E.1 Chat Completions остаются stateless

Классический Chat Completions: клиент каждый ход присылает полный messages[]. Сервер не обязан помнить прошлый ход. KV прошлого запроса к этому моменту уже мёртв, если нет prompt cache по префиксу.

E.2 OpenAI Responses API — опциональный store

Документация conversation state (модель в примерах `gpt-6-astra`):

- `previous_response_id` — сервер подтягивает прошлый response.
- `store` (по умолчанию сохранение; `store=false` — не хранить).
- Responses хранятся 30 дней по умолчанию.
- Conversation objects (Conversations API) без 30-дневного TTL.
- Даже с `previous_response_id` предыдущие input tokens биллятся.
- Compaction: `context_management` + `compact_threshold` (пример 200 000) внутри `/responses`, либо отдельный `POST /responses/compact`. Сервер кладёт opaque encrypted compaction item; это не обновление весов. Режим ZDR-friendly при `store=false`.

Источники: https://developers.openai.com/api/docs/guides/conversation-state/ · https://developers.openai.com/api/docs/guides/compaction

E.3 Anthropic Messages и Gemini

Messages API по контракту тоже принимает полный список сообщений (клиентский transcript). Prompt caching — отдельный слой поверх этого (секция F), не «память личности».

Gemini 4 Argon (доки обновлены 2026-10-14): единое окно 1 048 576 токенов; max output 65 536 по умолчанию / до 1M long-decode; только implicit cache; explicit caching на Gemini 4 Argon не поддерживается. Источник: https://ai.google.dev/gemini-api/docs/models/gemini-4-argon · https://ai.google.dev/gemini-api/docs/caching

E.4 Продуктовая «память» ChatGPT/Claude

Claude: одна память на chat и Cowork; пользователь правит topic files; чувствительные темы выключены по умолчанию; SSN / criminal / immigration не хранятся даже если память включена. Это файлы/retrieval, которые потом инжектятся в контекст, не fine-tune. Источник: https://claude.com/blog/claudes-memory-works-everywhere-and-you-decide-whats-in-it

ChatGPT Memory / Dreaming: https://openai.com/index/chatgpt-memory-dreaming/ в этом прогоне вернул пустое тело. Независимых цитат OpenAI по механике dreaming в отчёте нет. Из serving-архитектуры (веса frozen) и из открытой документации Responses следует тот же класс систем: внешнее хранилище + подстановка в prompt, не live gradient update.

F. Контекст: окна, квадрат внимания, кэш, rot, RAG

F.1 Окна на октябрь 2026 (то, что открыто)

Claude (overview): Fable 5.1, Opus 5.5, Sonnet 5.5, Haiku 5.5 — все 1M context, 128K max output. Knowledge cutoff июнь 2026. Цены input/output: Fable $10/$50, Opus 5.5 $4/$20, Sonnet 5.5 $2/$10, Haiku от $0.10/$0.50 за MTok. Источник: https://platform.claude.com/docs/en/models/overview

Gemini 4 Argon: 1 048 576 unified. Источник: https://ai.google.dev/gemini-api/docs/models/gemini-4-argon

OpenAI: в открытых в этом прогоне страницах фигурируют `gpt-6-astra` и `gpt-5.3-codex` как имена в примерах compaction/state; отдельная таблица «окно GPT-5.6 / GPT-6» с каноническим числом в этом прогоне не читалась. Не утверждаю размер окна OpenAI сверх того.

F.2 Квадрат внимания vs линейный KV

Полный attention по n токенам — O(n²) по compute на prefill. Decode добавляет O(n) на токен (пройтись по KV). Память KV растёт линейно: bytes/token × длина × батч. Именно линейный KV, а не квадрат FLOP, обычно упирается в HBM и ограничивает concurrent users. Отсюда PagedAttention, GQA, disagg, prompt cache.

F.3 Prompt caching: цены и TTL

Anthropic (открытая страница prompt-caching): TTL 5 минут по умолчанию, refresh на hit; 1 час за 2× write. Минимум 512 токенов на текущих моделях. Изоляция по workspace.

Sonnet 5.5: $2.00 input / $2.50 5m write / $4.00 1h write / $0.10 hits (= 0.05× input). Fable 5.1 hits $0.25 (0.025×). Opus 5.5 hits $0.20 (0.05×).

Models overview формулирует cache reads как 10% base input, для Fable 5.1 — 2.5%, для Opus 5.5 и Sonnet 5.5 — 5%. Это согласуется с долларовой таблицей, а не спорит с ней.

Источник: https://platform.claude.com/docs/en/build-with-claude/prompt-caching · https://platform.claude.com/docs/en/models/overview

OpenAI prompt caching: для GPT-5.6+ writes 1.25×, reads 0.1× (для GPT-6.1 Sol — 0.05×); минимум 1024 токена; TTL 30 минут по умолчанию; кэш не шарится между org; хранятся KV, не токены. Источник: https://developers.openai.com/api/docs/guides/prompt-caching

Gemini: implicit cache по умолчанию с 2.5+; Gemini 4 Argon min 4096; 2.5 Flash/Pro min 2048; explicit cache на Argon нет. Источник: https://ai.google.dev/gemini-api/docs/caching

F.4 Lost in the middle и context rot

Liu et al., TACL 2023 / arxiv 2307.03172: U-образная кривая — начало и конец контекста читаются лучше середины. У GPT-3.5-Turbo точность в середине может падать ниже closed-book baseline 56.1%. Источник: https://arxiv.org/html/2307.03172

Chroma «Context Rot» (июль 2025, 18 моделей): даже простые needle-in-a-haystack, semantic, distractor и repeat-word деградируют с длиной входа. LongMemEval: focused ~300 токенов vs full ~113k; у семейства Claude самый большой gap focused-vs-full через abstention. Перемешанный haystack иногда лучше структурированного. Источник: https://www.trychroma.com/research/context-rot

Практический вывод: 1M окно ≠ 1M рабочей памяти. Compaction (OpenAI) и саммаризация длинного чата — инженерный ответ на rot, не на квадрат FLOP.

F.5 RAG

RAG не расширяет окно модели. Он подставляет retrieved куски в prompt. Дальше действуют те же KV, тот же lost-in-the-middle и тот же prompt cache по стабильному префиксу (system + retrieved, если retrieved зафиксирован).

G. Три частых заблуждения

«Модель учится на моём чате в реальном времени». На инференсе веса frozen. Обучение — отдельный цикл (pretrain / SFT / RLHF), пакетный, с задержкой в недели и месяцы. Продуктовая Memory пишет во внешнюю БД и на следующем ходе кладёт текст в prompt. InstructGPT и CAI описывают offline alignment, не online gradient на чате. https://arxiv.org/abs/2203.02155 · https://arxiv.org/abs/2212.08073 · https://claude.com/blog/claudes-memory-works-everywhere-and-you-decide-whats-in-it

«Каждому пользователю — своя модель». Одна (шардированная) копия весов на много запросов. Изоляция — KV и application-state, не веса. https://arxiv.org/html/2309.06180 · https://www.usenix.org/conference/osdi22/presentation/yu

«Модель сама помнит вчерашний разговор». API Chat Completions не помнит. Responses помнит, только если store / previous_response_id / Conversations. Клиентский UI ChatGPT/Claude хранит transcript на сервере приложения и присылает его (или id) обратно в модель. Без этого вчерашнего чата в весах нет. https://developers.openai.com/api/docs/guides/conversation-state/

H. Таблица чисел (только то, что стояло в открытых источниках)

Метрика | Число | Условие | Источник
---|---|---|---
Orca vs FasterTransformer | 36.9× throughput | GPT-3 175B, тот же latency | https://www.usenix.org/conference/osdi22/presentation/yu
vLLM vs FT/Orca | 2–4× | статья SOSP 2023 | https://arxiv.org/html/2309.06180
KV OPT-13B | 800 KB/token FP16 | 2×5120×40×2 | https://arxiv.org/html/2309.06180
Утилизация KV до paging | 20.4–38.2% | prior systems | https://arxiv.org/html/2309.06180
A100-40GB 13B split | ~65% веса / ~30% KV | vLLM | https://arxiv.org/html/2309.06180
Anyscale vs naive static | до 23× | OPT-13B A100-40GB, vLLM+CB | https://www.anyscale.com/blog/continuous-batching-llm-inference
Naive CB vs static | ~8× | тот же стенд | https://www.anyscale.com/blog/continuous-batching-llm-inference
FasterTransformer vs naive | ~4× | тот же стенд | https://www.anyscale.com/blog/continuous-batching-llm-inference
SARATHI decode | до 10× | LLaMA-13B A6000 | https://arxiv.org/abs/2308.16369
SARATHI e2e | 1.33× / 1.25× | 13B A6000 / 33B A100 | https://arxiv.org/abs/2308.16369
SARATHI PP bubbles | −6.29×, e2e 1.91× | pipeline | https://arxiv.org/abs/2308.16369
DistServe | 7.4× запросов или 12.6× tighter SLO | статья | https://arxiv.org/abs/2401.09670
vLLM disagg throughput | не улучшает | docs 2026-10-03 | https://docs.vllm.ai/en/latest/features/disagg_prefill/
SGLang | до 6.4× | vs vLLM/Guidance/LMQL | https://arxiv.org/abs/2312.07104
TRT-LLM 405B specdec | 3.61× (120.75 vs 33.46 tok/s) | 4×H200, draft 3B | https://developer.nvidia.com/blog/tensorrt-llm-speculative-decoding-boosts-inference-throughput-by-up-to-3-6x/
TRT-LLM 70B specdec | 2.86× | 1×H200, draft 1B | https://developer.nvidia.com/blog/tensorrt-llm-speculative-decoding-boosts-inference-throughput-by-up-to-3-6x/
Greedy уникальных | 80 / 1000 | Qwen3-235B-A22B, temp 0 | https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/
CC ASR | 86% → 4.4% | vs unguarded | https://www.anthropic.com/research/constitutional-classifiers
CC overhead | +23.7% compute, +0.38 п.п. refusal | CC-1 | https://www.anthropic.com/research/constitutional-classifiers
CC++ overhead | ~1% compute, 0.05% refusal | Sonnet 4.5 / Opus 4.0 traffic | https://www.anthropic.com/research/next-generation-constitutional-classifiers
CC++ high-risk | 0.005 / 1000 | 198k попыток | https://www.anthropic.com/research/next-generation-constitutional-classifiers
Redis 2023 | 1.2% Plus PII | last-4/expiry/name/email/address | https://www.theverge.com/2023/3/24/23655622/chatgpt-outage-payment-info-exposed-monday
Claude context | 1M in / 128K out | Fable/Opus/Sonnet/Haiku 5.x | https://platform.claude.com/docs/en/models/overview
Gemini 4 Argon | 1 048 576 | unified window | https://ai.google.dev/gemini-api/docs/models/gemini-4-argon
Anthropic cache TTL | 5 мин / 1 ч | 1h = 2× write | https://platform.claude.com/docs/en/build-with-claude/prompt-caching
Sonnet 5.5 cache hit | $0.10 / MTok | 0.05× input $2 | https://platform.claude.com/docs/en/build-with-claude/prompt-caching
OpenAI cache | write 1.25×, read 0.1×, TTL 30 мин | GPT-5.6+; Sol read 0.05× | https://developers.openai.com/api/docs/guides/prompt-caching
OpenAI responses TTL | 30 дней | store default | https://developers.openai.com/api/docs/guides/conversation-state/
Lost-in-middle | GPT-3.5-Turbo mid < 56.1% closed-book | Liu 2023 | https://arxiv.org/html/2307.03172
Context rot LongMemEval | ~300 tok focused vs ~113k full | Chroma 2025 | https://www.trychroma.com/research/context-rot
InstructGPT | 1.3B preferred over 175B GPT-3 | human eval | https://arxiv.org/abs/2203.02155
KV Llama-3-70B | ≈320 KiB/token BF16 | оценка 2×80×8×128×2 | не из статьи; оценка

I. Первичные URL, открытые в этом исследовании (≥20)

1. https://www.usenix.org/conference/osdi22/presentation/yu
2. https://arxiv.org/html/2309.06180
3. https://www.anyscale.com/blog/continuous-batching-llm-inference
4. https://arxiv.org/abs/2308.16369
5. https://arxiv.org/abs/2401.09670
6. https://docs.vllm.ai/en/latest/features/disagg_prefill/
7. https://arxiv.org/abs/2312.07104
8. https://developer.nvidia.com/blog/tensorrt-llm-speculative-decoding-boosts-inference-throughput-by-up-to-3-6x/
9. https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/
10. https://www.theverge.com/2023/3/24/23655622/chatgpt-outage-payment-info-exposed-monday
11. https://arxiv.org/abs/2409.20002
12. https://arxiv.org/abs/2502.07776
13. https://www.anthropic.com/research/constitutional-classifiers
14. https://www.anthropic.com/research/next-generation-constitutional-classifiers
15. https://arxiv.org/abs/2601.04603
16. https://www.anthropic.com/news/activating-asl3-protections
17. https://arxiv.org/abs/2406.11717
18. https://arxiv.org/abs/2203.02155
19. https://arxiv.org/abs/2212.08073
20. https://ai.meta.com/research/publications/llama-guard-llm-based-input-output-safeguard-for-human-ai-conversations/
21. https://platform.claude.com/docs/en/build-with-claude/prompt-caching
22. https://platform.claude.com/docs/en/models/overview
23. https://developers.openai.com/api/docs/guides/prompt-caching
24. https://developers.openai.com/api/docs/guides/conversation-state/
25. https://developers.openai.com/api/docs/guides/compaction
26. https://ai.google.dev/gemini-api/docs/models/gemini-4-argon
27. https://ai.google.dev/gemini-api/docs/caching
28. https://arxiv.org/html/2307.03172
29. https://www.trychroma.com/research/context-rot
30. https://claude.com/blog/claudes-memory-works-everywhere-and-you-decide-whats-in-it

J. Покрытие, оценки, расхождения (внутри отчёта, не отдельным письмом)

Не получено тело: официальный постмортем OpenAI https://openai.com/blog/march-20-chatgpt-outage и https://openai.com/index/chatgpt-memory-dreaming/ (пусто). OpenAI Moderation docs — редирект, тело не прочитано. Llama Guard — абстракт Meta, полный PDF не разбирался. HTML vLLM и Lost in the Middle в ранних фетчах обрезались ~60k; ключевые цифры 800 KB, 2–4×, 20.4–38.2%, U-shape и 56.1% из влезающего куска.

Оценки явно: KV Llama-3-70B ≈320 KiB/token; «тысячи пользователей на один replica-set» как ёмкость — функция HBM, длины контекста и SLO, одного канонического «batch size = N» у вендоров нет.

Источники спорят в четырёх местах: (1) DistServe «больше запросов под SLO» vs vLLM «disagg не поднимает throughput»; (2) академический аудит 2025 о global prompt-cache sharing vs доки Anthropic/OpenAI об org/workspace isolation; (3) 36.9× / 23× / 2–4× — разные базы; (4) CC-1 нашёл 1 universal jailbreak, CC++ — пока ноль; это смена системы, не одно измерение.

Практическая картинка для инженера: replica = frozen weights + PagedAttention pool. Scheduler набирает mixed batch (chunked prefill + decode). Prefill-кластер при необходимости шлёт KV на decode-кластер. Общий system prompt сидит в radix/prompt cache. Классификатор/probe смотрит вход, активации и выход. История живёт в БД приложения или в Responses store, в модель попадает как текст. Пользователь не получает «свою» сеть и не дообучает общую.

🧒 Простыми словами

Это один общий движок на всех, как один калькулятор в библиотеке. Книга формул (веса) одна и на столе лежит открытой. У каждого читателя свой черновик (KV-кэш и история чата). Когда вы уходите, черновик выбрасывают, книга остаётся. Сосед не переписывает книгу вашим вопросом. Он может толкнуть стол (гонка в Redis, тайминг кэша, чужой батч чуть меняет округление). «Память» чата — это библиотекарь подкладывает вчерашние листочки обратно в окошко, а не впечатывает их в книгу. Вредные вопросы ловят на входе, в «внутреннем чутье» модели и на выходе; часть всё равно пролезает, поэтому поверх стоят лимиты и люди с bug bounty.

DR26-10-19-HUB-05-2154 · Grok 4.6 · Mac
