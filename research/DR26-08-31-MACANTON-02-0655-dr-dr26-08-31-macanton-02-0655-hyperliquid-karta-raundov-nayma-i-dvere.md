---
dr_id: DR26-08-31-MACANTON-02-0655
title: "DR DR26-08-31-MACANTON-02-0655 · Hyperliquid: карта раундов, найма и дверей (август 2026)"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# DR DR26-08-31-MACANTON-02-0655 · Hyperliquid: карта раундов, найма и дверей (август 2026)

> Синтез трёх НЕЗАВИСИМЫХ рельс. Каждый факт несёт число согласившихся рельс.
> Оригиналы verbatim: `_originals/deep-research/DR26-08-31-MACANTON-02-0655-hyperliquid-ecosystem-hiring-funding-map-{grok,chatgpt,claudeai}.md`

## Честность прогона (чем добыто и чем НЕ добыто)
- ✅ **chatgpt** (codex CLI, web_search) — 25.3 KB, **81 живой URL**, 1456 с. Единственная рельса с настоящими ссылками.
- ✅ **grok** (CLI) — 28.7 KB, 365 с. Источники названы прозой (издание + дата), URL теряет РЕНДЕР, не разведка (порог `MIN_URLS["grok"]=0` намеренный).
- ✅ **claudeai** (Fable 5 + web_search, живая вкладка Chrome) — 16.4 KB.
- ❌ **gemini** — вендор отозвал лицензию Code Assist: `no valid license of this product (#3501)`, exit=1 bytes=0 за 6 с. Не `IneligibleTierError`, рецепт 16.08 через `GOOGLE_CLOUD_PROJECT` НЕ лечит.
- ❌ **glm** — анонимная дверь в in-app pane мертва (тумблеры веб-поиска и флагманской модели не переключаются), логин там требует пароль. В живом Chrome аккаунт bb@ залогинен, прогон запущен, к моменту синтеза отчёта не отдал.
- ⛔ **mistral** — аккаунта на узле нет.
⇒ Кворум 4/6 не взят. Вывод стоит на ТРЁХ рельсах; всё, что подтверждено 1/3, помечено 🤔 и фактом не считается.

## Что подтвердили ВСЕ ТРИ рельсы
- Hyperliquid Labs остаётся самофинансируемым ядром без внешнего VC-раунда — подтверждают: grok, chatgpt, claudeai (источник: Grayscale/Fortune/SEC и материалы Foundation, 2026)
- Labs нанимает внешних людей, но публично только инженеров; публичной BD/institutional-sales вакансии у ядра нет — подтверждают: grok, chatgpt, claudeai (источник: вакансии Labs и публикации команды, октябрь 2025 — август 2026)
- Для BD/капитал-формации рабочие двери находятся в проектах вокруг ядра, а не в Labs — подтверждают: grok, chatgpt, claudeai (источник: продуктовые анонсы и вакансии экосистемы, 2025–2026)
- Kinetiq публично раскрыл seed $1,75 млн; iHYPE предназначен для институционального стейкинга — подтверждают: grok, chatgpt, claudeai (источник: Kinetiq/DefiLlama/GlobeNewswire, 19 июня и 22 октября 2025)
- Hyperbeat привлёк $5,2 млн seed и прямо называл трейдеров, протоколы и институты целевой аудиторией — подтверждают: grok, chatgpt, claudeai (источник: CoinDesk/Hyperbeat, 15 августа 2025)
- HyperLend привлёк $1,7 млн и запустил Aviya как KYC/KYB-gated площадку институционального обеспеченного кредита — подтверждают: grok, chatgpt, claudeai (источник: DefiLlama/HyperLend/Aviya, 19 января — июль 2026)
- Сумма раунда Felix публично не раскрыта; его HIP-3-направление было прекращено в июне 2026 — подтверждают: grok, chatgpt, claudeai (источник: CandyDrops/SEC/постмортем 0xBroze, июнь 2025 — 19 июня 2026)
- HypurrFi сворачивается; Euler забрал Mewler, поэтому это не актуальная дверь найма — подтверждают: grok, chatgpt, claudeai (источник: HypurrFi/Crypto Times, 15–16 мая 2026)
- Unit описывается как самофинансируемый проект без раскрытого внешнего раунда; публичный BD-наём не найден — подтверждают: grok, chatgpt, claudeai (источник: Unit docs и вакансии Unit, 2026)
- Based привлёк $11,5 млн Series A во главе с Pantera; отдельная institutional-BD вакансия не обнаружена — подтверждают: grok, chatgpt, claudeai (источник: The Block, 23 февраля 2026)
- HIP-3 требует залога 500 000 HYPE, позволяет deployer запускать рынки и получать 50% комиссий — подтверждают: grok, chatgpt, claudeai (источник: документация Hyperliquid HIP-3, актуальна на август 2026)
- Builder codes позволяют фронтендам брать программируемую комиссию с потока без разрешения Labs — подтверждают: grok, chatgpt, claudeai (источник: документация Hyperliquid builder codes, актуальна на август 2026)
- У ядра нет договорной программы назначенных маркет-мейкеров; институциональный доступ строят прайм-брокеры, кастодианы, фронтенды и deployers — подтверждают: grok, chatgpt, claudeai (источник: документация market making и анонсы посредников, 2026)

