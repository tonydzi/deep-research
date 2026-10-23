# DR26-08-04-ZB-08-1744 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Провожу глубокое исследование: сначала — открытые инструменты и платные сервисы, затем метрики цитирования, каналы вклада и GDPR.Проверяю конкретные репозитории, платные сервисы и GDPR.# GEO-аналитика по origin-логам: что существует, что предсказывает, куда нести вклад

Дата среза: **2026-08-04**. Уровень: только проверяемые источники; отсутствие = явный минус.

Ваш замер (краул без индекса) **совпадает с тем, как рынок уже делит стадии**, но почти никто публично не связывает **частоту возврата на URL → цитирование** научным доказательством.

---

## 0. Карта трёх стадий (контекст вашего замера)

| Стадия | Что видно | Источник истины | Типичные UA (примеры) |
|--------|-----------|-----------------|------------------------|
| **Краул** | HTTP-запросы к origin/CDN | access.log / edge logs | GPTBot, ClaudeBot, PerplexityBot, OAI-SearchBot, CCBot, Bytespider, Google-Extended |
| **Индекс / retrieval corpus** | URL доступен движку для ответа | Bing Webmaster / site: / vendor index APIs (часто закрыты) | ≠ краул |
| **Цитирование** | URL/бренд в ответе LLM | prompt-monitoring (Profound, Peec…), ручной аудит | ChatGPT-User / Claude-User / Perplexity-User = fetch «на лету», не то же, что GPTBot |

**Established (практика индустрии):** JS-аналитика (GA и т.п.) **не видит** AI-краулеров — они не исполняют JS. Единственный надёжный origin-сигнал — server/edge logs.  
Источники: Cloudflare AI Crawl Control docs; Logwick README; Profound Agent Analytics («Uses server logs instead of JavaScript trackers»).

**Emerging:** деление AI-трафика на **training / search-index / user-fetch** (Logwick taxonomy; Vercel «three types of AI bot traffic», 2025-08; Profound Technical AEO).

---

## 1. Открытые инструменты: разбор логов именно AI-краулеров

### 1.1 Ближе всего к «GEO log analytics» (origin logs → bot × URL × revisit)

