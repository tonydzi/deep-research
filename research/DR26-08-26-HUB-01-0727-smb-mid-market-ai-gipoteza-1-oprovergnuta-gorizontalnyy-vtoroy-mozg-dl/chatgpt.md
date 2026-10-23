# DR26-08-26-HUB-01-0727 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Одностраничный вердикт

**Гипотеза «меньше 1% компаний автоматизированы AI» — опровергнута**, если под автоматизацией понимать хотя бы одно рабочее применение AI, использование AI в бизнес-функции или оплату специализированного AI-сервиса:

- В США AI хотя бы в одной бизнес-функции использовали примерно **20% компаний с 1–19 сотрудниками, 21,6% с 20–49, 25,0% с 50–99, 31,4% со 100–249 и 38,5% с 250+ сотрудниками** на 14 июня 2026 года. Это репрезентативный Census BTOS, а не технологический опрос добровольцев. [Federal Reserve/Census, 17 июля 2026](https://www.federalreserve.gov/econres/notes/feds-notes/the-ai-buildout-and-the-economy-publicly-available-data-to-assess-ais-impact-accessible-20260717.htm)
- В ЕС в 2025 году AI применяли **17,0% малых компаний с 10–49 сотрудниками, 30,4% средних с 50–249 и 55,0% крупных с 250+**. Micro-компании с менее чем 10 сотрудниками из обязательной статистики исключены. [Eurostat, 11 декабря 2025](https://ec.europa.eu/eurostat/web/products-eurostat-news/w/ddn-20251211-2)
- По банковским транзакциям **17,7%** американских малых бизнесов когда-либо платили за AI-сервис к концу 2025 года; среди работодателей — **26,1%**, среди компаний без сотрудников — **15,3%**. Медиана расходов плательщика была всего **$28 в месяц**. [JPMorgan Chase Institute, 14 апреля 2026](https://www.jpmorganchase.com/institute/all-topics/business-growth-and-entrepreneurship/understanding-ai-use-by-small-businesses)
- Даже самоотчётное использование agentic AI среди владельцев малого бизнеса уже оценивалось в **9% в апреле 2025 года**. Это слабее Census по репрезентативности и опирается на определение поставщика, но всё равно далеко от 1%. [Intuit QuickBooks, обновлено 6 июля 2026](https://quickbooks.intuit.com/r/running-a-business/agentic-ai/)

**Где формулировка «<1%» всё ещё может оказаться верной:** если определить автоматизированную компанию как организацию, где автономные агенты имеют права на запись в нескольких критических системах, выполняют сквозные процессы без постоянного участия человека, координируются между функциями и создают доказанный P&L-эффект. Репрезентативной статистики по этому определению нет. Поэтому утверждать «<1%» нельзя: это правдоподобная, но пока непроверенная гипотеза.

**SMB — не пустой рынок.** Microsoft, Google, HubSpot, Notion, Zapier, Intercom и вертикальные SaaS уже обладают приложениями, данными, каналами и установленной базой. Microsoft сообщает о **более чем 20 млн платных мест Microsoft 365 Copilot**, а Google Workspace — о **более чем 11 млн компаний-пользователей**. [Microsoft, 29 апреля 2026](https://www.microsoft.com/en-us/investor/events/fy-2026/earnings-fy-2026-q3), [Google, 29 июля 2025](https://workspace.google.com/blog/product-announcements/workspace-named-leader-forrester-wave-ucaas-2025-report) Незакрытым остаётся не доступ к модели или чату, а внедрение: соединение локального стека, память о контексте компании, права, надёжность, человеческие подтверждения и ответственность за конкретный результат.

**Glean и Dust не закрыли enterprise.** Glean сообщает о **$300 млн ARR**, но не публикует общее число клиентов, минимальный контракт или минимальное число мест; его публичное позиционирование остаётся enterprise. [Glean, 28 мая 2026](https://www.glean.com/press/glean-surpasses-300m-arr-unrivaled-enterprise-context-fuels-ai-adoption) Dust уже пошёл вниз: self-service тариф рассчитан на компании до **100 сотрудников** и стоит от **$24 за пользователя в месяц** при годовой оплате. Dust сообщает о **3 000+ организациях**, но это смесь размеров, а не 3 000 крупных предприятий. [Dust pricing, август 2026](https://dust.tt/home/pricing), [Dust, 18 мая 2026](https://www.globenewswire.com/news-release/2026/05/18/3296726/0/en/Dust-raises-40M-to-make-AI-multiplayer-inside-the-enterprise.html)

**Венчурный тезис SMB работает не как “AI для всех SMB”, а как vertical AI или outcome software.** В 2025–2026 годах крупные раунды получили Owner.com для ресторанов, Siro для полевых продаж, Affiniti для финансов малого бизнеса, Sintra для предпринимателей и Pie для Main Street. Инвесторы аргументируют масштаб через замещение labour/service-бюджета, outcome pricing, вертикальные данные и повторяемую дистрибуцию, а не через ещё одну подписку на помощника. [Bessemer, январь 2026](https://www.bvp.com/atlas/building-vertical-ai-an-early-stage-playbook-for-founders), [a16z](https://a16z.com/vsaas-vertical-saas-ai-opens-new-markets/)

**Рекомендация: путь (c), consulting-first hybrid, но в жёстко продуктированной форме.** На первые 90 дней не продавать «второй мозг + флот агентов» как горизонтальную платформу. Продать два проверяемых результата:

1. Enterprise: фиксированный аудит безопасности AI-агентов/MCP с последующим мониторингом.
2. SMB: один вертикальный workflow — например, диспетчер исключений для владельца сети прачечных: поломки, жалобы, смены, платежные аномалии, отзывы и ежедневный операционный digest.

Консалтинг здесь нужен как платный discovery и источник реальных интеграций, но не как конечная идентичность компании. Победитель определяется фактическими депозитами, сроком сделки, повторяемостью внедрения, поддержкой, удержанием и доказанным ROI.

---

# 1. Реальная пенетрация

## США: репрезентативная статистика

| Размер компании | Используют AI хотя бы в одной бизнес-функции | Что именно измерено |
|---|---:|---|
| 1–4 сотрудника | 19,8% | Четырёхпериодное среднее Census BTOS на 14 июня 2026 года. [Federal Reserve/Census](https://www.federalreserve.gov/econres/notes/feds-notes/the-ai-buildout-and-the-economy-publicly-available-data-to-assess-ais-impact-accessible-20260717.htm) |
| 5–9 | 19,5% | То же определение. [Federal Reserve/Census](https://www.federalreserve.gov/econres/notes/feds-notes/the-ai-buildout-and-the-economy-publicly-available-data-to-assess-ais-impact-accessible-20260717.htm) |
| 10–19 | 20,0% | То же определение. [Federal Reserve/Census](https://www.federalreserve.gov/econres/notes/feds-notes/the-ai-buildout-and-the-economy-publicly-available-data-to-assess-ais-impact-accessible-20260717.htm) |
| 20–49 | 21,6% | То же определение. [Federal Reserve/Census](https://www.federalreserve.gov/econres/notes/feds-notes/the-ai-buildout-and-the-economy-publicly-available-data-to-assess-ais-impact-accessible-20260717.htm) |
| 50–99 | 25,0% | То же определение. [Federal Reserve/Census](https://www.federalreserve.gov/econres/notes/feds-notes/the-ai-buildout-and-the-economy-publicly-available-data-to-assess-ais-impact-accessible-20260717.htm) |
| 100–249 | 31,4% | То же определение. [Federal Reserve/Census](https://www.federalreserve.gov/econres/notes/feds-notes/the-ai-buildout-and-the-economy-publicly-available-data-to-assess-ais-impact-accessible-20260717.htm) |
| 250+ | 38,5% | Census не разделяет публично 250–499 и 500+. [Federal Reserve/Census](https://www.federalreserve.gov/econres/notes/feds-notes/the-ai-buildout-and-the-economy-publicly-available-data-to-assess-ais-impact-accessible-20260717.htm) |

Census изменил формулировку вопроса в ноябре 2025 года: вместо AI «в производстве товаров или услуг» теперь спрашивается об AI «в любой бизнес-функции». Тестирование показало, что старая формулировка пропускала AI в найме, бухгалтерии, управлении проектами и R&D. [Census, описание изменения вопроса](https://www.census.gov/hfp/btos/downloads/AI%20Question%20Wording%20Updates.pdf)

Именно поэтому показатель вырос примерно с **10% в мае 2025 года** до диапазона **17–20% после декабря 2025 года**: часть роста реальна, а часть объясняется более широким определением. [Census, июль 2025](https://www.census.gov/about/history/stories/monthly/2025/july-2025.html), [Census, 26 мая 2026](https://www.census.gov/library/stories/2026/05/ai-use-businesses.html)

BTOS охватывает выборочные когорты из базы примерно **1,2 млн предприятий**, но даже этот источник не отделяет эксперимент сотрудника от производственного workflow с измеримым эффектом. [Federal Reserve, 3 апреля 2026](https://www.federalreserve.gov/econres/notes/feds-notes/monitoring-ai-adoption-in-the-u-s-economy-20260403.html)

## ЕС

| Размер | Доля предприятий, использовавших хотя бы одну AI-технологию в 2025 году |
|---|---:|
| Micro, менее 10 сотрудников | Данных нет: участие micro-компаний в ICT-опросе необязательно, и они не входят в основной показатель. [Метаданные Eurostat](https://ec.europa.eu/eurostat/cache/metadata/en/isoc_e_esms.htm) |
| Small, 10–49 | 17,0% |
| Medium, 50–249 | 30,36% |
| Large, 250+ | 55,03% |

Источник всех трёх опубликованных категорий: [Eurostat, 11 декабря 2025](https://ec.europa.eu/eurostat/web/products-eurostat-news/w/ddn-20251211-2). Общий показатель для компаний с 10+ сотрудниками вырос с **13,5% в 2024 году до 20,0% в 2025 году**. [Eurostat](https://ec.europa.eu/eurostat/web/products-eurostat-news/w/ddn-20251211-2)

Eurostat засчитывает применение хотя бы одной AI-технологии, включая анализ текста, распознавание речи, машинное обучение, роботизацию процессов или поддержку принятия решений. Поэтому эти 20% нельзя интерпретировать как долю компаний со сквозной автономной автоматизацией. [Методология Eurostat](https://ec.europa.eu/eurostat/cache/metadata/en/isoc_e_esms.htm)

## Платное использование и глубина

| Источник | Результат | Ограничение |
|---|---|---|
| JPMorgan Chase Institute, апрель 2026 | 17,7% малых компаний когда-либо платили AI-поставщику к концу 2025 года; работодатели — 26,1%, non-employers — 15,3%. [Исследование](https://www.jpmorganchase.com/institute/all-topics/business-growth-and-entrepreneurship/understanding-ai-use-by-small-businesses) | Транзакции **4,6 млн** клиентов Chase; не видит бесплатный, встроенный или самостоятельно разработанный AI. |
| JPMorgan Chase Institute | Среди плательщиков медиана расходов — $28/мес.; 25-й перцентиль — $20, 75-й — $90; только 30,8% плательщиков generative-AI платили каждый месяц 2025 года. [Исследование](https://www.jpmorganchase.com/institute/all-topics/business-growth-and-entrepreneurship/understanding-ai-use-by-small-businesses) | Расход не доказывает успешное внедрение. |
| Intuit, май 2026 | В опросах 77% американских бизнесов сообщали о регулярном AI, но транзакции показывали лишь 12%, плативших отдельным AI-поставщикам в 2021–2025 годах. [Intuit AI Impact Report](https://www.intuit.com/blog/global-stories/ai-impact-report/) | Яркая иллюстрация разницы между self-report и наблюдаемым платежом. |
| QuickBooks, апрель 2025 | 68% компаний до 100 сотрудников заявили о регулярном AI; 28% — о ежедневном. [QuickBooks survey](https://quickbooks.intuit.com/r/small-business-data/april-2025-survey/) | Онлайн-опрос более чем 2 200 компаний, а не репрезентативный Census. |
| Salesforce, август–сентябрь 2024 | 75% из 3 350 SMB-лидеров в 26 странах экспериментировали с AI или использовали его. [Salesforce, 8 января 2025](https://www.salesforce.com/news/stories/smbs-ai-trends-2025) | Vendor-sponsored survey компаний до 200 сотрудников. |

## Agentic и автономные сценарии

Репрезентативной разбивки agentic adoption по micro/small/medium/enterprise не найдено.

Лучшие доступные ориентиры:

- QuickBooks сообщает, что **9% владельцев малого бизнеса** уже использовали agentic AI в апреле 2025 года. Это self-report и широкое определение поставщика. [QuickBooks](https://quickbooks.intuit.com/r/running-a-business/agentic-ai/)
- В глобальном опросе McKinsey 2026 года около **20% организаций-респондентов** масштабировали AI-агентов; среди компаний с выручкой более $1 млрд — **40%**, среди меньших организаций — **22%**. Однако выборка из **1 719 респондентов в 97 странах** сильно смещена к крупным и AI-активным компаниям и не является статистикой населения компаний. [McKinsey, 25 августа 2026](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai)
- В том же исследовании только **6% респондентов** соответствовали определению AI high performer: значимый эффект и не менее 5% EBIT, относимого к AI. Это снова доля внутри выбранной аудитории, а не всех компаний. [McKinsey](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai)

**Итог по гипотезе:**

| Определение «автоматизирована AI» | Вердикт по «<1%» |
|---|---|
| Любое применение AI в бизнес-функции | Опровергнуто: официальные показатели — примерно 17–55% в зависимости от размера и региона. |
| Компания платит специализированному AI-поставщику | Опровергнуто: 17,7% в транзакционных данных JPMorgan. |
| Владелец сообщает об agentic AI | Опровергнуто в доступном vendor survey: 9%. |
| Масштабированные агенты | Вероятно выше 1% среди технологически активных компаний; репрезентативных данных нет. |
| Полностью автономная cross-functional multi-agent система с доказанным P&L | Данных нет; «<1%» возможно, но не доказано. |

---

# 2. Кто уже продаёт SMB и mid-market

## Горизонтальные игроки

| Игрок | SMB/mid-market предложение и цена | Минимум/ICP | Traction |
|---|---|---|---|
| Microsoft 365 Copilot | Copilot Business — **$21/польз./мес.** при годовой оплате; временная цена первого года **$18** действует с 1 июля по 31 декабря 2026 года. Требуется совместимый Microsoft 365. [Microsoft pricing](https://www.microsoft.com/en-us/microsoft-365-copilot/pricing) | Business-планы рассчитаны максимум на **300 пользователей**; публичного минимального контракта сверх лицензирования нет. [Microsoft pricing](https://www.microsoft.com/en-us/microsoft-365-copilot/pricing) | Более **20 млн платных мест**; почти 90% Fortune 500 имеют активных low/no-code агентов. [Microsoft, 29 апреля 2026](https://www.microsoft.com/en-us/investor/events/fy-2026/earnings-fy-2026-q3) |
| Google Workspace + Gemini | Business Starter **$7**, Standard **$14**, Plus **$22/польз./мес.** при годовой оплате; AI встроен в планы. [Google pricing](https://workspace.google.com/pricing?hl=en) | Business-планы — до **300 пользователей**. [Google pricing](https://workspace.google.com/pricing?hl=en) | До включения Gemini в тарифы более **100 тыс. клиентов** уже покупали generative-AI; Workspace сообщает о более **11 млн компаний**. [Google, 15 января 2025](https://workspace.google.com/blog/product-announcements/empowering-businesses-with-AI), [Google, 29 июля 2025](https://workspace.google.com/blog/product-announcements/workspace-named-leader-forrester-wave-ucaas-2025-report) |
| Notion AI/Agent | Business — **$20/участник/мес.**; custom agents после пробного периода расходуют кредиты по **$10 за 1 000 кредитов**. [Notion pricing](https://www.notion.com/pricing) | Self-service, публичного seat minimum нет. | Более **100 млн пользователей** Notion в целом; отдельная платная AI-выручка не раскрыта. [Notion](https://www.notion.com/en-US) |
| Zapier Agents | Free — **400 activities/мес.**; Pro — **$33,33/мес.** при годовой оплате за 1 500 activities. [Zapier pricing](https://zapier.com/pricing) | Self-service от одного пользователя до Enterprise. | Zapier сообщает о **200 млн AI-задач**, темпе 23 млн в месяц и экосистеме почти 9 000 приложений. [Zapier, 29 апреля 2025](https://zapier.com/blog/zapier-ai-orchestration-platform/) |
| HubSpot Breeze/agents | Starter — текущая спеццена **$7/место/мес.**, обычная $20; Professional — **$1 300/мес.**, Enterprise — **$4 700/мес.**; дополнительные AI-кредиты — $9 за 1 000. [HubSpot pricing](https://www.hubspot.com/pricing/suite) | Официальный ICP — B2B-компании с **2–2 000 сотрудниками**. [HubSpot 2025 10-K](https://ir.hubspot.com/static-files/efb8d22a-4fcd-4c15-b154-7cf59069c05c) | **288 706 клиентов** в 135+ странах на 31 декабря 2025 года; adoption отдельных AI-агентов не раскрыт. [HubSpot 10-K](https://ir.hubspot.com/static-files/efb8d22a-4fcd-4c15-b154-7cf59069c05c) |
| Intercom Fin | Базовый Intercom Essential — **$29/место/мес.**; Fin — **$0,99 за закрытый outcome**, для внешнего helpdesk возможен минимум 50 outcomes. [Intercom pricing](https://www.intercom.com/pricing) | Подходит небольшой support-команде; нет обязательной большой инсталляции. | Почти **8 000 клиентов Fin**, около 2 млн обращений в неделю; Intercom заявляет средний resolution rate 76%. [Intercom, 25 марта 2026](https://www.intercom.com/blog/announcing-monitors-opening-the-ai-black-box/) |

Вывод: горизонтальная цена для SMB уже зажата между включённым AI и примерно **$7–33 за пользователя в месяц**, тогда как медиана фактических расходов малого AI-плательщика составляет **$28 в месяц на компанию**. [JPMorgan Chase Institute](https://www.jpmorganchase.com/institute/all-topics/business-growth-and-entrepreneurship/understanding-ai-use-by-small-businesses) Это делает продажу ещё одного общего помощника тяжёлой; outcome-продукт с экономическим владельцем бюджета может брать деньги из операционных или трудовых расходов.

## Glean и Dust

| Вопрос | Glean | Dust |
|---|---|---|
| Публичный ICP | Продолжает позиционироваться как enterprise work AI platform; релиз $300 млн ARR подчёркивает Fortune 500 и крупные deployment. [Glean](https://www.glean.com/press/glean-surpasses-300m-arr-unrivaled-enterprise-context-fuels-ai-adoption) | Enterprise-платформа плюс явный self-service Business для организаций до 100 сотрудников. [Dust pricing](https://dust.tt/home/pricing) |
| Публичная цена | Не раскрыта; sales-led demo. [Glean](https://www.glean.com/) | Pro — **$24/польз./мес.** ежегодно; Max — **$120/польз./мес.**; Enterprise — договорная. [Dust pricing](https://dust.tt/home/pricing) |
| Минимальные seats/ACV | Не раскрыты. | Публичного обязательного seat minimum на Business-странице нет. [Dust pricing](https://dust.tt/home/pricing) |
| Traction | **$300 млн ARR** на 28 мая 2026 года; общее число клиентов не раскрыто. [Glean](https://www.glean.com/press/glean-surpasses-300m-arr-unrivaled-enterprise-context-fuels-ai-adoption) | **3 000+ организаций**, 51 тыс. MAU и 300 тыс. созданных агентов; показатели заявлены компанией. [Dust, 18 мая 2026](https://www.globenewswire.com/news-release/2026/05/18/3296726/0/en/Dust-raises-40M-to-make-AI-multiplayer-inside-the-enterprise.html) |
| Down-market | Публичного self-service SMB-плана или подтверждённого down-market плана не найдено. | Down-market уже состоялся: отдельный тариф до 100 сотрудников. |

Утверждение, что эти две компании «закрыли весь enterprise», не подтверждается. Только в ЕС было около **55 тыс. предприятий с 250+ сотрудниками** в 2024 году, а в США — **22 704 фирмы с 500+ сотрудниками** по доступной оценке SBA на базе данных 2021 года. Пороги различаются, и это лишь США+ЕС, но уже получается более **77 тыс. крупных компаний**. [Eurostat, 9 декабря 2025](https://ec.europa.eu/eurostat/web/products-eurostat-news/w/ddn-20251209-2), [U.S. SBA, ноябрь 2024](https://advocacy.sba.gov/wp-content/uploads/2024/11/Research-Spotlight-How-Small-Business-Jobs-Transition-508.pdf)

Точную долю Glean+Dust рассчитать нельзя: Glean не раскрывает число клиентов, Dust не разбивает 3 000 организаций по размеру, а пересечение клиентских баз неизвестно.

---

# 3. Венчурность SMB-фокуса

## Профинансированные примеры 2025–2026

| Стартап | Раунд | Дата и лид | Питч |
|---|---:|---|---|
| Owner.com | **$120 млн Series C**, valuation $1 млрд | 13 мая 2025; Meritech Capital и Headline. [Restaurant Business](https://www.restaurantbusinessonline.com/technology/tech-supplier-ownercom-raises-120m-giving-it-1b-valuation) | AI и цифровой front office для независимых ресторанов: сайт, заказы, маркетинг, guest engagement. |
| Siro | **$50 млн Series B** | 22 мая 2025; SignalFire. [Siro/PR Newswire](https://www.prnewswire.com/news-releases/siro-raises-50m-series-b-to-bring-ai-to-in-person-sales-302463711.html) | AI-коучинг и анализ личных продаж для home improvement, home services, medspa и других полевых вертикалей. |
| Affiniti | **$17 млн Series A** | 20 мая 2025; SignalFire. [SignalFire](https://www.signalfire.com/blog/affiniti-investor) | Финансовая operating system для малого бизнеса: расходы, кредит, управление cash flow. |
| Sintra | **$17 млн seed** | 1 июля 2025; Earlybird, с Inovo и Practica. [Global Private Capital Association](https://www.globalprivatecapital.org/newsroom/earlybird-vc-leads-usd17m-seed-round-for-lithuanias-sintra/) | Набор AI-помощников для предпринимателей и небольших команд. Компания сообщает о 40 тыс.+ пользователей в 100+ странах. [Sintra](https://www.sintra.com/) |
| Layer | **$6,6 млн seed** | 9 июля 2025; Emergence Capital. [Layer/GlobeNewswire](https://www.globenewswire.com/news-release/2025/07/09/3112479/0/en/Layer-Raises-6-6M-Led-by-Emergence-Capital-to-Bring-Embedded-Accounting-to-the-SMB-Software-Stack.html) | Embedded AI-accounting внутри вертикальных SMB-платформ — дистрибуция через уже установленный SaaS. |
| Rillet | **$70 млн Series B** | 6 августа 2025; a16z и ICONIQ. [Rillet](https://www.rillet.com/blog/rillet-raises-70m-series-b-from-andreessen-horowitz-and-iconiq) | AI-native ERP/бухгалтерия для быстрорастущих компаний; более 200 клиентов на момент раунда. |
| Pie | **$19,5 млн Series A**, $23,7 млн всего | 30 июня 2026; Lightspeed. [Pie](https://getpie.com/blog/pie-raises-23.7m-to-bring-ai-powered-growth-to-main-street-businesses) | AI growth platform для Main Street: локальная обнаруживаемость, лиды и конверсия. |

Это показывает, что VC не отвергает SMB. Но финансируются преимущественно:

- вертикальные продукты с понятным workflow;
- системы, контролирующие revenue или cost outcome;
- embedded-продукты с чужой дистрибуцией;
- AI-native замена услуги или труда.

Bessemer рекомендует начинать с конкретного дорогостоящего workflow, а не с общей AI-технологии. [Bessemer, январь 2026](https://www.bvp.com/atlas/building-vertical-ai-an-early-stage-playbook-for-founders) a16z прямо строит vertical-AI тезис на расширении TAM из software spend в labour spend. [a16z](https://a16z.com/vsaas-vertical-saas-ai-opens-new-markets/)

## Unit economics

Надёжных публичных агрегатов именно по SMB-AI CAC и churn нет. Ближайшие ориентиры:

| Метрика | Наблюдаемые данные | Интерпретация |
|---|---|---|
| Willingness to pay | Медиана фактических AI-платежей малого бизнеса — **$28/мес.**, межквартильный диапазон $20–90. [JPMorgan](https://www.jpmorganchase.com/institute/all-topics/business-growth-and-entrepreneurship/understanding-ai-use-by-small-businesses) | Горизонтальный AI seat имеет очень низкий естественный потолок без доказанного outcome. |
| Регулярность | Только **30,8%** generative-AI плательщиков делали платежи все 12 месяцев 2025 года. [JPMorgan](https://www.jpmorganchase.com/institute/all-topics/business-growth-and-entrepreneurship/understanding-ai-use-by-small-businesses) | Это не churn cohort, но сигнал экспериментального и прерывистого спроса. |
| Low-ARPA retention | Для SaaS с ARPA ниже $25 медианное удержание клиентов составило **62% при годовой оплате против 41% при месячной**. [ChartMogul 2025 SaaS Billing Report](https://chartmogul.com/reports/saas-billing-report/) | Соответствует 38% и 59% годового logo churn; это весь SaaS, не AI-SMB. |
| Mid-ACV retention | Для частного B2B SaaS с ACV $25–50 тыс. медианный NRR — **102%**, верхний квартиль — 111%. [SaaS Capital, 2025](https://www.saas-capital.com/blog-posts/what-is-a-good-retention-rate-for-a-private-saas-company/) | Более высокий ACV и expansion радикально улучшают экономику, но это revenue retention, не logo churn. |
| CAC payback | Vendor benchmark Maxio называет «хорошим» payback около **9 месяцев для very-small-business, 7 месяцев для SMB и 14 месяцев для mid-market**. [Maxio](https://www.maxio.com/saaspedia/cac-payback) | Это эвристика, а не наблюдаемая статистика AI-стартапов; публичных репрезентативных CAC в долларах нет. |

## Почему это может быть венчуром

Венчурный вариант возникает, если выполняются одновременно четыре условия:

1. Цена привязана к результату или заменяемому труду, а не к «доступу к AI».
2. Один вертикальный workflow повторяется почти без кастомизации.
3. Дистрибуция масштабируется через установленный SaaS, франшизу, ассоциацию, MSP или marketplace.
4. Данные, интеграции и результат каждого внедрения улучшают следующий deployment.

Bessemer в своей рамке AI-native services считает такие компании инвестируемыми, если AI выполняет работу с software-like margins, выручка повторяется, а данные и workflow создают moat. [Bessemer, 31 июля 2026](https://www.bvp.com/atlas/owning-the-outcome-bessemers-ai-native-services-evaluation-framework)

## Контраргументы

- Низкий ARPA сочетается с высоким logo churn. [ChartMogul](https://chartmogul.com/reports/saas-billing-report/)
- Интеграция, обучение и поддержка могут стоить дороже подписки: SMB чаще жалеет о покупке из-за слабого внедрения и нехватки внутренних IT-ресурсов. [Capterra 2025](https://www.capterra.com/resources/tech-trends-smb-enterprise-software-purchase-tips/)
- Microsoft и Google включают AI в уже оплачиваемые пакеты, снижая perceived value отдельного горизонтального продукта. [Microsoft](https://www.microsoft.com/en-us/microsoft-365-copilot/pricing), [Google](https://workspace.google.com/blog/product-announcements/empowering-businesses-with-AI)
- У SMB данные сильнее фрагментированы, а владелец покупает устранение проблемы, не инфраструктуру агентов.

Из фондов наиболее системный публичный тезис вокруг vertical/applied AI демонстрируют Bessemer, a16z, Emergence и SignalFire; SignalFire в апреле 2025 года привлёк более **$1 млрд** с заявленным фокусом на applied AI по вертикалям. [SignalFire](https://www.signalfire.com/blog/signalfire-raises-one-billion-in-new-capital)

---

# 4. Консалтинг и AI-агентства

## Размер рынка и экономика

Надёжной оценки именно рынка «AI automation agencies для SMB» нет.

Коммерческие оценки широкого AI-consulting расходятся примерно вдвое:

- Future Market Insights оценивает мировой рынок AI consulting в **$11,07 млрд в 2025 году**. [FMI](https://www.futuremarketinsights.com/reports/ai-consulting-services-market)
- Market Data Forecast оценивает тот же широкий рынок в **$22,27 млрд в 2025 году** и $30,24 млрд в 2026-м. [Market Data Forecast](https://www.marketdataforecast.com/market-reports/ai-consulting-services-market)

Эти оценки включают enterprise strategy, data science, cloud и systems integration; использовать их как TAM SMB-агентств нельзя.

Репрезентативной статистики типичной выручки AI-автоматизационного агентства также нет. Ближайший proxy — опрос 151 digital-services agency: средняя численность составляла **26 сотрудников**, медианная — 13, средняя чистая маржа в 2024 году — **14%**; у студий — 19%, у средних агентств — 8%. Выборка преимущественно американская и не специфична для AI. [Promethean Research, 2025](https://research.prometheanresearch.com/products/surviving-thriving-through-a-tough-market-state-of-digital-services-2025)

## Agency-to-SaaS: где сработало

- Mailchimp вырос из web-design агентства: основатели построили email-инструмент как побочный продукт для клиентов малого бизнеса. Позже Intuit приобрёл Mailchimp за **$12 млрд**; компания сообщает о **13 млн пользователей**. [Mailchimp history](https://mailchimp.com/about/), [Mailchimp founders](https://mailchimp.com/about/founders/)
- Basecamp появился внутри design-консалтинга 37signals; примерно через год после запуска выручка продукта превысила design-бизнес, после чего компания прекратила клиентские услуги. [Basecamp history](https://basecamp.com/about)

Эти примеры сработали не потому, что услуги сами стали венчурными, а потому, что клиентская работа обнаружила повторяемую проблему, после чего продукт отделился от почасовой модели.

## Где модель ломается

Builder.ai привлёк более **$450 млн**, обещая AI-автоматизацию разработки приложений, но столкнулся с неплатёжеспособностью в мае 2025 года; пресса сообщала о существенной роли человеческих инженеров и пересмотре выручки вниз. [TechCrunch, 20 мая 2025](https://techcrunch.com/2025/05/20/once-worth-over-1b-microsoft-backed-builder-ai-is-running-out-of-money/) Это не обвинение всех гибридов, а предупреждение: нельзя выдавать трудоёмкий сервис за software gross margin.

## Как VC смотрит на services revenue

Публичной универсальной формулы вроде «инвестор дисконтирует services revenue на X%» нет. Практический инвестиционный тест — не наличие услуг, а:

- повторяемость scope;
- валовая маржа;
- доля ручного труда на следующем клиенте;
- recurring component;
- способность продукта поглощать услуги со временем;
- честное раздельное представление software и services revenue.

Bessemer допускает венчурность AI-native services, если рынок фрагментирован и необходим, AI выполняет большую часть работы, а экономика приближается к software. [Bessemer, июль 2026](https://www.bvp.com/atlas/owning-the-outcome-bessemers-ai-native-services-evaluation-framework)

Для вашей команды consulting-first рационален только с ограничителями: фиксированная цена, один workflow, один набор интеграций, учёт founder-hours и обязательная конверсия в recurring monitoring/outcome product.

---

# 5. Каналы дистрибуции в SMB

## Как SMB покупает технологию

По исследованию GTIA среди **720 SMB**:

- 65% покупали технологии непосредственно у поставщика;
- 45% — через online marketplace;
- 40% — через retailers;
- 28% — через channel-компании;
- 61% регулярно или иногда отдавали IT наружу.  
  Ответы были множественными, а managed services не полностью отражены в transactional channel percentages. [GTIA, 29 июля 2025](https://gtia.org/blog/whats-up-with-your-customers-5-smb-tech-buying-trends)

Главные AI-барьеры в том же исследовании: стоимость — **38%**, обучение персонала — 37%, поиск поставщика — 30%, нехватка квалификации — 28%, данные — 28%, неопределённый ROI — 24%. [GTIA 2025 report](https://gtia.org/hubfs/GTIA%202025%20SMB%20Technology%20and%20Buying%20Trends%20Research.pdf)

Социальное доказательство критично: Gartner Digital Markets сообщает, что **90% software buyers** считают social proof сильным фактором shortlist, а 83% меняют первоначальный список поставщиков в процессе покупки. [Gartner Digital Markets, 2025](https://www.gartner.com/en/digital-markets/insights/research-rundown-2025-software-journey)

Следовательно, реальные каналы SMB-AI:

1. Вертикальный SaaS, уже хранящий транзакции и workflow.
2. MSP, бухгалтер, equipment dealer или отраслевой консультант.
3. Marketplace Shopify, QuickBooks, HubSpot или Microsoft.
4. Ассоциация и отраслевое мероприятие.
5. Франчайзер или крупная сеть, распределяющая решение по точкам.
6. Реферал от другого владельца с измеримым кейсом.

Shopify App Store предлагает более **21 тыс. приложений** миллионам merchants в 175+ странах. [Shopify](https://www.shopify.com/news/about-us) QuickBooks поддерживает сотни приложений и сеть solution providers. [QuickBooks Partners](https://quickbooks.intuit.com/partners/)

## Тест: сеть прачечных из 20 точек

### Что она уже может покупать

| Слой | Продукт/цена | Стоимость для 20 точек |
|---|---|---:|
| Машины, платежи, telemetry, маркетинг | Laundroworks Starter **$89/location**, Growth $249, Ultimate $399 в месяц. У компании 70+ дилеров; оборудование устанавливается через локальный канал. [Laundroworks pricing](https://laundroworks.com/pricing) | Простое умножение: **$1 780–7 980/мес.**, без оборудования, processing fees и внедрения. |
| Вертикальная operating system | Cents: POS, self-service, wash-and-fold, pickup/delivery, employee management и multi-location reporting; цена публично не раскрыта. [Cents](https://www.trycents.com/solutions/laundry-management-software) | Данных нет. Cents заявлял о 4 000 laundromat locations и $1 млрд годовых платежей в августе 2025 года. [Cents/PR Newswire](https://www.prnewswire.com/news-releases/cents-demonstrates-next-generation-laundry-technology-solutions-at-clean-show-2025-302531712.html) |
| Scheduling/time clock | Homebase Essentials — **$24/location/мес.** ежегодно; Plus с AI scheduling — $56. [Homebase](https://wf.joinhomebase.com/pricing) | **$480–1 120/мес.** |
| Бухгалтерия | QuickBooks Online: list price от **$38 до $275/мес.** в зависимости от плана. [QuickBooks pricing](https://quickbooks.intuit.com/pricing/) | Обычно company-level, но структура нескольких entities может увеличить стоимость; точных данных без конфигурации нет. |
| Отраслевой канал | Coin Laundry Association имеет каталоги equipment/service providers; service-provider membership стоит **$1 250 в год**. [CLA](https://laundryassociation.org/membership/service-providers/) | Это канал доступа к владельцам, не операционный software fee. |

### Кто продал бы ей AI сегодня

Наиболее вероятный продавец — не горизонтальный AI-стартап, а:

- дилер оборудования или платежных систем;
- Cents/Laundroworks или их партнёр;
- локальный MSP;
- бухгалтер/QuickBooks solution provider;
- поставщик scheduling/marketing;
- отраслевой консультант или член Coin Laundry Association.

Laundroworks прямо распространяется через сеть **70+ дилеров**, а CLA каталогизирует поставщиков оборудования, платежей, маркетинга и технологий. [Laundroworks](https://laundroworks.com/pricing), [CLA](https://laundryassociation.org/for-owners/laundry-equipment-services/)

### Куда встроить «второй мозг + агенты»

Не как новый универсальный workspace, а как слой исключений над существующим стеком:

- ежедневный owner digest по 20 точкам;
- обнаружение падения выручки, ошибок платежей и аномалий машин;
- жалоба клиента → восстановление контекста → предложенный ответ/возврат с лимитами;
- поломка → поиск истории машины → обращение дилеру → контроль SLA;
- невыход сотрудника → поиск замены → уведомление менеджера;
- отрицательный отзыв → сверка с транзакцией и историей → draft ответа;
- SOP и память по локации для управляющего;
- координация Cents/Laundroworks, Homebase, QuickBooks, email, SMS и service vendors.

Это предлагаемое позиционирование, а не доказанная готовность рынка платить.

Для пилота разумно тестировать цену, например **$2 500 setup + $750/мес. за сеть**, а не за пользователя. Это экспериментальная цена, не рыночная статистика. Она должна быть привязана к 10+ сэкономленным manager-hours, сокращению downtime или восстановленной выручке, иначе её необходимо снижать или workflow закрывать.

---

# 6. Вердикт для вашей команды

## Сравнение путей

| Путь | Сильные стороны | Главные риски | Вердикт |
|---|---|---|---|
| **(a) Enterprise-клин через agent/MCP security** | Уже есть OSS-framework; конкретный buyer — CISO/AI platform owner; потенциально высокий ACV; риск агентов растёт вместе с adoption. | No-name vendor должен заслужить доверие; длинная закупка; необходимы воспроизводимые находки, страхование/юридические условия и интеграции с security stack. | Сильный кандидат на основной продукт, но сначала продавать фиксированный аудит, а не большую платформу. |
| **(b) SMB “второй мозг + флот”** | Огромное число потенциальных компаний; быстрый доступ к владельцу; техническое решение уже существует. | Горизонтальные incumbents уже встроены; медиана AI-spend $28/мес.; architecture не является job-to-be-done; высокий support burden и churn. | Не запускать как горизонтальный продукт. Допустим только вертикальный outcome wedge. |
| **(c) Consulting-first hybrid** | Самый быстрый путь к платежам, данным, интеграциям и кейсам; позволяет проверить оба ICP до большого product bet. | Ловушка кастомной разработки; founder bottleneck; низкая маржа и слабая венчурность без повторяемости. | Лучший первый путь при строгой продуктовой дисциплине. |

## Рекомендуемый 90-дневный тест

Все цены и thresholds ниже — предлагаемые falsification criteria, а не опубликованные рыночные нормы.

### Дни 1–14: два оффера

**Security cell**

- «Agent/MCP Security Review».
- Срок — 10 рабочих дней.
- Цена — $12 500.
- Deliverables: threat model, permission map, credential leakage, prompt/tool abuse, supply-chain review, reproducible exploits, remediation plan.
- Целевая аудитория: 30 CISO, AI-platform и security engineering лидов, уже развернувших агентов или MCP.

**Vertical SMB cell**

- «Multi-location Operations Agent».
- Один рынок: laundromats либо другой выбранный multi-location vertical.
- Цена пилота: $2 500 setup + $750/мес.
- Целевая аудитория: 30 владельцев с 5–50 точками.
- Один outcome: исключения и follow-up между POS/payment, scheduling, accounting, reviews и vendors.

Не делать бесплатные пилоты: бесплатный интерес не измеряет willingness to pay.

### Дни 15–45: проверка продажи

Минимальные сигналы:

- Security: не менее **3 оплаченных аудитов**, суммарно не менее $30 тыс.; медианный цикл до платежа — не более 30 дней; хотя бы один клиент просит recurring monitoring.
- SMB: не менее **5 оплаченных пилотов** минимум по $500/мес. плюс setup; минимум четыре клиента дают доступ к реальным системам и используют продукт еженедельно.
- Consulting: фиксировать часы по каждому deployment и долю повторно использованных компонентов.

### Дни 46–90: проверка экономики

**Путь (a) выигрывает, если:**

- два или более клиента переходят из аудита в recurring monitoring;
- более 60% артефактов следующего аудита генерируются повторяемым движком;
- находится повторяющаяся категория критичных уязвимостей;
- покупатель и бюджет устойчиво принадлежат security, а не innovation lab.

**Путь (b) выигрывает, только если:**

- не менее четырёх SMB-пилотов запущены;
- не менее трёх готовы продлить оплату после 60 дней;
- появляется хотя бы один органический referral;
- каждый клиент показывает не менее 3× monthly gross benefit к цене либо экономит 10+ manager-hours в месяц;
- поддержка укладывается максимум в два часа на клиента в неделю.

**Путь (c) можно сохранять как мост, только если:**

- fully loaded delivery gross margin достигает хотя бы 50%;
- 60%+ implementation-компонентов повторно используются;
- минимум две трети клиентов конвертируются в recurring продукт;
- объём founder-hours на нового клиента последовательно снижается.

## Финальный выбор

Начать с **(c), продуктированный consulting-first hybrid**, одновременно тестируя два узких клина: enterprise security и один vertical-SMB operational outcome.

Наиболее вероятный итог по имеющимся данным:

1. **Security audit → recurring monitoring** имеет лучшие шансы на высокий ACV и технологическую дифференциацию.
2. **Vertical SMB agent → outcome pricing** может стать венчурным, если найдётся повторяемый канал и продукт отнимает бюджет у труда/операционных потерь.
3. **Горизонтальный “second brain + agent fleet for every employee”** сейчас выглядит самой слабой первой упаковкой: она сталкивается одновременно с Microsoft, Google, Notion, Zapier, Dust и низкой фактической SMB willingness to pay.

Рынок открыт не потому, что менее 1% компаний касались AI. Он открыт потому, что между «пользуемся AI» и «AI надёжно выполняет дорогой бизнес-процесс» остаётся огромный разрыв. Именно этот разрыв — внедрение, права, память, контроль и доказанный outcome — и должен быть вашим продуктом.