## Где рельсы РАСХОДЯТСЯ
| факт | grok | chatgpt | claudeai | чья версия правдоподобнее и почему |
|---|---|---|---|---|
| Главное окно найма | Не нашёл публичных BD-вакансий; ставит на HYPD/Entropy/Aviya | Нашёл живую Growth Lead в HyperLink | Публичных BD-вакансий в основных native-проектах не нашёл | chatgpt: единственный дал прямую ссылку на живую Ashby-вакансию; статус всё равно надо перепроверить перед откликом |
| HypurrFi в августе 2026 | Сворачивается, бренд закрывается | Сворачивается | Описывает wind-down, но таблица funding map ещё называет product-led | grok + chatgpt, а текст claudeai ниже также подтверждает wind-down: 3/3 по сути |
| Felix HIP-3 | Закрыт 19 июня 2026 | Партнёрство прекращено и свёрнуто | Вышел из USDH-quoted рынков в середине июня | Консенсус 3/3: HIP-3 дверь закрыта; точная дата 19 июня подтверждена только grok |
| Kinetiq общий объём финансирования | Публичная сумма $1,75–1,8 млн; дополнительные строки PitchBook без ясности | $1,75 млн seed | $1,75 млн seed, но Alpha Drops якобы считает около $5,25 млн за 3 раунда | $1,75 млн: совпадает у 3/3; $5,25 млн — 🤔 один голос |
| HypurrFi инвесторы | Называет Robot Ventures, Arrington, Breyer; сумма скрыта | Раунд не верифицирован | Раскрытый раунд не найден | chatgpt + claudeai по стандарту доказательности; список grok оставить гипотезой |
| Unit и trade.xyz | Считает одной родительской связкой | Описывает одной строкой, но не доказывает корпоративную идентичность | Рассматривает рядом, без доказанного юрлица | Не объединять юрлица: функциональная связь правдоподобна, корпоративная — 🤔 не доказана |
| trade.xyz раунд $200 млн | Не приводит раунд как факт | Не включает как подтверждённый | Называет слух и публичное отрицание Cobie | claudeai: корректно маркирует как неподтверждённое; в карту раундов не включать |
| Энтропия: $40 млн HYPE | Slashable bond/staking support, не раунд | В основной таблице отсутствует | Называет $14 млн seed + $40 млн stake | grok + claudeai: $14 млн — капитал, $40 млн — залог, не финансирование компании |
| Hyperliquid Strategies | Даёт подробные показатели PURR и руководство | Не включает в проектную таблицу | Пишет, что статус 2026 не проверен | grok — единственный подробный голос; использовать только как 🤔 до проверки первички |
| Foundation занимается BD | Называет Foundation grants/policy, не sales desk | SEC-описание включает ecosystem development и BD | Публичного ecosystem lead нет; community-BD делают третьи стороны | Совместимая трактовка: функция развития экосистемы есть, но публичной коммерческой команды/воронки найма нет |
| Кто основатель Felix | Charlie / 0xBroze | Charlie Ambrose / @0xBroze | Charlie.hl | grok + chatgpt согласны по handle; полное имя даёт только chatgpt, поэтому имя — 🤔 до второй проверки |
| Кто ведёт HyperLend | 0xNessus, Shimon COO | 0xNessus и fbsloXBT | Псевдонимная команда | 0xNessus подтверждён 2/3; остальные имена и роли требуют проверки |