| Репозиторий | Что делает | Лицензия | Stars | Последний push (API) | Живость | Совпадение с ТЗ |
|-------------|------------|----------|-------|----------------------|---------|-----------------|
| [stani-chirk/logwick](https://github.com/stani-chirk/logwick) | Self-hosted: edge/JSONL → классификация GPTBot/ClaudeBot/ChatGPT-User/Perplexity и др.; training vs user-fetch vs search; per-path; SQLite dashboard localhost | **AGPL-3.0** (+ commercial) | **4** | **2026-06-10** | Низкая (мало звёзд, 1 maintainer, нет явной внешней PR-активности) | **Высокое**: AI taxonomy + URL + sessions. Вход: **JSONL**, не raw nginx combined «из коробки» |
| [SpeyTech/crawler-log-analyser](https://github.com/SpeyTech/crawler-log-analyser) | Python 3.11+, **nginx** combined/seo_crawl; search+AI bots; deploy anomalies; AI discovery window; spoof/rotation; llms.txt hits; text/md/json | **AGPL-3.0-or-later** | **0** | **2026-05-17** | Feature-freeze до **2026-08-31**; «PRs welcome, review slow» | **Высокое** для nginx; нет UI/recrawl-дашборда; операционный SEO + AI |
| [sungurkeremgezergun/seo-log-analyzer-tools](https://github.com/sungurkeremgezergun/seo-log-analyzer-tools) | Streaming Apache/Nginx multi-GB; 70+ bot families **incl. GPTBot/ClaudeBot/PerplexityBot/Bytespider**; reverse-DNS; 24 reports; React dashboard; claim 16GB/50.9M lines | SPDX **NOASSERTION** (проверить LICENSE в дереве) | **0** | **2026-07-27** | Один автор, свежие коммиты | **Высокое** по описанию; adoption ≈0; лицензия неясна без чтения файла |
| [arturseo-geo/mcp-crawl-parity](https://github.com/arturseo-geo/mcp-crawl-parity) | MCP: nginx logs → Googlebot vs AI parity ratio + GSC crossref | **MIT** | **1** | **2026-03-28** | Спящий | Частичное: parity, не full recrawl analytics |
| [AI-Advisors/ai-crawler-analytics](https://github.com/AI-Advisors/ai-crawler-analytics) | GTM **server-side tag** → GA4 (не парсер access.log) | **Apache-2.0** | **0** | **2026-07-31** | Микро | Альтернативный путь, не «по файлу логов» |
| [ipanalytics/CrawlerScope](https://github.com/ipanalytics/CrawlerScope) | Dashboard IP-intelligence search/AI/user-fetchers | NOASSERTION | **1** | **2026-08-04** | Новый | Смежно (IP ranges), не full log GEO |

**Общие OSS-анализаторы логов (без AI-taxonomy):**

| Инструмент | Лицензия | Stars | Push | Что даёт / чего нет |
|------------|----------|-------|------|---------------------|
| [allinurl/goaccess](https://github.com/allinurl/goaccess) | MIT | ~20.7k | 2026-07-20 | Живой general log analyzer; **нет** встроенной AI bot taxonomy / GEO recrawl |
| [GDM-Pixel/spider-lens](https://github.com/GDM-Pixel/spider-lens) | — | 0 | 2026-06-02 | SEO log audit; adoption 0 |

**Де-факто open method (не продукт):** bash `grep`/`awk` one-liners по UA → count by bot → top URLs. Документировано в industry blogs (Dec 2025–2026), не peer-reviewed.

### 1.2 Списки ботов (не log analyzer, но фундамент)

| Репозиторий | Лицензия | Stars | Push | Роль |
|-------------|----------|-------|------|------|
| [ai-robots-txt/ai.robots.txt](https://github.com/ai-robots-txt/ai.robots.txt) | **MIT** | **~4025** | **2026-08-04** | Живой список AI UA + nginx/htaccess configs; **мержит PR от посторонних** (14+ merged с 2026-05, samples: guest20, INXPRNCD, glyn) |
| [monperrus/crawler-user-agents](https://github.com/monperrus/crawler-user-agents) | MIT | ~1391 | 2026-08-03 | Общий crawler UA JSON |
| [arcjet/well-known-bots](https://github.com/arcjet/well-known-bots) | MIT | ~78 | 2026-04-24 | Fork well-known bots |

### 1.3 Платные / SaaS: что дают, чего нет в OSS

| Сервис | Что даёт (по публичным страницам) | Чего нет / ограничения vs open origin-tool | Источник |
|--------|-----------------------------------|--------------------------------------------|----------|
| **Cloudflare AI Crawl Control** (ex **AI Audit**, GA ~2025-08) | На **всех** планах Cloudflare: кто краулит, как часто, какой контент; block / robots enforcement / 402 pay-per-crawl | Только если трафик идёт через Cloudflare; не «пришёл со своим nginx log»; не prompt-level citation score | [CF docs](https://developers.cloudflare.com/ai-crawl-control/), [changelog](https://developers.cloudflare.com/ai-crawl-control/changelog/) |
| **Vercel** | AI bots managed ruleset: **log or deny**; Bot Directory; network-level stats в блогах | Не GEO dashboard по URL-recrawl на любом origin; WAF-контроль, не research tool | [Vercel Bot Management](https://vercel.com/docs/bot-management) (обн. 2026-07) |
| **Known Agents** (ex **Dark Visitors**) | Agent directory; **Agent Analytics** (real-time agent traffic); LLM referral tracking; auto robots.txt; WP plugin | SaaS, не OSS core; не «скачал и прогнал access.log offline» | [knownagents.com](https://knownagents.com/), rename post |
| **Profound Agent Analytics** | Server/CDN log ingest (CF, Akamai, Fastly, Vercel, AWS…); crawler who/when/how often; **spoof verification**; attribution to human traffic; **citation/content performance** claims; network benchmarking | Closed SaaS; SOC2/GDPR claims; цена enterprise-scale | [tryprofound.com/features/agent-analytics](https://www.tryprofound.com/features/agent-analytics) |
| **Scrunch** | AXP: AI-friendly parallel content at CDN; visibility case studies; **Sitecore acquisition** ~2026-06 (Bloomberg ~$225M — unconfirmed by both) | Не «log parser OSS»; optimization layer | industry comparisons 2026 |
| **Peec AI** | Prompt/brand visibility across ChatGPT/Perplexity/Gemini; citation frequency; MCP/API | **Основной продукт = answer-side**, не origin log engine. Сравнения противоречивы: Profound marketing пишет, что у Peec **нет** CDN/server crawl monitoring; third-party reviews иногда приписывают Peec «server-log crawl auditing» — **не сведено к одной первичной док-странице** | peec.ai; Profound vs Peec pages |
| **LogLens** (loglens.ai) | Commercial real-time log stream (CF/Vercel/Nginx/Apache) + bot signals | Closed | product site |
| **SEO Utils** Log File Analysis | Commercial: AI View panels (AI Answer / Assistant / Training buckets), bot×URL | Closed desktop/SaaS | help.seoutils.app |

### 1.4 Что в open-source **системно отсутствует** (gap)

| Gap | У платных | У OSS сейчас |
|-----|-----------|--------------|
| Real-time multi-CDN connectors | Profound, LogLens, CF | Нет зрелого |
| Bot spoof verification at scale (IP range + rDNS + ASN) | Profound | Частично claims в seo-log-analyzer-tools / crawler-log-analyser |
| **Citation ↔ crawl join** (этот URL краулили N раз → попал в ответ) | Profound marketing | **Не найдено** open tool, который это делает end-to-end |
| Cross-site benchmarking network | Profound Network | Нет |
| Зрелый проект (>500★, внешние мерджи PR, docs) в нише «AI-only log GEO» | — | **Нет** — только микро-репо + живые **списки** UA |

**Вердикт §1:** инструмент «ровно ваш» **начинает появляться** (Logwick, SpeyTech, seo-log-analyzer-tools), но **категории-лидера OSS нет**. Доминируют: (а) `grep`, (б) Cloudflare, если вы на CF, (в) Profound-класс SaaS.

---

## 2. Какая метрика реально предсказывает цитирование?

### 2.1 Таблица доказательности

| Утверждение | Уровень | Источник + дата | Что именно доказано |
|-------------|---------|-----------------|---------------------|
| Контентные вмешательства (цитаты, статистика, структура) меняют **visibility в generative engine responses** в controlled setup | **Established (academic)** | Aggarwal et al., *GEO: Generative Engine Optimization*, arXiv:2311.09735 (v1 2023-11, v3 2024-06; KDD 2024) | Оптимизация **текста** и visibility metrics; **не** origin recrawl frequency |
| AI-краулеры = значимый трафик; JS analytics слепа | **Established (ops)** | Vercel «Rise of the AI crawler» 2024-12; CF AI Crawl Control; industry log guides 2025–26 | Объём краула; не citation |
| Freshness / brand mentions / earned media коррелируют с AI mentions/citations | **Emerging (industry large-N)** | Ahrefs: brand visibility 75k brands (2025-12); AI Overview top-10 overlap 76%→38% (2026-03); freshness stats (2025) | **Off-site brand signals**, не «recrawl count URL» |
| Answer engines краулят чаще/быстрее classic SE на отдельных кейсах | **Emerging (vendor case)** | Conductor AI crawler activity / citation velocity materials 2026 | Наблюдения на своих/клиентских логах; **не** causal paper «recrawl → cite» |
| «Частота повторного краула **конкретного URL** предсказывает попадание в ответ Perplexity/ChatGPT/Gemini» | **Speculative / folk** | Vendor blogs (Profound Technical AEO: «how often they come back» as baseline signal; Conductor dashboards) | **Рекомендация измерять**, не published causal study |
| Краул ≠ индекс ≠ цитата | **Established (ваш замер + ops logic)** | Ваш контроль 04.08.2026 + разделение bot intents | Согласуется с industry taxonomy |

### 2.2 Что **не найдено** (важный отрицательный результат)

1. **Peer-reviewed paper**, где независимая переменная = *recrawl frequency of URL from origin logs*, зависимая = *presence of that URL in Perplexity/ChatGPT/Gemini answers*, с контролем confounds (domain authority, SERP rank, freshness, brand).
2. **Открытый датасет** «log recrawl × later citation» для replication.
3. **Vendor whitepaper с reproducible methodology** (sampling prompts, false-positive rate, bot verification) именно для causal recrawl→cite.

**Практический вывод:** recrawl — **necessary-ish signal** (страница должна быть fetchable) и **weak / unproven predictor** citation. Сильнее выглядят (по текущим large-N): brand web mentions, freshness, earned media, structural citability (GEO paper). Краул без индекса у вас — **прямое опровержение** наивной модели «краулили = уже в индексе/цитатах».

---

## 3. Куда принимать вклад (issue-matching + живость)

### 3.1 Живые двери (мерджи от outsiders за ~90 дней)

| Проект | Живость | Что уместно нести | Issue-matching (что закрыло бы / куда стыкуется) |
|--------|---------|-------------------|--------------------------------------------------|
| **[ai.robots.txt](https://github.com/ai-robots-txt/ai.robots.txt)** | ★~4k, push **сегодня**, **14+ merged PR** с 2026-05 от разных людей | **Не** полный analyzer; но **UA taxonomy**, metrics table, nginx regex — если инструмент опирается на их `robots.json` | Open: bot metadata gaps (#246 ExaSearchBot, #235 Amazonbot training…); **analyzer PR сюда не про PR product**, а синк списка |
| **[thedaviddias/llms-txt-hub](https://github.com/thedaviddias/llms-txt-hub)** | Активные community merges 2026-08 | Запись **инструмента** в каталог llms/AI-ready tools | PR «add GEO log analyzer» — по форме contrib, не code |
| **[AnswerDotAI/llms-txt](https://github.com/AnswerDotAI/llms-txt)** | ★~2.5k, Apache-2.0, 100+ open issues | Спека llms.txt; **не** log analytics core | Уместен **cross-link** / companion tool; отдельный analyzer — соседний репо |
| **[monperrus/crawler-user-agents](https://github.com/monperrus/crawler-user-agents)** | ★~1.4k, push 2026-08-03 | Дополнения AI UA | PR новых паттернов |
| Awesome GEO lists | | | |
| [amplifying-ai/awesome-generative-engine-optimization](https://github.com/amplifying-ai/awesome-generative-engine-optimization) | ★~469, push 2026-04 | Список tools | **Не найдено** merged outsider PR за 90д (выборка API = []) |
| [luka2chat/awesome-geo](https://github.com/luka2chat/awesome-geo) | ★~146, push 2026-04 | То же | Merged PR 90д: **не найдено** |
| [DavidHuji/Awesome-GEO](https://github.com/DavidHuji/Awesome-GEO) | ★~117, research focus, push 2026-07 | Ссылка на paper/tool | Research-oriented |
| **Микро log tools** (Logwick, SpeyTech, seo-log-analyzer-tools) | Низкие ★ | PR/фичи | **Adoption gate**: 0–4★, у SpeyTech freeze до 31.08.2026 — плохая «главная дверь» для mission visibility |
| Vendor cookbooks Anthropic / OpenAI / Google | | | **Не найдено** cookbook-репо вендоров, которое просит/принимает «origin AI crawl log analyzer» как first-party pattern |

### 3.2 Где **не** ждать «официальную дверь»

| Канал | Почему |
|-------|--------|
| Anthropic / OpenAI / Google official cookbooks | Публично — про API/agents/RAG, **не** про publisher-side crawl analytics |
| Cloudflare | Продукт closed; open path — blog/Radar, не «залейте analyzer в их monorepo» |
| Profound/Peec | Конкуренты SaaS; вклад = research share / integration, не core |

### 3.3 Стратегия вклада (issue-matching, кофаундер-логика)

1. **Ship собственный MIT/Apache tool** (AK-47: stdin nginx combined → bot family × URL × recrawl interval histogram → JSON/HTML).  
2. **Синхронизировать UA** с `ai.robots.txt` / `crawler-user-agents` (PR туда = живая дверь).  
3. **Подать** в: llms-txt-hub, 2–3 awesome-geo, topic `generative-engine-optimization`.  
4. **Не** ждать merge в «главный GEO log monorepo» — его **нет** (negative finding = greenfield opportunity).

---

## 4. Правовые и этические границы (публикация analyzer логов)

### 4.1 Established

| Факт | Источник |
|------|----------|
| **IP-адрес = personal data** under GDPR, если можно связать с лицом (в т.ч. dynamic IP с доп. данными) | ECJ **C‑70/10** (*Scarlet*), **C‑582/14** (*Breyer*); обзоры law.stackexchange / legal practice |
| Access logs с IP — обработка ПДн: нужна **правовая основа** (обычно legitimate interest security/ops), **transparency**, **retention**, минимизация | GDPR Art. 5, 6; industry guides (Sematext GDPR logging 2025) |
| Перед передачей логов **третьей стороне** (SaaS analyzer) — **DPA**, минимизация, предпочтительно **anonymize first** | GDPR Art. 28 + industry practice |

### 4.2 Как решают существующие проекты

| Проект | Подход |
|--------|--------|
| **Logwick** | Local-only SQLite, dashboard `127.0.0.1`, «never phones home»; README явно: IP+UA may be personal data; retention на операторе; AGPL + commercial |
| **SpeyTech crawler-log-analyser** | Offline script, no telemetry, no external services |
| **Profound** | Claims SOC2, GDPR, encryption, RBAC — **vendor processes logs** (trust + contract surface) |
| **GoAccess / general analyzers** | Обычно silent on GDPR; оператор сам отвечает |

### 4.3 Требования к **публикации анализатора** (не к чужим логам)

| Можно | Нельзя / осторожно |
|-------|---------------------|
| Код парсера, taxonomy, synthetic sample logs | Реальные access.log с IP/cookies/query tokens в fixtures |
| Docs: «по умолчанию truncate IPv4 /64 IPv6, hash IP, drop query PII» | Требовать upload логов на ваш сервер без opt-in + DPA |
| Default **local processing** | Хранить raw logs «навсегда» в примерах CI |

**Рекомендуемый default для OSS GEO log tool (emerging best practice):**
1. Input local only.  
2. Optional `--anonymize-ip` (zero last octet / HMAC with local salt).  
3. Strip `?`-query known PII patterns.  
4. Document retention for operators.  
5. Sample data: synthetic UAs + fake IPs.

---

## 5. Сводка: confidence board

| Вопрос | Ответ одной строкой | Confidence |
|--------|---------------------|------------|
| Есть ли зрелый OSS «AI crawler log GEO»? | **Нет зрелого**; есть 2–3 молодых (Logwick, SpeyTech, seo-log-analyzer-tools) + grep | High |
| Чем платят SaaS? | CDN integr., control plane, citation/brand products, spoof, network benchmarks | High |
| Recrawl URL → citation? | **Не доказано peer-reviewed**; industry folk + weak necessary signal | High (на negative) |
| Куда PR? | **ai.robots.txt** (список), **llms-txt-hub** / awesome lists (каталог); code = **свой репо** | High |
| GDPR? | IP в логах = ПДн; analyzer local + anonymize; raw logs не в git | High |

---

## 6. Чего найти **не** удалось (явный список)

1. Репозиторий с **>100★** и активным merge outsider PR, **специализированный** на AI-only origin log GEO analytics.  
2. Академическое causal исследование **recrawl frequency (origin) → LLM citation**.  
3. Официальный Anthropic/OpenAI/Google cookbook «measure AI crawlers from your logs».  
4. Open-source end-to-end join **server crawl events × answer citations** (как у Profound Agent Analytics + Answer Engine Insights в одном closed product).  
5. Единая правда Peec: «есть/нет server-log crawl audit» — **конфликт маркетинговых страниц**, первичный docs-раздел не сверен в этом проходе.  
6. Подтверждённый public API Dark Visitors/Known Agents для offline batch log parse (продукт — real-time Agent Analytics + directory).

---

## 7. Практический next step (если строить/вносить)

**Минимальный продукт, который закрывает дыру:**

```
nginx access.log → classify(UA, optional rDNS) → 
  metrics: hits, unique URLs, recrawl_interval_p50/p90 per (bot_family, url) →
  split training | search | user_fetch →
  HTML/JSON report + exit codes
```

- Лицензия: **MIT/Apache** (шире adoption, чем AGPL Logwick/SpeyTech).  
- Taxonomy: import from `ai.robots.txt` robots.json (contribute upstream).  
- Privacy: anonymize by default in public demos.  
- Positioning: **Stage-1 GEO observability** — честно «не citation», а «кто вернулся на какой URL».

Это **ровно** ваш measured insight (краул без индекса) и **ровно** то, чего публичная GEO-дискуссия недодаёт.

---

🎙 **Майк:** Идём верно: рынок продаёт «видимость в ответах», а origin-логи — слепая зона с 0–4★ OSS. Строить Stage-1 recrawl tool + список в awesome/`ai.robots.txt` — лучше, чем ждать «науки recrawl→cite», которой нет. Вопрос: цель — **свой флагман на GitHub** или **PR/список в чужой живой репо**? От ответа зависит MIT monorepo vs тонкий contrib.

---

🧒 **Простыми словами:**  
Представь: роботы-читалки (ChatGPT, Claude) ходят на сайт, как курьеры. В обычной «статистике сайта» их не видно — они не кликают кнопки. Видно только в «журнале двери» (логах). Готовых хороших бесплатных «счётчиков курьеров» почти нет — пара маленьких самоделок и куча дорогих сервисов. Никто по-научному не доказал: «если курьер чаще возвращается на страницу — её чаще цитируют». Краулили ≠ процитировали (у вас это уже замерено). Если делать свой счётчик — можно, но аккуратно: в журнале есть IP-адреса людей, это как паспортные данные, нельзя выкладывать сырые логи. Положить список роботов — в живой репозиторий `ai.robots.txt`; сам счётчик — скорее свой проект + списки «awesome GEO».
