# DR26-08-05-ZB-03-1621 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Готовлю research brief с опорой на первичные источники: начну с юридического риска «QQQ», параллельно соберу данные по доменам, находимости и китайской практике.# RESEARCH BRIEF: короткое имя / домен для независимой AI-лаборатории

**Дата:** 2026-08-05 · **Цель:** решение «менять ли имя и домен» · **Покупка не делается**  
**Дисклеймер:** это **общая информация, не юридическая консультация**. Регистрации товарных знаков, UDRP и риски dilution требуют проверки у поверенного по TM (США + целевые юрисдикции).

---

## 1. Прямая рекомендация (три варианта)

| # | Вариант | Вердикт | Уверенность |
|---|---------|---------|-------------|
| **(в)** | **Третий путь: неологизм 5–8 букв (как Anthropic / Cohere / Mistral), дешёвый .ai или .com** | **Лучший** | **high** |
| **(а)** | Оставить **Palo Alto AI Research Lab** + `наш сайт` | **Приемлемо на 3–12 мес.**, пока копите артефакты | **med-high** |
| **(б)** | Перейти на **QQQ / QQQ.AI** | **Не делать** | **high** |

### Почему не QQQ (кратко)
1. **Юридический:** живой знак Nasdaq (класс 36), знаменитый ETF — dilution + UDRP-риск на домене; см. блок 1.  
2. **Находимость:** «QQQ» = тикер с гигантским search/LLM-смыслом. Короткость **ухудшает** cold discovery, а не улучшает.  
3. **Цена:** `qqq.ai` на вторичке **$154 498** (ask, 2026-08-05) — при нулевой SEO-истории это статусный расход, не рычаг найма.  
4. **Гипотеза «уникальное слово = нас найдут AI»** — **опровергается вашим же замером** (Perplexity: 10/10 GitHub/курируемые списки) и внешней литературой по LLM-citations: цитируют **артефакты + авторитетные площадки**, не длину бренда.

### Почему (в) лучше (а) и (б)
- Неологизм снимает коллизию с ETF **и** с географической/generic-выдачей «Palo Alto AI lab».  
- Регистрация: десятки–сотни USD/год, не $150k+.  
- Frontier-рекрутеры смотрят GitHub, papers, issues — **как Anthropic/Cohere/Mistral**, у которых имена не 3-буквенные и не куплены за миллионы.  
- **Оценка:** короткое имя — следствие статуса (Китай, z.ai), не причина заметности у hiring-команд.

**🎙 Майк:** тратить ~$150k и брать юридический хвост ради внутренней симпатии к «QQQ» — это статус, не GTM. Магнит для acqui-hire = публичный код + issue-match, не тикер.

---

## 2. Блок 1 — ЮРИДИЧЕСКИЙ ВЕРДИКТ «QQQ»

> **Общая информация, не legal opinion.** Перед любым использованием «QQQ» как бренда/домена — clearance у TM-поверенного.

### 2.1. Факт: QQQ = тикер ETF + товарный знак Nasdaq