## Карта проектов (сводная)
| проект | раунд и сумма | стадия | доказательство нужды в BD | дверь | человек | согласие рельс (3/3, 2/3, 1/3) |
|---|---|---|---|---|---|---|
| Hyperliquid Labs / Foundation | $0 внешнего VC | Production L1/HyperEVM; инженерный найм | Публичной коммерческой вакансии нет | Закрыта для холодного BD; сначала измеримый вклад | Jeff Yan, @chameleon_jeff; @HyperFND | 3/3 |
| Kinetiq | $1,75 млн seed | LST, iHYPE, Launch/HIP-3 | iHYPE, кастодиальные и validator-рельсы требуют институциональной дистрибуции | Founder-led institutional/capital markets | Omnia/@0xOmnia; Justin Greenberg; @Kinetiq_xyz | 3/3 |
| Felix | seed, сумма не раскрыта | Кредит/стейблкоин; HIP-3 свёрнут | Нужда остаётся в lending/RWA collateral, не в perps | Точечно: заёмщики и RWA-партнёры | @0xBroze; @felixprotocol | 3/3 |
| Hyperbeat | $5,2 млн seed | Yield, staking API, builder-code distribution | Раунд и API прямо нацелены на institutions/funds/custodians | Интеграции custody, treasury и fund distribution | Kilian Boshoff/@Fundi_Crypto; @hyperbeat | 3/3 по проекту; 2/3 по имени |
| HyperLend / Aviya | $1,7 млн | Money market + permissioned institutional credit | KYC/KYB, частные кредитные линии, сделка Hyperion/Anchorage | Origination кредиторов и заёмщиков | 0xNessus; @hyperlendx | 3/3 |
| HypurrFi | сумма не подтверждена | Wind-down; Mewler у Euler | Нужды в найме нет | Закрыта; смотреть Euler/Clearstar | legacy @HypurrFi | 3/3 |
| Unit | раскрытого раунда нет; self-funded | Asset onboarding/tokenization | Нужны issuers, custodians и MM, но вакансии BD нет | Только с конкретным листингом/потоком | @unitxyz; имя основателя 🤔 1/3 | 3/3 по статусу финансирования |
| Based | $11,5 млн Series A | Builder-code super-app/Cloud | Enterprise/affiliate distribution, но профиль больше retail/embedded | Partnerships/Cloud, не core institutional BD | Edison Lim; @BasedOneX | 3/3 |
| Hyperion DeFi | публичная компания, не обычный venture round | HYPE treasury + HAUS, validators, credit/vault partnerships | Capital markets и origination — часть операционной модели | Institutional partnerships / HAUS origination | Hyunsu Jung; David Knox | 3/3 |
| EntropyIO | $14 млн; $40 млн HYPE — залог, не раунд | Новый HIP-3 deployer | Новые рынки структурно требуют MM, ликвидность и listings | MM/LP origination | @entropyIO; люди не названы | 2/3 |
| HyperLink | отдельная сумма не раскрыта | Hyperliquid prime broker | Живая Growth Lead прямо отвечает за firms/MM/institutions | Прямой отклик | Suki Sohi/@s3ohi | 1/3, но с URL вакансии |
| Silhouette | $3 млн pre-seed | Private RFQ/block execution | Нужен MM/RFQ flow, но Head of Growth и BD уже есть | Senior origination/coverage, не общий BD | Chandler De Kock; Wayne van Niekerk; Json Romero | 1/3 |

## Как реально устроен институциональный онбординг (консенсус)
Ядро не выдаёт «официальный институциональный статус» и не ведёт переговоры через собственную sales-команду.
Фонд или маркет-мейкер может подключиться напрямую через API/agent wallets и разделять стратегии subaccounts.
Кастодиальные и prime-broker рельсы снимают часть требований к ключам, отчётности, марже и контрагентскому риску.
Builder codes превращают кошелёк, брокера или frontend в дистрибьютора: он приводит клиента и получает onchain-комиссию.
Это даёт коммерческую модель без лицензии или индивидуального договора с Labs.
HIP-3 создаёт отдельные рынки силами deployer, но требует slashable-залог 500 000 HYPE.
Deployer отвечает за market/oracle/settlement-параметры и получает 50% комиссий рынка.
Капитал залога можно собирать через Kinetiq Launch или получить через Hyperion HAUS; это и есть одна из ключевых BD-дверей.
HLP — общественный протокольный vault для market making и ликвидаций, а не отдел продаж и не персональная программа для фонда.
Пользовательские vaults дают управляемую стратегию, но имеют lock-up и продуктовые ограничения.
Для регулируемого капитала Kinetiq iHYPE добавляет KYB/KYC и custody-integrated staking.
HyperLend Aviya добавляет bilateral secured credit, KYC/KYB и возможность квалифицированного кастодиана.
Посредничают прайм-брокеры, кастодианы, listed wrappers, HIP-3 deployers и фронтенды, а не relationship manager из Labs.
Практический вход: принести конкретный поток, MM, заёмщика, кредитора, listing или staking ticket к подходящему посреднику.

