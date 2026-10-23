# DR26-07-31-MACANTON-04-2124 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: OpenAI Codex CLI (local headless on Mac16, ChatGPT subscription — NOT paid API), model gpt-5.6-sol
topic: Anthropic-эквивалент вендор-партнёрской модели kone.vc (Сабуров)
dr_id: DR26-07-31-MACANTON-04-2124
date: 2026-07-31

rail: codex exec --skip-git-repo-check, live web search, exit 0
sources: 24 unique URLs cited · 14 web-search calls in stderr

note: headless CLI rail — LOCAL session, no chatgpt.com/c/ URL exists for this run
---

Ниже — состояние на **31 июля 2026 года**. Важное ограничение: публичные страницы подтверждают существование программ, но коммерческие приложения к партнёрским договорам — комиссии, deal registration, MDF и выдачу промокодов конкретным партнёрам — в большинстве случаев закрыты. Поэтому я отдельно отмечаю публично подтверждённое и то, что нельзя честно установить без договора.

## Короткий вывод

Повторить модель kone.vc в Anthropic буквально — «получить пачку бесплатных Claude Team-лицензий и раздавать их как лид-магнит» — сейчас нельзя считать доступной публичной возможностью.

Лучший 90-дневный ход для Антона:

- **Один трек:** `Powered by Claude` — через самостоятельный публичный продукт, а не Services Track.
- **Один лид-магнит:** доказательный «Claude Workflow Audit» до встречи: 3 найденных процесса, оценка времени/ROI, один работающий демо-артефакт.
- **Один оффер:** четырёхнедельный Claude Adoption Pilot для компаний 10–50 человек: один процесс в production, обучение команды, evals и governance.
- **Целевая цена:** $12–25 тыс. за пилот; затем $2.5–5 тыс./мес. сопровождение.

Services Track стоит зарегистрировать как долгосрочную опцию, но делать его основной 90-дневной целью не следует: даже первый полноценный уровень требует минимум десяти действующих сертифицированных специалистов.

---

# 1. Официальные треки Anthropic

| Трек | Для кого | Публичные требования и подача | Что даёт | Пригодность Антону за 90 дней |
|---|---|---|---|---|
| **Claude Partner Network — Registered** | Консалтинг, интеграторы, агентства | Бесплатная заявка. Любая организация, выводящая Claude на рынок, может податься. Новый участник начинает как Registered с обязательством довести практику до 10 сертифицированных специалистов | Partner Hub, Partner Academy, обучение, playbooks, доступ к сертификации; возможная поддержка сделок | **Средняя:** можно получить статус участника, но это ещё не публичный tier и не сильный buyer-facing badge |
| **Services Track — Select** | Устоявшаяся Claude-практика | 10 активных сертифицированных специалистов; 2 совместных production-клиента за последние 12 месяцев; 1 публичный customer story | Публичный каталог, tier, техническая и GTM-поддержка, referral credit/deal protection, co-marketing по уровню | **Низкая для соло:** математически не пройти без расширения certified bench |
| **Services — Preferred** | Более крупные практики | 100 сертифицированных; 15 production-клиентов; 3 истории | Более высокий публичный tier и поддержка | Неприменимо |
| **Services — Global Premier** | Глобальные SI | 1 000 сертифицированных; 100 клиентов в ≥3 регионах; 15 историй; joint business plan и executive sponsors | Максимальная совместная работа | Неприменимо |
| **Powered by Claude directory** | Компании с собственным продуктом, реально использующим Claude | На странице есть форма `Build with Claude`; каталог курируемый. Детальные числовые критерии публично не опубликованы | Публичный листинг и официальный контекст «Powered by Claude» | **Высокая:** достижимо соло, если упаковать лабораторный движок как проверяемый продукт |
| **Claude Marketplace** | Enterprise-ready SaaS-продукты | Waitlist; продукт должен быть Claude-powered и соответствовать требованиям enterprise security, scale и compliance | Доступ к enterprise-покупателям; продукт может оплачиваться из существующего Anthropic commitment; Anthropic управляет invoicing | **Низкая сейчас:** нужен зрелый SaaS и procurement readiness |
| **Claude for Startups** | Стартапы, строящие на Claude | В программу может вступить ранний стартап. Для кредитов: создан ≤4 лет назад, institutional equity funding, раньше не получал Anthropic startup credits; нужны Console account, доменная почта, сайт и описание продукта | API-кредиты, максимальные rate limits на период кредитов, community, мероприятия и ранняя информация | **Высокая доступность, слабое соцдоказательство:** полезно для себестоимости, но это не знак vetted-консультанта |
| **Claude Campus / Ambassadors** | Студенты и университетские сообщества | Весенняя программа 2026 закрыта; Builder Clubs организуют workshops, hackathons и community | Stipend, API-кредиты для сообщества, контакт с Anthropic, guest speakers | **Не подходит**, если Антон не выступает как студент/кампус-лидер |
| **Resale Claude Team/Enterprise** | Предположительно отдельные channel/alliance relationships | Публичной открытой программы реселлинга Team/Enterprise с опубликованными скидками и маржой не найдено | Не установлено публично | **Не строить модель вокруг этого** |
| **Cloud channels** | AWS, Google Cloud, Microsoft и их партнёры | Через соответствующие cloud partner/channel программы | Продажа и внедрение Claude через Bedrock, Vertex AI или Microsoft Foundry; cloud incentives зависят от программы облака | Возможный второй этап, но это уже распыление |

