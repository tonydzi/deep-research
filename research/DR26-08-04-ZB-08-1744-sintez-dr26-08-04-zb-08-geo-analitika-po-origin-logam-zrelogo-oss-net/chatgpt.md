# DR26-08-04-ZB-08-1744 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

## Краткий вывод

Да, открытые анализаторы уже существуют. Самое близкое попадание в задачу — **Logwick**: локальная обработка origin/CDN JSONL, классификация AI-трафика по назначению, разбивка по URL и времени. Однако репозиторий пока фактически представляет собой однокоммитный публичный импорт, поэтому назвать его зрелым сообществом нельзя.

Главный незакрытый пробел — не подсчёт AI-ботов сам по себе, а воспроизводимая связка:

`подтверждённый бот → конкретный URL → интервалы возврата → индекс/доступность → фактические цитирования по стабильному набору запросов`.

Исследования, где **частота повторного краула URL статистически предсказывает последующее цитирование**, найти не удалось. Пока это гипотеза, а не established-метрика.

Важная поправка к исходным данным: **Google-Extended не имеет собственного HTTP User-Agent**. Это только управляющий токен `robots.txt`; фактическое сканирование выполняют другие Google UA. Если в access log буквально присутствует `Google-Extended`, это либо самодекларация стороннего клиента, либо ошибка классификатора, но не подтверждённый официальный crawler Google. [Google, июль 2026](https://developers.google.com/crawling/docs/crawlers-fetchers/google-common-crawlers).

---

# 1. Открытые инструменты

## Established: общие log analyzers

| Проект | Что умеет | Лицензия и живость | Что отсутствует |
|---|---|---|---|
| [GoAccess](https://github.com/allinurl/goaccess) | Nginx/Apache-логи, URL, время, UA, crawler-only mode, HTML/JSON/CSV, real-time | MIT; активен, выпуск 1.10.2; открытые issues создавались в мае–июне 2026 | Нет отдельной семантики training/search/user-fetch, нет citation join, нет готовой метрики recrawl interval |
| [Matomo Log Analytics](https://matomo.org/blog/2020/05/11-ways-piwik-analytics-helps-protect-visitors-privacy/) | Импорт серверных логов, страницы, визиты, privacy controls | GPL; зрелый проект | AI-краулеры не являются отдельной предметной моделью; нужен собственный UA-сегмент |
| [crawl-log-toolkit](https://github.com/nimajafari/crawl-log-toolkit) | Парсинг разных логов, DuckDB/SQL, top paths, crawl budget, IP-проверка Googlebot/Bingbot | OSS-репозиторий, активность обнаружена в 2026 | Ориентирован на традиционные search crawlers, а не AI-ботов и цитирование |

GoAccess уже технически позволяет отфильтровать crawler traffic и посмотреть requested files, но его открытый [issue #2935](https://github.com/allinurl/goaccess/issues/2935) — «Applebot not identified as Crawlers», 23.04.2026 — показывает, что даже базовая сигнатурная база требует постоянного сопровождения. Это возможная точка входа для более общего реестра AI-agent UA, но не готовый GEO-модуль.

## Emerging: специализированные AI-log analyzers

| Проект | Соответствие задаче | Лицензия | Последняя подтверждённая активность | Оценка |
|---|---|---:|---|---|
| [stani-chirk/logwick](https://github.com/stani-chirk/logwick) | JSONL origin/CDN → SQLite; AI training / search-index / user-fetch; vendor→URL Sankey; time series и sessions | AGPL-3.0, коммерческая dual license | Публичный репозиторий содержит **1 commit**; версия v1.1.0 была опубликована весной 2026, точный timestamp коммита GitHub в доступной выдаче не отдал | Наиболее точное функциональное совпадение, но пока не доказана community-живость |
| [galaar-org/AgentECHO](https://github.com/galaar-org/AgentECHO) | Next.js middleware → Go/ClickHouse; top routes, bots и time series; GPTBot, ClaudeBot, OAI-SearchBot, PerplexityBot, CCBot и др. | MIT | 13 commits, 4 stars, 0 issues/PR на момент проверки; точную дату последнего commit получить не удалось | Рабочий прототип, но не анализатор существующих nginx-файлов; требует инструментирования приложения |
| [Auriti-Labs/geo-optimizer-skill](https://github.com/Auriti-Labs/geo-optimizer-skill) | `geo logs --file access.log`, activity, top pages, trends; отдельно умеет GEO-аудиты | MIT | Последний видимый commit **17.07.2026** | Живой проект, но logs — один модуль большого open-core GEO-продукта; нет доказанной crawl→citation модели |
| [geo-data-analysis.py](https://gist.github.com/henu-wang/c8d15627bf1023794db68313044b7a73) | Nginx/Apache combined log, AI UA, frequency, top pages, CSV и графики | Лицензия не указана, значит безопасно считать код «all rights reserved» | Создан 09.03.2026 | Демонстрационный скрипт, не полноценный OSS-проект |

Logwick прямо и корректно оговаривает, что измеряет **upstream fetch signal, а не позиции или цитаты**; данные остаются в локальном SQLite, dashboard слушает `127.0.0.1`, telemetry/cloud sync отсутствуют. [Документация Logwick, 2026](https://r-sun.ai/products/logwick).

У AgentECHO есть серьёзный privacy-дефект по умолчанию: опубликованная схема ClickHouse сохраняет полный `ip String` и raw User-Agent без описанной анонимизации или retention policy. Это лучше исправлять до production-использования.

### Что считать «возвратом»

Ни один найденный проект не оформляет возврат как полноценную page-level метрику. Практическая модель должна вычислять хотя бы:

- `crawl_count(url, verified_agent, window)`;
- `median/p50/p90 recrawl_interval`;
- `days_since_last_fetch`;
- `unique_crawl_days`;
- долю 200/304/3xx/4xx/5xx;
- content-change-aware recrawl: изменился ли `ETag`, `Last-Modified` или content hash;
- раздельно training, search index и user-triggered fetch.

Просто `hits / URL` смешивает burst crawl, плановую переиндексацию и пользовательские retrieval-запросы.

---

# 2. Платные сервисы

| Сервис | Что подтверждено | Чего нет или что закрыто |
|---|---|---|
| [Cloudflare AI Crawl Control](https://developers.cloudflare.com/ai-crawl-control/) — бывший AI Audit | Dashboard crawler activity/request patterns, allow/block по crawler, robots.txt compliance, enforcement; доступен на всех Cloudflare plans. Документация обновлена 23.04.2026 | Cloudflare-only; нет сырых origin-логов, открытого классификатора и воспроизводимого crawl→citation анализа |
| [Vercel Bot Management](https://vercel.com/kb/guide/how-to-utilize-vercels-bot-management-features) | BotID/classification headers, Firewall Traffic analytics, managed rules для AI bots | Это security/traffic surface, а не GEO attribution; нет публичной модели цитирования |
| [Dark Visitors Agent Analytics](https://darkvisitors.com/docs/analytics) | Realtime agents/crawlers, AI referrals, integrations с Cloudflare/Fastly/Akamai/AWS/backend | Закрытый каталог и SaaS pipeline; по открытой документации не доказана корреляционная модель recrawl→citation |
| [Scrunch](https://scrunch.com/faqs/category/features) | Prompt monitoring, URL-level citations, brand/competitor sources; Enterprise добавляет agent traffic analytics | Данные и алгоритмы закрыты; crawl и citations показаны рядом, но публичной статистической валидации связи нет |
| [Profound](https://www.tryprofound.com/features) | Answer Engine Insights, citations/mentions/sentiment/competitors и Agent Analytics для crawler traffic | Enterprise SaaS; не публикует origin-level методологию и модель, связывающую повторный fetch с цитированием |
| [Peec](https://docs.peec.ai/understanding-your-performance) | Prompt-based visibility, citation/source analytics, share of voice; citation rate beta | По найденной документации нет origin-log crawler analytics; это downstream rank/visibility tracker |
| [LLM Pulse](https://llmpulse.ai/features/agent-analytics/nginx) | Прямая загрузка nginx combined и `.gz`, bot/vendor totals и time series; per-URL drilldown на Enterprise | Не open source; логи загружаются внешнему процессору; метод верификации UA/IP и retention требуют отдельной проверки |

Таким образом, преимущество SaaS — не более точный `grep`, а:

1. регулярный запуск запросов в несколько answer engines;
2. сохранение ответов и точных citation URLs;
3. конкурентный share of voice;
4. соединение с GA4/referrals/conversions;
5. managed bot/IP intelligence.

Преимущество OSS — локальность, доступ к raw событиям, воспроизводимость и возможность корректно разделить стадии.

---

# 3. Какая метрика предсказывает цитирование

## Established

| Результат | Источник и дата | Что доказано |
|---|---|---|
| Контентные изменения могут повышать visibility в синтетической generative engine | [GEO: Generative Engine Optimization, KDD 2024](https://dblp.org/rec/conf/kdd/AggarwalMRKND24) | Эксперименты с цитатами, статистикой, quotations и стилем; **не анализ origin logs** |
| Google rank является сильным observational predictor цитирования | [Lee, SSRN, апрель 2026](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6240278) | На исследованной выборке log(Google position) дал cross-validated AUC 0.802; это preprint/observational study, не доказательство причинности |
| Citation selection и absorption — разные стадии | [From Citation Selection to Citation Absorption, апрель 2026](https://arxiv.org/abs/2604.25707) | 602 prompts, 21 143 citations, 18 151 fetched pages и 72 page features; не исследуется периодический фоновый recrawl |
| Платформы используют разные роботы для разных стадий | [Anthropic, обновлено июль 2026](https://support.anthropic.com/en/articles/8896518-does-anthropic-crawl-data-from-the-web-and-how-can-site-owners-block-the-crawler), [Perplexity, 2026](https://docs.perplexity.ai/docs/resources/perplexity-crawlers) | ClaudeBot ≠ Claude-SearchBot ≠ Claude-User; PerplexityBot ≠ Perplexity-User |

Для практической аналитики это означает: **user-triggered fetch конкретного URL непосредственно перед наблюдаемым ответом** гораздо ближе к цитированию, чем частота GPTBot/ClaudeBot training crawl. Но даже такой fetch не гарантирует, что URL попал в финальный ответ.

## Emerging

Наиболее правдоподобные предикторы по совокупности найденных работ:

- попадание страницы в retrieval candidate set;
- rank/authority в используемом поисковом индексе;
- соответствие конкретному prompt/query intent;
- ясная извлекаемая формулировка ответа;
- наличие проверяемых данных и первичных источников;
- свежесть для time-sensitive запросов;
- фактический user-fetch/search-fetch около времени ответа.

## Speculative

- высокий общий crawl count;
- малый медианный recrawl interval;
- `llms.txt` сам по себе;
- наличие schema markup само по себе;
- попадание GPTBot/ClaudeBot training crawler;
- число обращений CCBot.

### Чего найти не удалось

Не найдено ни одного peer-reviewed или публично воспроизводимого исследования, где:

1. берутся origin logs нескольких сайтов;
2. для каждого URL измеряется повторный crawl одним и тем же подтверждённым агентом;
3. независимо запускается фиксированная панель prompts;
4. измеряются последующие citations;
5. контролируются rank, backlinks, freshness, content type и engine;
6. публикуется коэффициент/odds ratio между recrawl frequency и citation probability.

Поэтому утверждение «чаще возвращается → чаще процитирует» на 04.08.2026 следует маркировать как **folk hypothesis**.

---

# 4. Куда подавать вклад

Строгое условие «мерджили PR посторонних после 06.05.2026» оказалось самым слабым местом исследования: у релевантных нишевых репозиториев либо почти нет PR, либо видны только commits владельца. Я не буду подменять это общей активностью.

| Кандидат | Реальная пригодность | Issue matching | Проверка критерия живости |
|---|---|---|---|
| [Auriti-Labs/geo-optimizer-skill](https://github.com/Auriti-Labs/geo-optimizer-skill) | Лучший кандидат для интеграции parser/recrawl/citation export | Новый модуль мог бы расширить существующий `geo logs`; точного открытого issue под это не найдено | Проект активен до 17.07.2026, но внешний merged PR за 90 дней подтвердить не удалось |
| [GoAccess](https://github.com/allinurl/goaccess) | Хорош для общего UA registry, crawler panels, privacy-safe exports | [#2935 Applebot classification](https://github.com/allinurl/goaccess/issues/2935), [#2945 interactive filtering](https://github.com/allinurl/goaccess/issues) | Issues и maintainer responses активны; точный внешний merged PR за окно не подтверждён |
| [DavidHuji/Awesome-GEO](https://github.com/DavidHuji/Awesome-GEO) | Уместен после появления отдельного качественного репозитория и методологии | Open issues отсутствуют; README приглашает PR | 2 открытых PR; подтверждённого стороннего merge за 90 дней не найдено |
| [amplifying-ai/awesome-generative-engine-optimization](https://github.com/amplifying-ai/awesome-generative-engine-optimization) | Имеет отдельные Monitoring & Analytics и Tools sections | Подходящего открытого issue не найдено | Недостаточно данных, чтобы подтвердить заданный критерий |
| [AnswerDotAI/llms-txt](https://github.com/answerdotai/llms-txt) | Уместен только эксперимент: измерять обращения к `/llms.txt` относительно обычных URL | Открытого issue, которое закрывал бы полный log analyzer, не найдено | Это specification repo, не analytics project |
| [OpenAI Cookbook](https://github.com/openai/openai-cookbook) | Возможен независимый notebook про анализ логов, но он не относится напрямую к OpenAI API | Matching issue не найден | Проект живой, но без предварительного issue такой PR, вероятно, будет вне scope |
| Anthropic/Google cookbooks | Аналогично: максимум vendor-specific identification/export example | Matching issues не найдены | Подтверждения приёма такого scope нет |

### Практический маршрут

Наиболее реалистично:

1. Выпустить самостоятельный репозиторий под MIT/Apache-2.0 с CLI и DuckDB/SQLite.
2. Открыть в GEO Optimizer proposal issue на импорт или адаптер.
3. Отдельно предложить GoAccess обновляемый AI crawler registry и export.
4. После первого стабильного release отправить листинговые PR в Awesome-GEO и Monitoring & Analytics awesome-list.
5. Vendor cookbooks рассматривать только для коротких vendor-specific recipes, не как основной дом проекта.

Нельзя честно назвать ни один найденный awesome/cookbook repository гарантированным местом приёма по вашему строгому критерию. Это отрицательный, но полезный результат.

---

# 5. Правовые и этические границы

Это не юридическая консультация, но архитектура инструмента должна исходить из того, что raw logs могут содержать персональные данные.

| Требование | Основание | Что реализовать |
|---|---|---|
| IP может быть personal data | [CJEU C‑582/14 Breyer, 19.10.2016](https://eur-lex.europa.eu/legal-content/EN/ALL/?uri=celex%3A62014CJ0582) | Не публиковать и не загружать raw IP по умолчанию |
| Purpose limitation и lawful basis | [GDPR, Article 5/6 и Recital 47](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32016R0679) | Документировать цель; для operational/security analytics часто оценивают legitimate interest, но нужен balancing test |
| Data minimisation и storage limitation | [European Commission GDPR principles](https://commission.europa.eu/law/law-topic/data-protection/rules-business-and-organisations/principles-gdpr/overview-principles/what-data-can-we-process-and-under-which-conditions_en) | Отбрасывать IP после верификации; хранить агрегаты; configurable TTL |
| Transparency | [EC guidance по Articles 13–14](https://commission.europa.eu/law/law-topic/data-protection/rules-business-and-organisations/principles-gdpr/what-information-must-be-given-individuals-whose-data-collected_en) | Указать logging/analytics, цели, сроки, processors и transfers в privacy notice |
| Security | GDPR Art. 5(1)(f), 32 | Local-only default, filesystem permissions, encryption at rest, no public dashboard |
| Processor/transfer control | GDPR Art. 28, 44+ | Для SaaS-загрузки — DPA, subprocessor list, region и transfer mechanism |

Хороший default:

- IP используется потоково для проверки официального диапазона и затем удаляется;
- сохраняется `verified_bot=true/false`, vendor, bot, timestamp bucket, normalized path, status и bytes;
- query strings и fragments отбрасываются или редактируются, поскольку там бывают email, tokens и search terms;
- raw UA можно заменить classifier result и коротким hash;
- retention raw events 7–30 дней, aggregates — дольше;
- экспорт по умолчанию не содержит IP, cookies, authorization headers, query values и referrer query strings;
- salted hash следует считать **псевдонимизацией**, а не автоматической анонимизацией;
- публичные sample logs должны быть синтетическими.

Matomo показывает зрелый набор практик: IP masking, отключение visitor profiles, удаление старых raw events и configurable retention. [Matomo privacy configuration, 2026](https://matomo.org/faq/general/configure-privacy-settings-in-matomo/). Logwick выбирает local-only модель, но прямо оставляет legal basis, retention и access control пользователю — то есть self-hosting уменьшает передачу данных, но сам по себе не делает обработку GDPR-compliant.

---

## Итоговая классификация

**Established**

- crawl, index/retrieval и citation — разные стадии;
- тип бота критически важен;
- URL-level log analytics технически давно возможна;
- search rank/relevance и page characteristics связаны с цитированием;
- IP и некоторые другие поля логов создают GDPR obligations.

**Emerging**

- специализированная AI taxonomy;
- соединение crawler logs с prompt/citation monitoring;
- user-fetch как сильный upstream diagnostic signal;
- локальные DuckDB/SQLite pipelines;
- verified UA + published IP ranges там, где vendor их предоставляет.

**Speculative**

- recrawl frequency как предиктор citation probability;
- training crawler activity как сигнал скорого цитирования;
- `llms.txt` или schema как самостоятельный ranking factor;
- буквальная классификация `Google-Extended` по access-log UA.

Самая ценная новая работа здесь — не ещё один dashboard, а открытый longitudinal dataset и инструмент, который проверит гипотезу `recrawl interval → citation` с контролем смешивающих факторов.