## Анти-VC статус ядра: вердикт
Да, Hyperliquid Labs / core на 31 августа 2026 отказывается от внешнего VC: все три отчёта называют Labs self-funded и не находят внешнего раунда; chatgpt ссылается на SEC и Foundation, grok — на Grayscale/Fortune и документы Labs, claudeai — на заявления Yan и материалы 2026. Нет, ядро не отказывается от внешнего найма вообще: оно публично нанимает внешних backend/frontend инженеров. Отказ относится к VC и к видимой коммерческой машине — публичных BD, institutional sales или capital-markets ролей у Labs/Foundation три отчёта не нашли.

## ТОП-5 дверей (ранжировано)
1. HyperLink Pte./HyperLink | Единственная найденная живая роль с прямым совпадением: Growth Lead, onboarding trading firms/MM/institutions | Suki Sohi, @s3ohi, Ashby | Подать one-pager с 10–20 конкретными фирмами и планом TVL/onboarding | согласие: 1/3, но chatgpt дал настоящий URL; перепроверить статус
2. Hyperion DeFi (NASDAQ: HYPD) | HAUS, validators, Aviya и vault-партнёрства превращают капитал-формацию в продукт; $35M track record уместен | CEO Hyunsu Jung, CFO David Knox, IR/LinkedIn | Принести один живой HIP-3, secured-credit или institutional-staking mandate | согласие: 3/3
3. Kinetiq | Свежий капитал, iHYPE и Launch требуют распределения институционального стейкинга и 500k-HYPE bond capital | Omnia/@0xOmnia, @Kinetiq_xyz, contact@kinetiq.xyz | Назвать три custody/fund канала и одного deployer, которому нужен bond | согласие: 3/3
4. HyperLend / Aviya | Самая прямая native-DeFi нужда в institutional credit origination | 0xNessus, @hyperlendx | Принести конкретного HYPE-заёмщика либо кредитора и параметры сделки | согласие: 3/3
5. Hyperbeat | $5,2 млн и staking API для funds/custodians/fintechs; подходит сеть фондов и опыт инфраструктуры | Kilian Boshoff/@Fundi_Crypto, @hyperbeat | Предложить custody, treasury или fund-интеграцию с измеримым AUM | согласие: 3/3 по двери, 2/3 по человеку

## 🤔 Не подтверждено / дыры
- 🤔 EntropyIO как реально открытая оплачиваемая роль: два отчёта видят сильную структурную нужду, но ни один не даёт вакансию или названного hiring manager.
- 🤔 HyperLink Growth Lead: сильнейшая улика — прямая ссылка chatgpt, но два других отчёта сущность не упоминают; перед откликом проверить, что вакансия ещё открыта.
- 🤔 Kinetiq total funding около $5,25 млн: это есть только у claudeai; консенсусная раскрытая сумма — $1,75 млн.
- 🤔 Инвесторы HypurrFi Robot Ventures, Arrington и Breyer: только grok; чистого раунд-анонса нет.
- 🤔 Раунд trade.xyz $200 млн при оценке $1,5 млрд: только claudeai и там же публичное отрицание; фактом не считать.
- 🤔 Показатели PURR за август 2026 и объём equity facility: подробно приводит только grok; другие рельсы не подтвердили.
- 🤔 Связь Unit и trade.xyz как одного юрлица/родителя: отчёты показывают тесную продуктовую связь, но не дают согласованного юридического доказательства.
- 🤔 Полные юридические имена и роли команд Unit, EntropyIO, Hyperbeat и части HyperLend остаются неполными или расходятся.
- 🤔 Текущие вакансии BD у Kinetiq, Hyperbeat, HyperLend/Aviya, Hyperion и EntropyIO отсутствуют; у них доказана продуктовая нужда, а не открытый найм.
- Ни один отчёт не даёт полного списка юридических лиц, регистрационных стран и официальных рабочих email всех TOP-дверей.
- Ни один отчёт не доказывает компенсацию, бюджет роли, полномочия нанимающего лица и willingness создать роль под кандидата.

---
## Связи
Потребитель: plan-2026-08-31-hyperliquid-ecosystem-attack-plan (секция «DR-валидация»).
Смежное: insight-2026-08-31-recall-anton-candidate-positioning-full-map, mission-get-noticed-hired-by-llm-company.

## Связано
- DR26-08-31-MACANTON-02-0655-synthesis — evidence_for: полный консенсус-синтез рельс этого ДР