Anthropic запустила Claude Partner Network 12 марта 2026 года, заявив инвестиции **$100 млн** в обучение, technical support и joint market development. Membership бесплатна; квалифицированные фирмы попадают в Services Partner Directory. [Официальный анонс Anthropic](https://www.anthropic.com/news/claude-partner-network)

3 июня Anthropic опубликовала точные требования Services Track. Также подтверждены:

- индивидуальная сертификация через Partner Academy;
- квартальная проверка показателей;
- referral credit и deal protection отдельно от service tier;
- promotion review 1 января и 1 июля, плюс специальная проверка 1 октября 2026 года;
- понижение только при годовом review после уведомления.  
  [Официальное описание Services Track](https://www.anthropic.com/news/services-track-partner-hub)

Нюанс: Anthropic утверждает, что критерии одинаковы для десятичеловечной студии и глобального SI. Это хорошо для прозрачности, но фактически исключает соло-консультанта из Select.

### Powered by Claude и Marketplace — разные вещи

`Powered by Claude` — курируемый showcase продуктов. Marketplace — коммерческий procurement channel для enterprise-ready vendors. Marketplace позволяет заказчику направить часть уже согласованного Anthropic spend commitment на продукт партнёра; это гораздо сильнее простого логотипа, но и порог существенно выше. [Powered by Claude](https://claude.com/partners/powered-by-claude), [Claude Marketplace](https://claude.com/platform/marketplace), [partner waitlist](https://claude.com/marketplace-partners)

---

# 2. Есть ли у Anthropic раздаваемые promo-лицензии

## Публично подтверждено

### Startup API credits

Это кредиты самому qualifying startup, а не универсальный пул, который консультант может раздать своим SMB-лидам.

Условия для дополнительных льгот и кредитов:

- institutional equity funding;
- возраст компании до четырёх лет;
- startup ранее не получал Anthropic credits;
- использование через first-party Claude Console;
- кредиты не работают в AWS Bedrock или Google Vertex AI.

VC-фонды и акселераторы могут податься как equity investors и давать портфельным компаниям доступ к startup benefits. Это наиболее близкий публичный механизм «партнёр распространяет кредиты», но он предназначен для инвесторов и их портфеля, а не для консалтингового лидгена. [Claude for Startups](https://claude.com/programs/startups)

### Enterprise activation promotion

В июне–июле 2026 действовала официальная акция:

- $1 000 usage credits за каждого впервые активировавшего Claude Code или Cowork пользователя;
- до 10 000 пользователей / $10 млн на организацию;
- только usage-based Enterprise;
- не для Team, Pro или Max;
- активация до **2 июля 2026, 23:59 PT**;
- кредит закреплялся за активировавшим seat и действовал 90 дней.

На дату отчёта окно активации уже закрыто. Это была customer promotion, а не переносимый партнёрский пул. [Условия акции](https://support.claude.com/en/articles/15282265-claude-enterprise-activation-promo-for-claude-code-and-cowork)

### Claude Campus и nonprofit initiatives

Anthropic выдаёт API/Claude credits отдельным образовательным и nonprofit-программам. Например, Claude Campus распространял ресурсы вроде API credits среди клубов, а программа с LISC обеспечивала кредиты конкретной когорте предпринимателей. Это договорные cohort programs, не открытый commercial referral mechanism. [Claude Campus](https://claude.com/programs/campus), [Claude for Small Business announcement](https://www.anthropic.com/news/claude-for-small-business)

## Что не подтверждено

Нет публичного подтверждения, что обычный Registered или Services partner получает:

- пачку бесплатных Claude Team seats;
- универсальные промокоды;
- право самостоятельно назначать получателей API credits;
- стандартную reseller discount;
- комиссию с self-service Team/Enterprise subscriptions.

Партнёрская программа предусматривает co-investment, market development и поддержку customer deployments, поэтому кредиты или funded pilots могут существовать в конкретной сделке. Но это надо запрашивать через Partner Hub и deal registration; обещать клиентам заранее их нельзя.

### Практический вывод

Лид-магнит должен быть ценен без лицензии:

> «Мы за 48 часов строим доказательный Claude-аудит вашего бизнеса и показываем один рабочий workflow на публично доступных данных».

Если Anthropic даст credits под зарегистрированный pilot — это бонус для закрытия сделки, а не суть оффера.

---

# 3. Как маленькие AI-консалтинги реально зарабатывают

Основная экономика находится не в resale seats, а в четырёх слоях:

| Слой | Что продаётся | Реалистичный диапазон для SMB / lower mid-market |
|---|---|---:|
| Readiness / workflow audit | Интервью, process map, data/security review, ROI backlog | $2–10 тыс. |
| Enablement | Настройка workspace, policies, prompt/skill library, обучение 10–30 человек | $4–15 тыс. |
| Один production workflow | Интеграция, agent/MCP, evals, observability, handoff | $10–30 тыс. |
| Ongoing adoption | Evals, usage review, новые skills/workflows, office hours | $1.5–5 тыс./мес. для SMB; $5–15 тыс./мес. для mid-market |

Это не строгий industry price index: рынок непрозрачный, а большая часть публичных цифр опубликована самими продавцами. Но несколько наблюдаемых точек дают полезные границы:

- Palavir публикует **$2 500 за audit** и **$25 000 за один Claude pipeline за десять рабочих дней**. Это seller-published price, а не независимая статистика. [Прайс Palavir](https://palavir.co/consulting)
- Microsoft Marketplace содержит Copilot implementation workshop за **DKK 39 000**, примерно $5.5–6 тыс. [Marketplace listing](https://marketplace.microsoft.com/en-us/marketplace/consulting-services/itrelationas1670414068047.microsoft_365_copilot_implementation_workshop)
- ITnetX публиковал Copilot readiness package примерно за **CHF 9 680**, около $12 тыс. [Публичный пакет ITnetX](https://itnetx.ch/wp-content/uploads/2025/02/Microsoft-365-Copilot-Readiness-Package_EN.pdf)
- Insight публиковал adoption program до 30 участников за **$25 000**. [Insight service datasheet](https://www.insight.com/content/dam/insight-web/en_US/pdfs/microsoft/everyday-ai-adoption-for-microsoft-365-copilot-program.pdf)

Эти точки согласуются с моделью:

1. дешёвый или бесплатный teaser;
2. платный диагностический sprint;
3. fixed-scope implementation;
4. adoption/evals retainer.

## Где маржа

Высокомаржинальны:

- workflow discovery и prioritization;
- доменные skills и шаблоны;
- обучение на данных и ролях заказчика;
- eval-наборы и acceptance criteria;
- governance, permissions и data boundaries;
- интеграция с CRM, Google Workspace/Microsoft 365, knowledge bases;
- постоянное обновление workflows под изменения моделей;
- доказательство ROI.

Низкомаржинальны и плохо защищены:

- перепродажа seats;
- общие prompt packs;
- однодневный «что такое ChatGPT/Claude»;
- ручная настройка аккаунтов;
- wrapper без интеграции и evals.

Публичных финансовых отчётов именно от устойчивых 1–5-человечных практик почти нет. Reddit-кейсы вроде контракта на $46 тыс. или заявления «$2K → $20K MRR» можно использовать только как market anecdotes, а не как надёжный benchmark.

---

# 4. Что известно про OpenAI SMB Channel Partner Programme

Здесь нужно различать три программы:

1. **OpenAI Partner Network** — глобальная программа build/sell/deliver.
2. **SMB Channel Partner Programme** — ограниченный channel motion, в который вошли отдельные небольшие консалтинги.
3. **ChatGPT for small business program**, запущенная 21 июля 2026 года, — обучение, events, guides и ecosystem offers для самих SMB, а не то же самое, что channel partner program.

OpenAI публично обещает Partner Network:

- co-selling;
- enablement и technical support;
- ресурсы для build/deploy;
- Partner Locator для одобренных организаций.

Заявка открыта, но принятие курируемое: кандидат предоставляет capabilities, technical expertise и customer experience. Публичной таблицы уровней и минимальных требований, сопоставимой с Anthropic Services Track, нет. [OpenAI Partner Network](https://openai.com/business/partners/), [partner portal](https://partners.openai.com/)

### SMB channel

Независимое подтверждение существования отдельного SMB Channel Partner Programme есть у британской gecco. Важная формулировка компании:

> участие — **не licensing arrangement**.

Gecco описывает роль как помощь SMB с adoption, training и delivery, сохраняя multi-model практику — ChatGPT, Claude и Gemini. [Сообщение gecco](https://www.geccohq.com/news/gecco-joins-the-openai-smb-channel-partner-programme)

Это опровергает простое представление, что статус SMB partner автоматически означает reseller margin.

### Promo-лицензии

OpenAI официально подтверждает, что партнёры могут распространять unique promotional codes или links, включая промо для новых подписчиков. Но общие условия говорят только о redemption, eligibility, сроках и anti-abuse; они не говорят, кто может стать таким партнёром и сколько кодов получает. [OpenAI promotions and referrals](https://help.openai.com/en/articles/10492689-chatgpt-plus-promotions-referrals)

Следовательно:

- существование партнёрских промокодов подтверждено;
- наличие таких кодов у конкретного SMB channel partner требует документа или действующей landing page;
- стандартная комиссия и её размер публично не установлены;
- нельзя заключить, что каждый одобренный SMB partner получает promo seats;
- кейс kone.vc может быть частной кампанией или пилотом, а не тиражируемым entitlement программы.

### Честное сравнение

| Фактор | OpenAI | Anthropic |
|---|---|---|
| Открытая заявка для service firms | Да | Да |
| Публичные tier criteria | Нет в сопоставимой детализации | Да, очень подробно |
| Реалистичный buyer-facing tier для соло | Неясно, принятие курируемое | Нет: Select требует 10 certified |
| Публично подтверждённые partner promo codes | Да, но доступ не стандартизирован | Нет общего механизма |
| Публичная reseller margin | Не установлена | Не установлена |
| Product showcase | Partner Locator / ecosystem | Powered by Claude |
| Возможность остаться multi-model | Есть как минимум у gecco | Формального запрета не найдено |
| Лучший solo route | Попытаться войти в SMB channel | Powered by Claude + Registered Network |

Повторить кейс у OpenAI, вероятно, проще именно в части промо, но доступ туда discretionary. Anthropic даёт более прозрачный путь построения профессиональной практики, зато хуже подходит для licensing-led lead magnet.

---

# 5. «AI-аудит до первой встречи» и A2A-пресейл

## Кто делает

Рынок уже использует три близких паттерна:

- **Pre-call research agent:** анализирует сайт, LinkedIn и CRM и выпускает briefing до разговора. Ability.ai заявляет экономию 15–20 минут seller research на звонок. [Ability.ai](https://www.ability.ai/solutions/sales-intelligence/pre-call-research-agent)
- **Free audit with artifact:** Small Business Joe, Helix AI и другие обещают документ, который клиент сохраняет независимо от покупки. [Small Business Joe](https://www.smallbusinessjoe.com/blog/what-free-ai-audit-looks-like), [Helix AI](https://helix-ai.net/audit)
- **Personalized live demo:** Ciela превращает сайт prospect’а в персонализированный chat/voice demo до первой встречи. [Ciela AI](https://ciela.ai/blogs/how-to-get-ai-agency-clients-with-a-free-audit)
- **Consulting audit automation:** Audity автоматизирует discovery, scoring и формирование transformation report. [Audity](https://auditynow.com/)

A2A в данном случае лучше понимать не как обязательное использование формального Google A2A protocol, а как цепочку специализированных агентов:

```text
Prospect research
      ↓
Process hypotheses
      ↓
ROI/risk critic
      ↓
Demo or audit artifact
      ↓
Human approval
      ↓
Personalized outreach
```

Формальный A2A protocol здесь полезен только если независимые agent services действительно должны обмениваться задачами. Для пресейла достаточно Claude Agent SDK/MCP и обычной orchestration.

## Есть ли надёжные conversion measurements

Специфического независимого исследования вида «pre-meeting AI audit повышает close rate с X до Y» не найдено.

Публичные заявления включают:

- AI outbound case с 3.25% qualified-lead conversion среди engaged leads;
- anecdotal lead-response automation: booked-call conversion с 8% до 23%;
- vendor case: удвоение B2B conversion с 5% до 10%;
- анонимное сообщение оператора 30+ кампаний: персонализированная первая строка добавляла лишь около 0.5–1 п.п. reply rate.

Это несопоставимые воронки и преимущественно self-reported данные. Их нельзя переносить в финансовую модель Антона.

Более здравый внешний benchmark для холодного agentic outreach:

- reply rate: 3–8%;
- positive reply: 1–4%;
- meeting booked: 0.5–2%.

Но гипотезу аудита надо измерять собственным A/B-тестом.

## Рекомендуемый эксперимент

На 60 сопоставимых компаниях:

- 30 получают обычное персонализированное сообщение;
- 30 — одностраничный evidence-linked audit + 90-секундное видео;
- одинаковый ICP, отправитель, CTA и cadence.

Измерять:

1. просмотр артефакта;
2. positive reply;
3. booked call;
4. qualified opportunity;
5. paid audit/pilot;
6. часы ручной работы на prospect;
7. false claims или исправления после human QA.

Запрещать агенту:

- писать клиенту автоматически;
- использовать закрытые или персональные данные без основания;
- утверждать, что внутренний процесс компании известен;
- выдавать расчёт ROI как факт;
- использовать логотип Anthropic или «partner» до формального разрешения.

---

# 6. Какой трек даёт максимум social proof за 90 дней

## Рекомендация: Powered by Claude

Не Services Select и не startup credits.

Почему:

- соответствует существующим сильным сторонам Антона: OSS, Agent SDK, skills, fleet orchestration;
- не требует десяти сертифицированных сотрудников и двух прошлых deployments;
- создаёт публично проверяемый объект, а не ещё один сертификат;
- полезен для Anthropic DevRel/product teams независимо от принятия в каталог;
- одновременно является лид-магнитом и демонстрацией consulting delivery;
- может позже стать основанием для Marketplace или Services customer story.

### Какой продукт подать

Не «Palo Alto AI Research Lab» вообще и не generic AI-аудит.

Подать узкий продукт:

> **Claude Evidence Audit — агентная система, которая исследует публичный цифровой контур компании, формирует проверяемые гипотезы автоматизации и создаёт один безопасный workflow prototype с citations, evals и human approval.**

Чтобы это выглядело как Powered by Claude candidate:

- Claude должен быть существенной частью продукта, а не заменяемым API-вызовом;
- публичная demo page;
- 2–3 reproducible example audits;
- архитектурная страница: Claude, Agent SDK/MCP, evals, human gates;
- privacy/data-source policy;
- пользовательские результаты хотя бы двух design partners;
- чёткое описание, какие утверждения являются гипотезами;
- отдельная форма consent для непубличных данных.

## 90-дневный план

### Дни 1–30

- Податься в Claude Partner Network как Registered.
- Подать Claude for Startups, если лаборатория удовлетворяет возрасту/funding criteria.
- Собрать productized audit.
- Получить минимум одну индивидуальную Claude certification, даже если до Select далеко.
- Опубликовать OSS-компонент: eval harness или evidence/citation skill, а не generic wrapper.

### Дни 31–60

- Провести 20 бесплатных pre-call audits.
- Выбрать 3 design partners из одного вертикального сегмента.
- Продать 1–2 пилота.
- Измерить часы, adoption, quality и economic outcome.
- Получить разрешение хотя бы на один публичный customer story.

### Дни 61–90

- Опубликовать reproducible case study.
- Податься в Powered by Claude.
- Передать Anthropic partner/DevRel contact короткий dossier: продукт, OSS, usage, клиенты, eval results.
- Зарегистрировать реальные opportunities в Partner Hub, если функция доступна Registered-участнику.
- Не нанимать девять «номинальных» сертифицированных людей только ради Select.

---

# Итоговая конструкция

## Один трек

**Powered by Claude**, с параллельной административной регистрацией в Claude Partner Network, но без объявления Services partnership до получения соответствующего статуса.

## Один лид-магнит

**48-hour Evidence-Based Claude Workflow Audit**

Клиент получает:

- три гипотезы процессов;
- evidence links;
- ROI range с указанными предположениями;
- risk/data boundary;
- один небольшой работающий demo workflow;
- двухминутное персональное видео.

Не включать бесплатную полноценную архитектуру, production integration или передачу reusable source code.

## Один оффер

**Claude Adoption Pilot — 4 недели, $12–25 тыс.**

Состав:

- один процесс;
- 10–30 пользователей;
- Team/Enterprise setup либо API deployment;
- 3–5 Claude skills/workflows;
- integration с одним источником данных;
- eval suite;
- security/governance checklist;
- две обучающие сессии;
- usage/outcome dashboard;
- 30-дневная поддержка.

Продолжение:

> Claude Operations Retainer: $2.5–5 тыс./мес. за evals, обновления workflows, office hours и один небольшой improvement cycle.

Лицензии оплачивает заказчик напрямую Anthropic. Credits или vendor-funded pilot оформляются отдельно и никогда не обещаются до письменного подтверждения.

---

# Риски

### Эксклюзивность

Публичного требования эксклюзивности Claude Partner Network не найдено. Однако конкретный договор может включать ограничения на co-marketing, brand use или registered opportunities. Проверять Partner Program Guide до подписания.

Практичная позиция:

> Anthropic-first delivery, model-aware architecture.

Это позволяет глубоко специализироваться на Claude, не обещая клиентам бессмысленную пожизненную модельную эксклюзивность.

### ToS и перепродажа

Не покупать Team seats на лабораторию и не раздавать доступ сторонним компаниям как собственный SaaS. Team workspace контролируется его Primary Owner, включая данные, доступ и billing. Клиент должен владеть своей организацией и договором. [Управление Team seats](https://support.claude.com/en/articles/12004354-purchase-and-manage-seats-on-team-plans), [контроль данных организации](https://support.claude.com/en/articles/9265372-who-owns-and-manages-the-data-of-my-team)

Не перепродавать API access или shared accounts без явного договорного разрешения.

### Брендинг

Не использовать:

- “Anthropic partner” до formal acceptance;
- “Services Partner” на уровне Registered;
- `Powered by Claude` как будто это присвоенный статус до листинга;
- логотипы и lockups вне официальных brand guidelines.

Безопасная фактическая формулировка:

> Built primarily with Claude and the Claude Agent SDK.

### Конфликт с OpenAI-нейтральностью

Контент можно разделить на два слоя:

- OSS/исследования: vendor-neutral claims, воспроизводимые benchmarks, честные ограничения;
- коммерческий оффер: Anthropic-first implementation expertise.

Не публиковать сравнительные тесты, в которых:

- Claude получает лучшие prompts/tools;
- модели сравниваются в разных режимах;
- скрывается стоимость;
- лаборатория называется независимой при получении MDF или credits.

Каждая публикация должна раскрывать материальные отношения с Anthropic/OpenAI.

### Риск бесплатного аудита

Аудит может превратиться в дорогой bespoke presales. Ограничить его:

- только публичными данными;
- максимум 45–60 минут agent compute + 15 минут QA;
- один стандартизированный артефакт;
- никаких production credentials;
- ROI только диапазоном;
- demo не подключается к системам клиента.

Главная стратегическая ставка здесь — не «выбить бесплатные seats», а создать продуктовый объект, который одновременно приносит лиды, customer evidence и основание для официального Anthropic social proof. Это гораздо лучше совпадает с сильными сторонами Антона и не требует имитировать channel economics kone.vc.