**Установленный факт:** Invesco QQQ Trust, Series 1 — ETF на Nasdaq-100; в проспектах прямо: *«NASDAQ … and QQQ are trade/service marks of The Nasdaq Stock Market, Inc. … licensed for use by Invesco»*  
- Источник: [Invesco QQQ product page / disclosure](https://www.invesco.com/us/en/financial-products/etfs/invesco-qqq-trust-series-1.html) (проверено 2026-08-05).

### 2.2. USPTO — конкретная регистрация (открыта TSDR)

| Поле | Значение |
|------|----------|
| Mark | **QQQ** |
| US Serial | **76462475** |
| US Registration | **2898662** |
| Filing | 2002-10-21 |
| Registration | 2004-11-02 |
| Status | **LIVE / Issued and Active** — renewed **2025-03-06** (2-е 10-летнее продление) |
| Owner | **NASDAQ, INC.**, 151 W. 42nd Street, New York, NY 10036 |
| Mark type | Service Mark, Principal Register, standard characters |
| **Class** | **036 only** |
| Goods/services | *«Financial services, namely, providing and administering open-ended unit investment trusts in the nature of exchange-traded funds»* |
| International | IR **1925407** (Madrid, linked) |

**Прямая запись TSDR:**  
`https://tsdr.uspto.gov/statusview/sn76462475`  
(страница сгенерирована TSDR 2026-08-05 17:31 EDT; owner, class 036, wording — с этой страницы).

**Ключевой факт:** регистрация **не** в классах 9 (downloadable software) и **не** в 42 (SaaS / scientific & tech services).  
**Оценка (не факт):** для AI-лаборатории «чистый» class-overlap **слабый**.  
**Но:** famous-mark dilution (Lanham Act §43(c)) и brand confusion **не требуют** совпадения класса — это отдельная ось риска.

### 2.3. EUIPO / другие
Полный EUIPO-dump в этом прогоне **не выгружен** (WIPO BrandDB — captcha).  
**Факт:** у USPTO-записи есть linked International Registration **1925407** → Madrid-путь возможен; **конкретный статус в EU нужно досмотреть у поверенного**.  
**Данных нет** (в этом отчёте) по полному списку EU-регистраций «QQQ».

### 2.4. Практика защиты: UDRP / иски

| Вопрос | Результат поиска |
|--------|------------------|
| UDRP WIPO по доменам *именно* «qqq.*» от Nasdaq/Invesco | **Конкретных решений с номерами в открытом full-text поиске не найдено** («данных нет» по списку дел) |
| Применяется ли UDRP к `.ai`? | **Да** — WIPO: `.AI` в списке ccTLD, принявших UDRP ([WIPO domain disputes](https://www.wipo.int/en/web/amc/domain-name-disputes/index)) |
| Типичная стоимость UDRP | Complainant: **~$1 500** single panelist (1–5 domains) у WIPO; 3-member ~$4 000; priority ~$4 000 ([Abion / WIPO fees summary](https://abion.com/wipo-updates-domain-dispute-services/), CooleyGo) |
| Срок | Обычный ~1–2 мес.; expedited — до ~1 мес. (кейс 26 дней, GigaLaw 2026-07) |
| Исход статистически | ~**80–90%** transfer/cancel в пользу complainant, часто из-за default respondent (Finnegan; WIPO 2017 ~92% transfer) |

**Оценка риска UDRP для bona fide AI-lab:**  
- **Identical mark** к известному TM — легко.  
- **Rights/legitimate interests** — если реальная лаборатория, не parking/продажа «под ETF» — **защитимо**.  
- **Bad faith** — при регистрации *после* известности QQQ-ETF и ask $150k+ на qqq.ai (сейчас у текущего владельца) панели часто смотрят на fame. Для *вас* как покупателей: покупка «известного» 3-letter + использование рядом с tech/AI может выглядеть exploitative.  
**Спекуляция:** вероятность UDRP *немедленно* — низкая (Nasdaq не охотится на каждую лабораторию). Вероятность *если* бренд «выстрелит» или появится путаница с финансами — **средняя**.

### 2.5. Tencent / QQ

**Факт:** Tencent держит бренд **QQ** (мессенджер), массовые TM; публичные источники указывают, напр., US reg. **2972934** на QQ (Tencent Holdings) — через агрегаторы ([Trademarkia summary](https://www.trademarkia.com/qq-78197345); первоисточник USPTO не выгружен в этом прогоне из‑за API-лимитов).  
**Оценка:** риск Tencent к **«QQQ»** (не «QQ») **ниже**, чем Nasdaq: 3Q ≠ 2Q, другой market.  
**Остаточный риск:** в Азии/у китайских краулеров «Q»-кластер ассоциируется с Tencent; путаница бренда, не прямой class-9/42 claim.

### 2.6. Что происходит с маленькой компанией при возражении

| Сценарий | Деньги | Время | Типичный исход |
|----------|--------|-------|----------------|
| Cease & desist | $2–15k lawyer review/response | недели | Часто rebrand / settlement |
| UDRP | $1.5–5k + attorney $5–20k | 1–3 мес. | Transfer domain if bad faith found |
| Federal TM/dilution suit | $50k–$500k+ | 1–3+ лет | Settlement / injunction / rebrand |

**Факт:** UDRP **не** решает infringement в полном объёме, но **забирает домен**.  
**Оценка:** для лаборатории 4 человека проигрыш UDRP = потеря домена + rebrand; даже «победа» = distraction.

### 2.7. Вердикт блока 1

| Ось | Уровень | Что создаёт риск |
|-----|---------|------------------|
| Class 9/42 overlap (регистрация) | **Низкий** | Регистрация **только 036** |
| Famous mark / dilution | **Средний–высокий** | QQQ = один из самых узнаваемых ETF-тикеров мира |
| UDRP на `qqq.ai` / `qqq.com` | **Средний** | Identical + fame; .ai под UDRP |
| Tencent QQ | **Низкий–средний** | Косвенная ассоциация, не 1:1 |
| **ИТОГО для AI-лаборатории** | **СРЕДНИЙ–ВЫСОКИЙ** | Не «запрещено классом 9», а **famous-mark + невозможность clean brand search + цена домена** |

**Прямо:** риск создаёт **не** «мы в class 36», а **(1)** знаменитость QQQ, **(2)** identical domain, **(3)** вечная путаница с финансами, **(4)** невозможность зарегистрировать свой TM «QQQ» в tech без office action/отказа.

---

## 3. Блок 2 — Доступность и цена коротких доменов

### 3.1. Занятость 3-буквенных

| Зона | Статус | Источник / дата |
|------|--------|-----------------|
| **LLL.com** | **100% заняты** (все 17 576 letter-combos); last free ~1997–2000 | [Atom blog](https://www.atom.com/blog/are-any-3-letter-domains-available/) (2024+); industry consensus; 26³=17 576 |
| **LLL.ai** | Почти все «хорошие» на вторичке; ~1 000 awkward ещё free (NamePros sweep **2025-11-30**) | [BoldDomains 2026-07-21](https://bolddomains.com/blog/3-letter-ai-domains-for-sale) |
| LLL.io / .co / .org | Премиум-комбы заняты; «мусорные» иногда free | **оценка** (не полный census) |

### 3.2. Реальные сделки (даты + суммы)

**LLL.com (публичные, 2025–2026):**  
Источник: [Embrace / NameBio-derived](https://www.embrace.com/recent-3-letter-com-domain-name-sales/), [Strategic Revenue 2026-04](https://www.strategicrevenue.com/how-rare-are-3-letter-com-domain-sales-a-5-year-data-analysis/)

| Домен | Цена | Год |
|-------|------|-----|
| NAS.com | $1 250 000 | 2026 |
| TXT.com | $502 250 | 2026 |
| Bar.com / Pub.com | $500 000 | 2026 |
| HSM.com | $550 000 | 2025 |
| UIG.com | $280 000 | 2025 |
| Типичный диапазон LLL.com | **~$100k–$550k**, outliers выше | 2025–26 |

**.ai premium (DNJournal YTD 2026, через 2026-07):**  
Источник: [DNJournal YTD charts](https://www.dnjournal.com/ytd-sales-charts.htm)

| Домен | Цена | Когда / где |
|-------|------|-------------|
| Bot.ai | **$1 200 000** | Sedo, ~2026-03 |
| Genesis.ai | $400 000 | Atom, ~2026-04 |
| Lotus.ai | $400 000 | Brandforce, ~2026-02 |
| Free.ai | $350 000 | Atom, ~2026-04 |
| Neo.ai | $275 000 | ~2026-05 |

**3-letter .ai market range (оценка рынка, не одна сделка):**  
- Community: **$20k–$200k+** (NamePros 2026-04)  
- Editorial: pronounceable LLL.ai often **$50k–$150k** ([BoldDomains 2026-07](https://bolddomains.com/blog/3-letter-ai-domains-for-sale))  
- `qqq.ai` ask **$154 498** — **внутри** этого коридора (не «дешёвый outlier»).

**Рекорд категории:** AI.com **$70 000 000** (2026-02, FT / GetYourDomain PR).

### 3.3. Регистрация / продление `.ai`

| Параметр | Цифра | Источник / актуальность |
|----------|-------|-------------------------|
| Min term | **2 года** | Registry policy (Name.com, GoDaddy, etc.) |
| Retail reg (пример) | **~$80/yr** → $159.96 / 2 yr (Namecheap) | [Namecheap .ai](https://www.namecheap.com/domains/registration/cctld/ai/) — **2026-08** |
| Retail renewal (пример) | **~$115/yr** → $229.96 / 2 yr (Namecheap) | same |
| Wholesale hike | +$10/yr (~+$20 per 2-yr cycle) с **2026-03-05** | [Domain Name Wire 2026-02-02](https://domainnamewire.com/2026/02/02/ai-domain-name-prices-going-up-20/) |
| vs .com | .com ~$10–15/yr | industry standard |

### 3.4. Статус `qqq.ai` и `qqq.com` (проверено 2026-08-05)

| | **qqq.ai** | **qqq.com** |
|--|------------|-------------|
| Занят? | **Да** | **Да** |
| Created | **2025-04-08** | **1995-04-24** |
| Expires | **2027-04-08** | **2031-04-25** |
| Registrar | Namecheap | GoDaddy |
| Owner | Privacy (Withheld for Privacy ehf, IS) | Domains By Proxy |
| Live use | **302 → DomainEasy sale page** | parking/privacy; публичный сайт бренда лаборатории **не** |
| Публичная цена | **Buy now $154 498 USD** | **скрыта** (whois privacy; make-offer через GoDaddy) |

WHOIS: `whois.nic.ai` / `whois.godaddy.com` (CLI 2026-08-05).  
Sale: [domaineasy.com/buy-domain/qqq.ai](https://www.domaineasy.com/buy-domain/qqq.ai) — *«QQQ.ai is for sale… $154498 USD»*.

### 3.5. Дешёвые альтернативные модели (работают)

1. **Неологизм + first-reg .ai/.com** — Anthropic, Cohere, Mistral, Perplexity: **не** LLL, **не** secondary millions.  
2. **5–7 букв brandable .ai** — secondary часто $1k–$20k, не $150k.  
3. **Брендированная зона** (`lab.yourname.org`, GitHub org) — для hiring **достаточна**, если артефакты сильные.  
4. **Не покупать** 3-letter ради vanity.

---

## 4. Блок 3 — Находимость уникального слова

### 4.1. Холодный старт выдуманного слова

**Установленное (паттерн):** LLM/search «приклеивают» сущность к имени через **повторные упоминания на авторитетных узлах** (Wikipedia, GitHub, arXiv, news, Reddit), не через «короткость».  
**Замеров «N недель до stable resolve для brand X»** с peer-reviewed rigor для *лабораторий* — **данных нет**.  
**Оценка:** неологизм без артефактов = 0 в ответах; QQQ с артефактами = всё равно «ETF» в топ-intent.

### 4.2. Как LLM выбирают цитаты (доказательное)

| Наблюдение | Источник | Тип |
|-------------|----------|-----|
| ChatGPT top sources: Wikipedia ~**47.9%** of top-10 | [Discovered Labs / Profound citation patterns](https://discoveredlabs.com/blog/ai-citation-patterns-how-chatgpt-claude-and-perplexity-choose-sources) (2025-12) | established (vendor study) |
| Perplexity: Reddit heavy (~**46.7%** top) | same | established (study) |
| **67%** ChatGPT top-1000 cited pages = original research / first-hand / academic | Ahrefs via [Surfer 2026-02](https://surferseo.com/blog/llm-citations/) | established |
| Only ~**12%** AI citations overlap Google top-10 | Discovered Labs | emerging |
| Brand authority + multi-platform presence сильнее «красивого домена» | industry GEO analyses 2025–26 | emerging |

**Ваша гипотеза «короткое уникальное → AI нас найдёт»:**  
**Частично неверна.** Ваш замер (10/10 GitHub/lists) **совпадает** с внешними данными: планка = **атрибутируемый артефакт на high-trust surface**, не «сайт компании» и не «3 буквы».

**Имя/домен** играют роль только как **entity string** *после* того, как entity уже co-occurs с артефактами. Сами по себе — почти нет.

### 4.3. Риск неоднозначности (тикер)

**Факт:** поиск «QQQ» в Google/finance = Invesco QQQ, charts, holdings.  
**Оценка:** brand query «QQQ AI lab» конкурирует с триллионами impressions ETF-контента — **убивает unprompted discovery**.  
**Кейсы «тикер убил бренд»:** прямых peer-reviewed «company X died from ticker collision» мало; есть **entity ambiguity** как SEO-паттерн ([Wise Digital 2026-01](https://www.wisedigitalpartners.com/learn/blog/seo/seo-risks-generic-business-names)): generic/ambiguous names → algorithm defaults to dominant intent.  
**Кейсы «не помешало»:** компании с ticker-like abbreviations живут, если **primary brand word другой** (напр. «Invesco» + QQQ как product code). У вас QQQ = *весь* бренд — худший layout.

### 4.4. Текущее имя «Palo Alto AI Research Lab»

| Минус | Плюс |
|-------|------|
| Generic words → конкуренция с тысячами «AI lab» | «Palo Alto» = сигнал tech geography для рекрутера |
| Не brandable exact-match | Уже есть `наш сайт`, история 9 дней, crawlers уже ходят |
| Слабый exact brand search | Не конфликтует с $400B ETF |

**Оценка:** имя **плохое для SEO-бренда**, **приемлемое для trust** у hiring. Хуже QQQ по ambiguity? **Нет — QQQ хуже.**

### 4.5. Что важнее для **вашей** цели (заметность у frontier hiring)

**Свидетельства (не мнения):**
- Perplexity/ChatGPT citation studies → research, GitHub, Wikipedia, community.  
- Ваш access-log: краулеры есть, **индекс 0** → проблема не «длинное имя», а **нет indexable/attributable surface** (static thin page, no backlinks from trusted graphs).  
- Паттерн OSS-найма: рекрутеры ищут **repos, commits, issues, papers** — org name вторичен.

**Вывод блока 3:** короткий уникальный **не-коллизионный** неологизм помогает *после* артефактов. **QQQ** — коллизионный, поэтому **мешает**. Для acqui-hire: **артефакты ≫ имя**.

---

## 5. Блок 4 — Китайская практика коротких доменов

### 5.1. Примеры с суммами (публично известные)

| Домен | Цена | Когда | Кто |
|-------|------|-------|-----|
| **JD.com** | ~**$5M** (оценка по SEC: domain asset jump ~33.4M RMB) | **2012** | JD.com (было 360buy.com) — [Bill Hartzer / Kirikos](https://www.billhartzer.com/domain-names/jd-com-domain-name-sold-for-5-million-in-2012/) |
| **Mi.com** | **$3.6M** (Weibo co-founder) | **2014** | Xiaomi — [TNW](https://thenextweb.com/news/xiaomi-simplifies-web-domain-mi-com-appeal-international-audience), [CNET](https://www.cnet.com/tech/mobile/xiaomi-spent-3-6m-on-new-two-letter-domain/) |
| **360.com** | **$17M** | **2015** | Qihoo 360 — [Wikipedia list](https://en.wikipedia.org/wiki/List_of_most_expensive_domain_names) |
| 43+ LL.com в использовании китайскими компаниями | — | as of 2021 | [Domain Name Wire](https://domainnamewire.com/2021/08/24/these-43-two-letter-com-domains-are-used-by-chinese-companies/) |

**z.ai:** ваш факт — Zhipu/GLM product surface; **публичной цены покупки z.ai в открытых сделках NameBio/DNJ в этом прогоне не найдено** («данных нет» по сумме).

### 5.2. Причинность: короткий домен **сделал** бренд?

**Оценка (высокая уверенность):** **нет — следствие успеха.**  
- JD, Xiaomi, Qihoo покупали ultra-short **уже будучи** крупными (миллиардные valuations / IPO path).  
- 2015 Chinese short-domain **investment craze** ([Quartz](https://qz.com/581248/chinas-latest-investment-craze-is-short-domain-names)) — спекуляция + status, не «startup growth hack».  
- Логика: бренд уже известен в pinyin/abbreviation → короткий .com **убирает friction** для сотен миллионов пользователей, не создаёт first awareness.

### 5.3. Культурная непереносимость

**Факт/паттерн:** китайский веб любит 2–3 символа, цифры (168, 520), pinyin initials (JD = Jing Dong).  
**Оценка:** англоязычный AI talent market + US/EU hiring **не** резолвит «QQQ» в «лаборатория» — резолвит в ETF. Практика **не переносится 1:1**.

### 5.4. Запад: короткий домен = измеримый эффект?

- **Положительные:** category-killers (AI.com $70M) — brand *is* the category; не аналог лаборатории.  
- **Негативные rebrand:** classic cases (например, «company renames to something unsearchable») — **специфичных «short rename killed us» с цифрами для AI labs** в открытых источниках мало.  
- **Западные AI labs:** Anthropic.com, openai.com, mistral.ai, cohere.com — **произносимые слова**, не LLL vanity.

---

## 6. Блок 5 — Цена смены имени при «почти нулевой» истории

### 6.1. SEO / 301

| Ситуация | Что известно |
|----------|--------------|
| Сильный сайт, правильные 301 | Volatility **4–8 недель**, стабилизация **2–4 мес.** ([isHosting 2026](https://blog.ishosting.com/en/domain-migration-seo)); иногда 6–18 мес. для complex |
| Плохие redirects / deindex weeks | Signals decay — recovery hard (GSQI / Google guidance) |
| **Ваш случай: 0 pages in index, 9 days** | **Терять почти нечего** по SEO equity |

### 6.2. LLM / training lag

**Установленное:** web-RAG (Perplexity, browsing ChatGPT) обновляется **быстрее** training corpus.  
**Оценка:** training-cutoff models «помнят» старый brand месяцы–годы; у вас **нечего помнить**.  
**Данных нет** по точному lag «rename → LLM entity update» в днях.

### 6.3. «Менять сейчас дёшево» — верно или наивно?

| Утверждение | Вердикт |
|-------------|---------|
| SEO equity ≈ 0 → migrate cheap **технически** | **Верно** |
| Значит, можно сжечь $150k на qqq.ai | **Наивно** — цена не в SEO-loss, а в **капитале + legal + ambiguity** |
| Лучший момент для **неологизма** за $100–200/yr | **Верно** — window открыт |
| Лучший момент для QQQ | **Нет** — window «дёшево» ≠ «умный buy» |

---

## 7. Что убьёт затею (3–5 сценариев)

1. **Nasdaq C&D / UDRP** на `qqq.ai` после того, как вы вложили бренд в GitHub/posts — rebrand mid-flight.  
2. **Вечная путаница в поиске/LLM:** «QQQ» → ETF; hiring-pass «не нашли лабораторию, нашли фонд».  
3. **$154k ask + $0 hiring signal** — capital burn без артефактов (ваш bottleneck).  
4. **Отказ USPTO** (или office action hell) на собственный TM «QQQ» в 9/42 → нельзя защитить имя.  
5. **Opportunity cost:** 3 месяца на branding/domain drama вместо 3 shippable OSS anchors в vendor cookbooks.

---

## 8. Дешёвая проверка гипотезы (1 неделя, малые деньги)

**Цель:** отделить «нужно ли короткое имя» от «нужны ли артефакты».

| День | Действие | Бюджет |
|------|----------|--------|
| 1 | Зафиксировать baseline: 5 запросов в Perplexity/ChatGPT/Claude/Gemini про вашу lab + 5 про «independent AI research lab OSS» — скриншоты | $0 |
| 1–2 | Зарегистрировать **тестовый** неологизм `.ai` или `.dev` (~$80–160 / 2 yr) — **не** QQQ | <$200 |
| 2–5 | Опубликовать **1 атрибутируемый артефакт** (repo + README + license + 1 issue fix в чужом cookbook) с org-name = новый бренд | $0 |
| 5–7 | Повтор тех же 5+5 запросов; замер: cite/mention? | $0 |
| 7 | A/B: тот же артефакт под старым org `palo-alto-ai` vs новым — где cite? | $0 |

**Критерий успеха гипотезы «имя решает»:** новый бренд без артефакта появляется в ответах.  
**Ожидание (оценка):** **не появится**. Артефакт появится **вне зависимости** от 3 vs 12 букв.  
**Не делать:** не покупать `qqq.ai` «для эксперимента» — $154k ≠ cheap test.

Параллельно (бесплатно): `robots.txt` + sitemap + 1 indexable page с JSON-LD Organization + GitHub org link — чинит «краулят, не индексируют» **без** смены имени.

---

## 9. Сводка: факт / оценка / спекуляция

| Утверждение | Класс |
|-------------|-------|
| QQQ reg 2898662, Nasdaq, class 036 only, live 2025 renewal | **Факт** (TSDR) |
| qqq.ai for sale $154 498 | **Факт** (DomainEasy 2026-08-05) |
| .ai under UDRP | **Факт** (WIPO list) |
| 0 index + crawler traffic = thin/no-authority surface problem | **Оценка** (ваша + SEO pattern) |
| QQQ worse for discovery than current name | **Оценка** high confidence |
| Nasdaq *will* sue you next month | **Спекуляция** — нет доказательств pipeline |
| Короткий домен *создаёт* frontier visibility | **Опровергается** кейсами + вашим Perplexity-замером |

---

## 10. Итоговая позиция (кофаундер)

**Не покупать QQQ. Не строить бренд на тикере.**  
Сейчас: (а) оставить Palo Alto AI Research Lab **или** (в) за $100–300 взять **чистый неологизм** + домен. 100% энергии — в **артефакты, которые AI-движки уже цитируют** (GitHub, cookbooks, papers). Имя — витринная рама; QQQ — рама от чужого музея.

**Уверенность рекомендации «против QQQ»:** **high**.  
**Уверенность «неологизм > держать Palo Alto навсегда»:** **med** (Palo Alto ок как interim 6–12 мес.).

---

## 11. Выполненные поиски и открытые URL

### Поисковые запросы (выборка)
- Invesco QQQ trademark USPTO registration  
- QQQ trademark class 9 42 software USPTO  
- QQQ domain UDRP WIPO Invesco  
- qqq.ai domain whois for sale  
- Nasdaq QQQ trademark registration 2898662 classes  
- three letter .com domains all taken NameBio  
- .ai domain registration renewal price 2025 2026  
- 3 letter .ai domain sales NameBio 2024 2025 2026  
- JD.com domain purchase $5 million mi.com Xiaomi  
- website migration 301 ranking recovery  
- LLM citation factors ChatGPT Perplexity  
- brand name same as stock ticker SEO  
- Tencent QQ trademark USPTO  
- UDRP complainant success rate  

### Реально открытые / проверенные URL
1. https://tsdr.uspto.gov/statusview/sn76462475 — **QQQ TM full record**  
2. https://www.invesco.com/us/en/financial-products/etfs/invesco-qqq-trust-series-1.html — mark disclosure  
3. whois.nic.ai → **qqq.ai** registration data  
4. whois.godaddy.com → **qqq.com**  
5. https://www.domaineasy.com/buy-domain/qqq.ai — **$154,498**  
6. https://www.wipo.int/en/web/amc/domain-name-disputes/index — UDRP + .AI  
7. https://www.namecheap.com/domains/registration/cctld/ai/ — .ai pricing  
8. https://domainnamewire.com/2026/02/02/ai-domain-name-prices-going-up-20/ — wholesale hike  
9. https://www.dnjournal.com/ytd-sales-charts.htm — .ai sales 2026  
10. https://www.embrace.com/recent-3-letter-com-domain-name-sales/ — LLL.com  
11. https://www.billhartzer.com/domain-names/jd-com-domain-name-sold-for-5-million-in-2012/ — JD.com  
12. https://thenextweb.com/news/xiaomi-simplifies-web-domain-mi-com-appeal-international-audience — Mi.com $3.6M  
13. https://en.wikipedia.org/wiki/List_of_most_expensive_domain_names — 360.com, AI.com  
14. https://discoveredlabs.com/blog/ai-citation-patterns-how-chatgpt-claude-and-perplexity-choose-sources  
15. https://surferseo.com/blog/llm-citations/  
16. https://bolddomains.com/blog/3-letter-ai-domains-for-sale  
17. https://abion.com/wipo-updates-domain-dispute-services/ — UDRP fees  
18. https://www.cooleygo.com/protecting-your-brand-online-with-urs-and-udrp/  
19. https://domainnamewire.com/2021/08/24/these-43-two-letter-com-domains-are-used-by-chinese-companies/  
20. https://www.atom.com/blog/are-any-3-letter-domains-available/  

---

### 🧒 Простыми словами

Представьте: вы хотите назваться **«Сбер»**, потому что коротко и запоминается — но «Сбер» уже банк на всю страну.  
**QQQ** — это как «Сбер» в мире биржи: все думают про деньги, а не про вашу лабораторию.  
Домен `qqq.ai` сейчас продают примерно за **полтораста тысяч долларов** — как купить номерной знак «001» на машину, которая ещё без мотора.  
Что реально зовёт Anthropic/OpenAI смотреть на вас: **крутые штуки на GitHub**, а не три буквы.  
**Рецепт:** не покупать QQQ; либо жить пока с Palo Alto, либо придумать **своё слово** за копейки и вложить силы в код и статьи.
