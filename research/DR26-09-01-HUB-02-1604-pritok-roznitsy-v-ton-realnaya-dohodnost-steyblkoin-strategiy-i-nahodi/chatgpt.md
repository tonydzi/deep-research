# DR26-09-01-HUB-02-1604 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# A. Краткий вердикт

Срез исследования: 10.09.2026, 12:57 BST. В задании указано 01.09.2026; там, где источник обновился после этой даты, это помечено отдельно.

Гипотеза подтверждается только частично: Telegram дал TON уникальный канал дистрибуции, а в 2024 году сеть действительно получила миллионы новых адресов. Но текущие показатели гораздо ниже игрового пика: 2,23 млн активных кошельков за 30 дней и 128 тыс. за сутки на 10.09.2026 против пика 12,06 млн monthly active wallets 02.10.2024; прямого доказательства, что оставшаяся аудитория — «обычные люди, не знающие DeFi», нет. [TONStat](https://www.tonstat.com/), [исторический TONStat через The Coin Republic](https://www.thecoinrepublic.com/2025/05/03/ton-coin-active-addresses-drop-further-dips-ahead/)

Рынок доходности на стейблкоинах жив, но нормальная базовая ставка сентября 2026 года находится примерно в зоне 3–5% годовых; двузначный APY почти всегда добавляет субсидию, плечо, кредитный, деривативный, контрактный или контрагентский риск. TON при этом остаётся тонким DeFi-рынком: около $49,3 млн TVL при $771 млн стейбкоинов на 10.09.2026. [DefiLlama reference rates](https://defillama.com/yields/rates), [DefiLlama TON](https://defillama.com/chain/ton)

Smart-money-аналитика пригодна для поиска кандидатов, но не доказывает воспроизводимую доходность. Особенно важно: Nansen действительно индексирует TON, однако на 10.09.2026 его официальная матрица не показывает для TON ни PnL, ни Smart Money endpoint; готового «Nansen Smart Money для TON» сейчас нет. [Nansen chain matrix](https://docs.nansen.ai/reference/chains), [Nansen Smart Money API](https://docs.nansen.ai/api/smart-money)

---

# B. Вопрос 1. Правда ли розница идёт в TON

## B1. Динамика сети

Сопоставимой открытой помесячной серии с одной неизменной методикой за январь 2024 — сентябрь 2026 найти не удалось. Разные источники смешивают daily active address, outgoing wallet, activated wallet и monthly active wallet, поэтому соединять их в одну гладкую линию было бы методологически неверно.

| Дата | Метрика | Значение | Источник и трактовка |
|---|---:|---:|---|
| январь 2024 | daily active addresses | 26 274 | Artemis через [CryptoSlate](https://cryptoslate.com/ton-blockchain-sees-explosive-3435-growth-in-daily-active-addresses-over-2024/) |
| январь 2024 | транзакции/сутки | 232 286 | тот же источник |
| сентябрь 2024 | daily active addresses | 902 737 | рост примерно в 34 раза к январю; Artemis через [CryptoSlate](https://cryptoslate.com/ton-blockchain-sees-explosive-3435-growth-in-daily-active-addresses-over-2024/) |
| сентябрь 2024 | транзакции/сутки | 2 823 801 | рост примерно в 12 раз; тот же источник |
| сентябрь 2024 | среднее DAA / транзакции за месяц | почти 1 млн / 90 млн | одновременно DEX-volume упал на 64%, то есть большая часть всплеска была не торговой; [OAK Research](https://oakresearch.io/en/reports/sectors/layer-1-activity-report-september-2024) |
| 31.08.2024 | транзакции/сутки | 8,68 млн | тогдашний рекорд; [The Block](https://www.theblock.co/news/ecosystems/2024-09-09-ton-hits-record-transaction-highs-post-durov-arrest-while-token-price-plummets-30-315360) |
| 02.10.2024 | monthly active wallets | 12 063 774 | исторический максимум TONStat, воспроизведённый вторичным источником; [The Coin Republic](https://www.thecoinrepublic.com/2025/05/03/ton-coin-active-addresses-drop-further-dips-ahead/) |
| конец апреля 2025 | monthly active wallets | 2 471 463 | на 79,5% ниже пика октября 2024 года; тот же вторичный источник |
| 11.12.2024 → 18.05.2025 | daily active wallets | 1,23 млн → 129 142 | падение на 89,5%; [CCN со ссылкой на TONStat](https://www.ccn.com/news/crypto/telegram-trading-bots-ton-wallets-decline/) |
| первое полугодие 2025 | daily active addresses | примерно 218 600 → 161 000 | нисходящий тренд; [Binance Research H1 2025](https://public.bnbstatic.com/static/files/research/half-year-report-2025.pdf) |
| 10.09.2026 | active wallets, rolling 30d | 2 226 343 | текущий счётчик [TONStat](https://www.tonstat.com/) |
| 10.09.2026 | active wallets, сутки | 128 395 | кошельки с исходящей транзакцией; [TONStat](https://www.tonstat.com/) |
| 10.09.2026 | новые активированные кошельки/сутки | 35 400 | «активация» означает хотя бы одну исходящую транзакцию; [TONStat](https://www.tonstat.com/) |
| 10.09.2026 | транзакции/сутки | 3 393 975 | [TONStat](https://www.tonstat.com/) |
| 10.09.2026 | активированные кошельки, накопительно | 58 174 338 | накопительный показатель, не число текущих людей; [TONStat](https://www.tonstat.com/) |
| 10.09.2026 | active / new addresses / tx за 24h | 126 963 / 43 214 / 3,33 млн | независимая витрина [DefiLlama](https://defillama.com/chain/ton); определения отличаются от TONStat |

Текущие 2,23 млн monthly active wallets на 81,5% ниже пика 02.10.2024. Сеть сохранила материальную активность, но форма кривой — резкий игровой взрыв, обвал и более низкое плато, а не непрерывный массовый приток.

## B2. Что реально изменилось между Telegram и TON

С 21.01.2025 Telegram обязал новые blockchain Mini Apps использовать TON и TON Connect. Существующим Mini Apps был дан срок миграции до 21.02.2025; обычные Telegram-боты без блокчейн-интеграции под правило не подпадают. [Telegram Blockchain Guidelines](https://core.telegram.org/bots/blockchain-guidelines)

Это сильный дистрибуционный moat, но одновременно platform risk: спрос может формироваться не органическим выбором сети, а политикой одного приложения.

Внутри Telegram существуют два разных продукта:

- Crypto Wallet — кастодиальный мультичейн-сервис с KYC, управляемый TG Wallet Inc.; внутренние переводы между пользователями могут быть off-chain. Сервис прямо говорит, что юридически независим от Telegram. [Wallet Help, актуально на 10.09.2026](https://help.wallet.tg/article/113-wallet-where-to-start)

- DeFi Account, ранее TON Wallet — self-custodial TON-адрес с TON Connect и доступом к dApps, разработанный TON Space Ltd. [Wallet Help, обновлено 01.09.2026](https://help.wallet.tg/article/1032-welcome-to-defi-account)

Поэтому «активировал Wallet», «пользователь Telegram» и «активный ончейн-кошелёк TON» — три разные метрики.

### География

- США: кастодиальный Crypto Wallet на 10.09.2026 остаётся в списке недоступных юрисдикций. Self-custodial TON Wallet был запущен в США 22.07.2025; заявление о потенциальном охвате почти 87 млн пользователей — пресс-релиз самого Wallet, а не измеренная аудитория. [Wallet launch](https://wallet.tg/news/ton-wallet-launches-in-the-united-states-for-almost-87-m-telegram-users), [ограничения](https://help.wallet.tg/article/113-wallet-where-to-start)

- ЕС/ЕЭЗ: Crypto Wallet недоступен во многих, но не во всех странах ЕС. Переходный период MiCA для CASP закончился 01.07.2026; дальнейшая доступность зависит от конкретной лицензии, страны и продукта. [ESMA](https://www.esma.europa.eu/sites/default/files/2026-04/ESMA75-113276571-1679_Statement_on_the_end_of_transitional_periods_under_MiCA.pdf)

- Россия и часть СНГ не перечислены в опубликованном на 10.09.2026 blacklist Crypto Wallet. Это не гарантирует доступ отдельному человеку: остаются KYC, санкционные и локальные ограничения. [Wallet Help](https://help.wallet.tg/article/113-wallet-where-to-start)

- Азия фрагментирована: в актуальном списке ограничений присутствуют, в частности, Бангладеш, Пакистан, Филиппины, Таиланд, Мьянма и Макао. [Wallet Help](https://help.wallet.tg/article/113-wallet-where-to-start)

Павел Дуров был задержан во Франции 24.08.2024, а в марте 2025 года ему разрешили вернуться в Дубай при продолжении расследования. Прямой причинной связи между французским делом и продуктовой политикой Wallet/TON опубликованные данные не доказывают. [AP](https://apnews.com/article/421a69e62ca419ff50d48a11fb944187), [TechCrunch](https://techcrunch.com/2025/03/17/telegram-founder-pavel-durov-allowed-to-leave-france/)

## B3. Стейблкоины на TON

### Историческая траектория USDT

Официальный отчёт TON приводит следующую supply-серию, но это отчёт заинтересованной экосистемной стороны:

| Месяц | USDT supply на TON |
|---|---:|
| май 2024 | $94,4 млн |
| июнь 2024 | $346,1 млн |
| июль 2024 | $548,7 млн |
| август 2024 | $619,2 млн |
| сентябрь 2024 | $620 млн |
| октябрь 2024 | $768 млн |
| ноябрь 2024 | $1,0 млрд |
| декабрь 2024 | $1,2 млрд |
| январь 2025 | $1,2 млрд |
| февраль 2025 | $1,4 млрд |

Источник: [TON Jan–Feb 2025 report](https://storage.googleapis.com/ton-strapi/ton_report_jan_feb2025_05079eade3/ton_report_jan_feb2025_05079eade3.pdf).

### Текущий снимок

| Сеть, 10.09.2026 | Все стейблкоины | 30d | Крупнейшие активы |
|---|---:|---:|---|
| Tron | $94,17 млрд | +2,66% | USDT $92,26 млрд; [DefiLlama](https://defillama.com/stablecoins/tron) |
| Solana | $16,32 млрд | +4,92% | USDC $7,18 млрд; USDT $2,77 млрд; [DefiLlama](https://defillama.com/stablecoins/solana) |
| BSC | $13,29 млрд | −0,64% | USDT $9,18 млрд; [DefiLlama](https://defillama.com/stablecoins/bsc) |
| Base | $5,05 млрд | +3,12% | USDC $4,30 млрд; [DefiLlama](https://defillama.com/stablecoins/base) |
| TON | $769,84 млн | −3,21% | USDT $605,69 млн; USDe $164,16 млн; [DefiLlama](https://defillama.com/stablecoins/ton) |

На 10.09.2026 USDT занимал 78,68% stablecoin supply TON и за предыдущие 30 дней сократился на 9,63%. USDe вырос на 31,23% за тот же период, а FDUSD составлял лишь около $1 086. [DefiLlama TON](https://defillama.com/stablecoins/ton)

Текущий USDT supply приблизительно на 56,7% ниже заявленных экосистемой $1,4 млрд февраля 2025 года. Сравнение не идеально: историческое число поступает из отчёта TON, текущее — из агрегатора DefiLlama.

В абсолютном масштабе на 10.09.2026 Tron был примерно в 122 раза больше TON по stablecoin supply, Solana — в 21 раз, BSC — в 17 раз, Base — в 6,6 раза. На месячном горизонте TON терял долю относительно Tron, Solana и Base, поэтому тезис «доля TON растёт сейчас» текущими данными опровергается.

По данным Tether от 16.10.2024, 3,3 млн TON-адресов хотя бы раз получили USDT за первые шесть месяцев после запуска. Tether отдельно признал, что для TON использовались данные Artemis без кластеризации: один человек мог иметь много адресов, а однократное получение тоже считалось «user». [Tether methodology](https://tether.io/news/how-many-usdt-users-are-there/)

Актуальное количество уникальных держателей USDT и USDe на TON на сентябрь 2026 года в надёжном открытом источнике не найдено. Доля стейблкоин-переводов в полном экономическом обороте TON также не подтверждена.

## B4. Люди, боты и сибилы

TON Foundation описывает методику очистки Dune-данных: оставить wallet/multisig interfaces и исключить masterchain, известные организационные адреса, sybil-наборы и более 8 млн известных адресов кастодианов, бирж, ботов и платёжных сервисов. Авторы признают, что полностью выявить неизвестных ботов и сибилов невозможно. [TON/Dune methodology, 31.07.2025](https://blog.ton.org/how-to-analyze-ton-users-and-token-flows-on-dune)

Даже после фильтра остаются нерешёнными три задачи:

- один человек может контролировать много адресов;

- один адрес может принадлежать сервису или группе людей;

- ончейн-история не показывает, знает ли владелец DeFi.

Автоматизированная активность была материальной. В апреле 2025 года торговые боты Blum, по вторичному анализу Dune и отчёта TON, давали около 25% ончейн-активности TON и примерно $10 млн недельного trading volume; число interactions достигло более 850 тыс. 09.04.2025, после чего упало более чем на 75%. [CCN](https://www.ccn.com/news/crypto/telegram-trading-bots-ton-wallets-decline/)

## B5. Что произошло с tap-to-earn

По Helika, retention Telegram-игр в Q4 2024 составлял 5–20% против 20–30% у традиционных игр. Hamster Kombat сообщал о пике более 155 млн MAU, но к ноябрю 2024 года осталось 41 млн, то есть падение минимум на 73,5%; исследователи отдельно указывали, что botting и farming мешают считать реальных игроков. [The Block/Helika, 07.02.2025](https://www.theblock.co/news/web3/2025-02-07-telegram-games-had-trouble-earning-revenue-retaining-users-in-q4-report-339563)

После snapshot Hamster 20.09.2024 и airdrop 26.09.2024 daily active wallets TON снизились примерно с 1,47 млн 30.09.2024 до 52 951 07.10.2024, или на 96,4%. Новые активации упали с 284 246 19.09.2024 до 70 704 07.10.2024, или на 75,1%; это сильное временное совпадение, но не строгий причинный эксперимент. [CCN](https://www.ccn.com/news/crypto/hamster-kombat-game-telegram-craze-ton-fizzles/)

Notcoin сообщал о 35 млн игроков к airdrop 20.05.2024, тогда как Telegram Mini App показывал 580 890 monthly users 10.09.2026. Это несопоставимые метрики — накопленные игроки против текущего MAU, — но сохранения пикового масштаба они не показывают. [Binance Research H1 2024](https://public.bnbstatic.com/static/files/research/half-year-report-2024.pdf), [Notcoin Telegram App](https://telegram.me/notcoin_bot)

Ключевой ответ: приток адресов подтверждён; приток именно «обычных людей без знания DeFi» — **не подтверждён**. Лучше всего наблюдаемую кривую объясняет смесь tap-to-earn, airdrop farming, ботов, спекуляции и части органической пользовательской активности.

---

# C. Вопрос 2. Живые стратегии доходности

## C1. Инвентарь классов

APY ниже — текущие или близкие к текущим снимки, а не обещание будущей ставки.

| Класс | Механика и источник денег | Наблюдаемая доходность и размер | Главный риск |
|---|---|---|---|
| Lending | Заёмщики платят процент поставщикам ликвидности; ставка меняется с utilization. [Aave](https://aave.com/help/supplying/supply-tokens) | На 10.09.2026 средневзвешенная base supply rate крупных EVM-рынков без rewards — 3,64%; Spark 1,85%, Aave V3 3,31%, Morpho 3,86%, Maple 4,89%, Euler 8,06%, но Euler имел лишь 0,2% веса. [DefiLlama rates](https://defillama.com/yields/rates) | Bad debt, неправильный оракул, недостаточная ликвидация, exploit, отсутствие ликвидности вывода. [Morpho](https://docs.morpho.org/curate/concepts/security-considerations/) |
| Delta-neutral basis/funding | Покупается spot/yield-актив и открывается встречный short; доход дают funding, basis, staking/lending reserves. [Ethena](https://docs.ethena.fi/) | sUSDe: 4,80% APY и $1,327 млрд TVL на 10.09.2026. [DefiLlama sUSDe](https://defillama.com/yields/pool/66985a81-9c51-46ca-9977-42b4fe7bc6df) | Funding становится отрицательным, basis исчезает, hedge не исполняется, CEX/кастодиан блокирует средства, collateral депегается. |
| Yield-tokenization | Доходный актив делится на principal token и yield token; PT покупается с дисконтом и погашается к maturity, YT получает переменный доход и затем обнуляется. [Pendle](https://docs.pendle.finance/pendle-v2/ProtocolMechanics/YieldTokenization/PT) | LP sUSDe с maturity 26.11.2026: headline 5,33%, включая 4,71% base и 0,62% PENDLE rewards; TVL $3,67 млн, снимок 07.09.2026. [DefiLlama](https://defillama.com/yields/pool/afdef3b3-8c37-5156-9c39-c2849e20f7a8) | Риск базового актива плюс maturity, ошибочная оценка implied yield, reward-токен и ликвидность выхода. |
| RWA/T-bills | Купоны и рост NAV казначейских бумаг за вычетом issuer/custody fees. | Категория T-bills: $14,24 млрд TVL на 04.09.2026; BUIDL около 3,56% при $992 млн, USDY 3,57% при $1,175 млрд, USYC 3,19% при $2,561 млрд. [DefiLlama T-bills](https://defillama.com/protocols/treasury-bills), [yields](https://defillama.com/yields) | Кастодиан, эмитент, санкционная заморозка, KYC, банковские часы, ограниченный redemption и расхождение цены с NAV. |
| LP в stable-пуле | Трейдеры платят комиссии; сверху могут начисляться governance rewards. | Curve crvUSD/USDT: 3,45%, но лишь 0,19% от fees и 3,26% от rewards; TVL $54,35 млн, снимок 03.09.2026. Uniswap V3 USDC/USDT: 1,97%, TVL $34,14 млн, 10.09.2026. [Curve](https://defillama.com/yields/pool/90a3e412-e3be-47e1-a233-fd3760f4a794), [Uniswap](https://defillama.com/yields/pool/e737d721-f45c-40f0-9793-9f56261862b9) | При депеге арбитражёры оставляют LP с худшим активом; contract risk, out-of-range liquidity и падение rewards. |
| Curated/structured vault | Пользователь вносит один актив, а куратор распределяет его по разрешённым рынкам и берёт fee. | Morpho Steakhouse Prime USDC на Base: 4,03% APY и $20,9 млн supplied, снимок 07.09.2026. [DefiLlama](https://defillama.com/yields/pool/7820bd3c-461a-4811-9f0b-1d39c1503c3f) | Ошибка куратора, плохой collateral, oracle failure, socialized bad debt, adapter risk и bank run. |

Практическая базовая ставка сентября 2026 года — примерно 3–5% годовых. Доходность заметно выше этого уровня не обязательно плохая, но требует назвать источник дополнительного spread: кредит, leverage, funding, токеновая субсидия, неликвидность или tail risk.

## C2. Что существует непосредственно в TON

Снимок DefiLlama на 10.09.2026:

- весь DeFi TVL TON — около $49,3 млн;

- stablecoin supply — около $771,0 млн;

- отношение TVL/stablecoin supply — примерно 6,4%, но это не точная доля стейблкоинов в DeFi, поскольку TVL включает другие активы и производные позиции. [DefiLlama TON](https://defillama.com/chain/ton)

Основные продукты:

| Протокол | Размер и доходность | Что это |
|---|---|---|
| EVAA | $8,99 млн TVL на 10.09.2026; USDT supply APY 2,22% на $3,21 млн supplied и borrow APY 3,43% на $2,61 млн borrowed, снимок 05.09.2026 | TON-native lending. [TON chain](https://defillama.com/chain/ton), [EVAA pool](https://defillama.com/yields/pool/73afa0b0-a513-4fdd-a9af-09c6ceac109b) |
| Affluent | $2,47 млн TVL на 10.09.2026; отдельный USDT pool показывал 4,97% на всего $37 112 supplied примерно 20.08.2026 | lending/vault layer; ставка такого малого пула непригодна как массовый benchmark. [Affluent](https://defillama.com/yields/pool/251b8e07-374b-43e0-b000-7fc709d59c70) |
| Ethena tsUSDe | $3,13 млн TON TVL на 10.09.2026 | обёртка внешней стратегии Ethena; не TON-native basis desk. [DefiLlama TON](https://defillama.com/chain/ton), [Ethena](https://docs.ethena.fi/) |
| STON.fi | $24,97 млн TVL на 10.09.2026 | DEX целиком, а не чистый stablecoin product. [DefiLlama TON](https://defillama.com/chain/ton) |
| Torch / TONCO / SettleTON | соответственно около $841 тыс. / $300 тыс. / $83,5 тыс. TVL на 10.09.2026 | небольшие LP/yield-продукты. [DefiLlama TON](https://defillama.com/chain/ton) |

Один Ethereum Aave USDT pool имел около $2,96 млрд TVL на 10.09.2026 — более чем в 250 раз больше совокупного TVL EVAA и Affluent. [DefiLlama yields](https://defillama.com/yields), [TON](https://defillama.com/chain/ton)

Вывод: TON уже позволяет сделать lending, LP и yield-wrapper, но глубина рынков, разнообразие collateral, число ликвидаторов, оракулов, hedging venues и профессиональных curators несопоставимы с EVM.

### Аудиты

- EVAA публикует Trail of Bits и Quantstamp audits с версиями и commit hashes. [EVAA contracts](https://github.com/evaafi/contracts)

- Для Affluent DefiLlama показывал `Audits: No`, но репозиторий проекта содержит review protocol февраля 2025 года и vault/RFQ review июня 2025 года. Это конфликт метаданных, а не доказательство отсутствия проверки. [DefiLlama](https://defillama.com/yields/pool/251b8e07-374b-43e0-b000-7fc709d59c70), [audit repository](https://github.com/factorial-finance/audits)

- Trail of Bits публиковал проверки STON.fi в январе 2025 года, FIVA/FIVA-EVAA в мае 2025 года и EVAA в августе 2025 года. [Trail of Bits publications](https://github.com/trailofbits/publications)

Любой аудит относится к конкретной версии и scope и не гарантирует отсутствия уязвимостей. [TON audit guidance](https://docs.ton.org/audits)

## C3. Headline APY против реального результата

Полезная формула:

`net yield = interest/funding/coupon/fees + реально проданные rewards − protocol/curator/performance fees − gas/bridge/slippage/hedging − bad debt/depeg/liquidation loss − налоги`.

Ключевые корректировки:

- APY annualizes текущую или короткую ставку. Aave прямо указывает, что supply rate меняется вместе с utilization и параметрами рынка. [Aave](https://aave.com/help/supplying/supply-tokens)

- Reward APY использует текущую цену reward-токена. В Curve-примере на 03.09.2026 из headline 3,45% лишь 0,19% приходилось на торговые комиссии, а 3,26% — на токеновые rewards. [DefiLlama Curve](https://defillama.com/yields/pool/90a3e412-e3be-47e1-a233-fd3760f4a794)

- Leveraged loop приблизительно даёт `L × supply yield − (L−1) × borrow rate − costs`. При росте borrow rate положительный spread исчезает, но риск ликвидации остаётся.

- Fiat-backed stablecoin добавляет банковский и issuer risk; synthetic dollar — funding, hedge и counterparty risk; RWA — legal/custody/redemption risk.

- Единой посленалоговой APY нет. Результат зависит от резидентства, формы продукта, момента признания дохода и возможности учитывать убытки.

## C4. Как стратегии вели себя в стрессе

| Событие | Что сломалось | Улика |
|---|---|---|
| Anchor/UST, май 2022 | Anchor рекламировал около 19–20% годовых; из примерно $19 млрд UST около $14 млрд находилось в Anchor. Алгоритмический peg и субсидируемая ставка разрушились вместе. | [SEC amended complaint](https://www.sec.gov/files/terraform-labs-pte-ltd-amended-complaint.pdf); это регуляторная жалоба, не ончейн-аудит |
| USDC/SVB, 10–13.03.2023 | Circle временно не могла получить $3,3 млрд, около 8% резервов; USDC падал приблизительно до $0,86, а стресс распространился через DAI/PSM. | [Federal Reserve, 17.12.2025](https://www.federalreserve.gov/econres/notes/feds-notes/in-the-shadow-of-bank-run-lessons-from-the-silicon-valley-bank-failure-and-its-impact-on-stablecoins-20251217.html) |
| Curve/Vyper, 30.07.2023 | У stable-LP материализовался не рыночный, а контрактный reentrancy-риск. | [LlamaRisk postmortem](https://llamarisk.com/research/curve-pool-reentrancy-exploit-postmortem) |
| Aave, 10.10.2025 | Более $250 млн ликвидаций после примерно $1,7 млрд вывода стейблкоинов; протокол заявляет, что выдержал событие. | [Aave, 06.02.2026](https://aave.com/blog/historical-liquidations); self-report протокола |
| Aave, 31.01–05.02.2026 | Ещё $429 млн ликвидаций примерно в 12 500 транзакциях без заявленного downtime. | тот же [Aave report](https://aave.com/blog/historical-liquidations) |
| USDe на Binance, 10.10.2025 | Venue-specific depeg/oracle/liquidation failure; Binance компенсировала затронутых пользователей. Это не доказательство неплатёжеспособности Ethena. | [Binance](https://www.binance.com/en/support/announcement/detail/0989d6c7f32545bfb019e3249eaabc3f) |
| Stream Finance xUSD, ноябрь 2025 | Внешний fund manager заявил об убытке $93 млн; withdrawals были остановлены, xUSD снижался примерно до $0,94–0,88. | [Gauntlet Vaultbook](https://vaultbook.gauntlet.xyz/resources/market-volatility); сумма не подтверждена полным независимым аудитом |
| Resolv USR, 22.03.2026 | Компрометация ключа/off-chain infrastructure позволила выпустить необеспеченный USR; извлечено около $23 млн, после чего произошёл depeg. | [Chainalysis](https://www.chainalysis.com/blog/lessons-from-the-resolv-hack/) |
| TAC bridge, 11.05.2026 | Эксплойт моста в TON/TAC-экосистеме; на 12.05.2026 оценка потерь составляла около $2,854 млн. | [TAC postmortem](https://tac.build/blog/post-mortem-report-tac-bridge) |

«Безопасная доходность» чаще ломается не из-за одного APY, а из-за скрытой зависимости: банк, оракул, мост, admin key, CEX, управляющий или невозможность быстрой ликвидации.

## C5. Где одна кнопка, а где нужен оператор

| Уровень | Продукт | Что скрыто от пользователя |
|---|---|---|
| Одна кнопка | CEX Earn, custodial wallet earn, managed vault | Площадка сама выбирает размещение и контролирует вывод; пользователь получает counterparty и terms risk |
| Почти одна кнопка | Aave supply, простой Morpho vault, EVAA/TON Wallet Earn | Нужны правильная сеть, approval, gas и понимание self-custody |
| Нужен оператор | basis/funding carry, leveraged loops, concentrated LP, Pendle YT, cross-chain routing | Margin, hedge, maturity, borrow rate, bridge, oracle и ликвидность выхода |

В Ethena permissionless-пользователь может приобрести USDe через внешний рынок, но прямой mint/redeem доступен одобренным KYC/KYB market makers. Документация также исключает предложение sUSDe резидентам ЕС/ЕЭЗ. [Ethena](https://docs.ethena.fi/)

Граница проходит не по числу кнопок, а по тому, кто управляет tail risk. Если интерфейс делает это сам, розница покупает риск оператора; если пользователь делает сам, это уже не пассивный массовый продукт.

## C6. Регуляторный слой

### США

GENIUS Act стал Public Law 119-27 18.07.2025. Он запрещает permitted payment stablecoin issuer платить interest/yield исключительно за holding, use или retention платёжного стейблкоина. [GovInfo](https://www.govinfo.gov/app/details/PLAW-119publ27), [текст в U.S. Code](https://uscode.house.gov/view.xhtml?edition=prelim&path=%2Fprelim%40title12%2Fchapter56)

На 10.09.2026 основной режим ещё не действовал полностью. Treasury 17.08.2026 называло ожидаемой датой 18.01.2027 и опубликовало NPRM; OCC также находилось на стадии proposed rules, а final rule ожидалось в ноябре 2026 года. [Treasury](https://home.treasury.gov/news/press-releases/sb0605), [OCC proposed rule](https://occ-gov.cms.awsdev.treasury.gov/news-issuances/bulletins/2026/bulletin-2026-3/), [OCC timetable](https://occ.treas.gov/news-issuances/news-releases/2026/nr-occ-2026-69.html)

Запрет направлен на доход, который платит сам эмитент за простое держание. Отдельный lending, vault, broker reward или securities product не получает автоматического разрешения: его надо анализировать по banking, securities, commodities, consumer-protection и state law. [CRS analysis](https://www.everycrsreport.com/reports/IN12553.html)

### ЕС

MiCA Article 50 запрещает эмитентам e-money tokens и CASP предоставлять interest по EMT и связанным услугам. Определение охватывает выгоду, зависящую от времени владения, включая экономически эквивалентные конструкции. [EUR-Lex](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32023R1114)

Совместный отчёт EBA/ESMA от 16.01.2025 указывает, что crypto lending/borrowing не регулируется MiCA как самостоятельная деятельность. Это не означает «разрешено всем»: применяются национальные режимы, финансовое право, раскрытие рисков и правила конкретной структуры. [EBA/ESMA](https://www.esma.europa.eu/sites/default/files/2025-01/ESMA75-453128700-1391_Joint_Report_on_recent_developments_in_crypto-assets__Art_142_MiCA_.pdf)

Итог: ни в США, ни в ЕС нельзя делать универсальное заявление «stablecoin earn легален для всей розницы». Законность определяется тем, кто платит доход, откуда он возникает, какие лицензии есть и является ли продукт займом, ценной бумагой, фондом или обходом запрета на interest.

---

# D. Вопрос 3. Кто такие «фармеры» и как проверять их результат

## D1. Реальные роли

| Роль | Что делает | Как зарабатывает |
|---|---|---|
| Индивидуальный yield-оператор | Переносит собственный капитал между lending, LP, Pendle, funding и incentive-программами | Собственный PnL; число таких людей и медианный заработок не подтверждены |
| Allocator/strategist | Перераспределяет vault между разрешёнными рынками и следит за utilization/liquidity | Зарплата, grant или доля fee |
| Vault/risk curator | Выбирает рынки, collateral, caps, oracle, withdrawal queue и risk policy | Management/performance fee |
| DAO risk-provider | Моделирует ликвидность, collateral и liquidation parameters | Фиксированный DAO-контракт |
| Delta-neutral desk | Держит spot/yield leg и short leg на CEX/perp-DEX | Funding, basis, spread и incentives |
| Treasury-manager | Управляет резервами DAO, RWA, lending, LP и операционной ликвидностью | Retainer плюс иногда performance fee |

В Morpho curator задаёт рынки и caps, allocator управляет размещением внутри этих ограничений. Euler Earn формально разделяет owner, curator, allocator и guardian. [Morpho](https://docs.morpho.org/learn/concepts/curator/), [allocations](https://docs.morpho.org/curate/tutorials-v1/manage-allocations/), [Euler Earn](https://docs.euler.finance/use/euler-earn/)

Morpho V2 допускает protocol ceilings до 50% performance fee и 5% management fee в год; Euler Earn — до 50% performance fee. Это технические максимумы, не рыночная средняя. [Morpho fees](https://docs.morpho.org/curate/concepts/fee/), [Euler](https://docs.euler.finance/curate/vaults/euler-earn/deployment-and-management/)

На 10.09.2026 Morpho показывал около $4,083 млрд assets under curation, 37 curators и около $12,111 млн annualized curation fees. На Steakhouse приходилось около $1,809 млрд, Sentora — $1,018 млрд, Gauntlet — $331 млн. [Morpho curation dashboard](https://data.morpho.org/curation)

Препринт декабря 2025 года оценивал curated-vault TVL Aave/Morpho/Euler/Maple/Gearbox/Silo на 19.11.2025 примерно в $7,27 млрд. На Gauntlet приходилось около $2 млрд, Steakhouse — $1,29 млрд, MEV Capital — $915 млн; это подтверждает рынок профессиональных кураторов, но также концентрацию риска. [arXiv 2512.11976](https://arxiv.org/abs/2512.11976)

## D2. Как доказать результат уликами

Минимальный стандарт проверки:

1. Адреса публикуются до начала периода, а контроль доказывается подписью сообщения.

2. Учитываются все пополнения, выводы, bridges, sibling-wallets и связанные CEX transfers.

3. Realised и unrealised PnL разделяются; депозит не считается прибылью.

4. Interest, funding и trading fees показываются отдельно от reward-токенов.

5. Rewards оцениваются по цене фактической продажи, а не по пиковой цене начисления.

6. Из результата вычитаются gas, commissions, slippage, borrow interest, bridge и curator fees.

7. Публикуются leverage, drawdown, liquidation events и капитал под риском.

8. Показывается доля прибыли от top-1/top-3 сделок и результат без них.

9. В исходную выборку включаются закрытые и ликвидированные кошельки, а не только выжившие.

Nansen определяет Total PnL как realised плюс unrealised PnL, а ROI — относительно стоимости купленных токенов; доступны окна 7 дней, 90 дней, один год и all-time. Это хороший скринер, но он не подтверждает наличие CEX hedge, связанных адресов или чужого капитала. [Nansen Token God Mode](https://academy.nansen.ai/help/articles/3874203-token-god-mode-101)

Zerion использует FIFO и отдельно показывает realised/unrealised PnL, fees, net invested, deposits и withdrawals. Активы без цены исключаются, а PnL endpoint не поддерживает кошельки с более чем 1 млн транзакций. [Zerion PnL](https://developers.zerion.io/recipes/wallet-pnl-tracker), [limitations](https://developers.zerion.io/api-reference/wallets/get-wallet-pnl)

Hyperliquid позволяет хорошо проверить сделки внутри своей площадки, но portfolio graph сэмплируется каждые 15 минут и не отражает внешние spot, lending и CEX legs. Отображаемый frontend PnL также не заменяет reconciliation по executions и margin. [Hyperliquid graphs](https://hyperliquid.gitbook.io/hyperliquid-docs/trading/portfolio-graphs), [PnL](https://hyperliquid.gitbook.io/hyperliquid-docs/trading/entry-price-and-pnl)

## D3. Инструменты на сентябрь 2026 года

Цены ниже проверены 10.09.2026 и могут меняться.

| Инструмент | Что реально даёт | TON | Цена | Чего не даёт |
|---|---|---|---|---|
| Nansen | Entity/Smart Money labels, holdings, flows, trades, profiler и PnL на поддерживаемых сетях | Balances, transactions, transfers, holders и related wallets; нет TON PnL и Smart Money | Pro $49/мес. при годовой оплате или $69 помесячно. [Pricing](https://academy.nansen.ai/articles/9412804-about-nansen-pro) | Полный wallet cluster, CEX legs, доказанную личность или risk-adjusted skill |
| Arkham | Entity labels, graph tracing, alerts, wallet/entity pages | TON live с 05.11.2024, включая transfers, entity/token pages и custom labels. [Launch](https://info.arkm.com/announcements/ton-is-now-live-on-arkham) | UI доступен; фиксированный публичный API-тариф не найден. [API](https://arkm.com/api) | Гарантию правильной атрибуции; labels остаются confidence-based |
| Dune | Raw/decoded SQL, dashboards, воспроизводимые queries | TON подключён 19.12.2024. [Dune](https://dune.com/blog/ton-blockchain-is-now-live-on-dune) | Free 2 500 credits/мес.; Analyst $75/мес.; Plus $399/мес. [Pricing](https://docs.dune.com/resources/credits-billing/how-credits-work) | Готовый авторитетный список «умных фермеров» |
| DefiLlama | TVL, yields, stablecoins, fees, borrow, hacks, treasury data | Полная TON chain page | Core free; Pro $40,83/мес. при годовой оплате; API $250/мес. [Subscription](https://defillama.com/subscription) | Identity и career PnL управляющего |
| DeBank | EVM portfolio, debt, positions, history и net-worth curves | TON не подтверждён; продукт EVM-oriented | 1 млн compute units за 200 USDC. [Cloud](https://open.debank.com/) | TON и проверенный Smart Money leaderboard |
| Zerion | EVM/Solana portfolio, positions, PnL и webhooks | TON отсутствует в текущем списке. [Networks](https://developers.zerion.io/supported-blockchains) | Free 2 000 requests/day; Builder $149/мес.; Startup $499/мес. [API](https://zerion.io/api) | TON; полный PnL при неизвестных ценах и CEX legs |
| Artemis | Chain/app fundamentals, users, stablecoin и developer metrics | TON есть в asset catalog. [Catalog](https://www.artemis.ai/docs/data-reference/asset-metrics-catalog) | Lite free; Pro $300/мес. или $250/мес. при годовой оплате. [Pricing](https://www.artemis.xyz/pricing) | Wallet attribution и farmer PnL |
| Kaito | Поиск по social, governance, research и attention metrics | TON wallet analytics не подтверждён | Фиксированный публичный Kaito Pro tariff не найден | Связь X username с wallet и доказанный trading skill. [Yaps](https://docs.kaito.ai/kaito-yaps-tokenized-attention/yaps-open-protocol) |

### Nansen и TON: окончательный ответ

Партнёрство было объявлено 09.01.2025. Первый этап включал Query и Growth Dashboards; Hot Contracts, Smart Alerts, Token God Mode и Profiler были обозначены как последующие функции. [Nansen–TON announcement](https://nansen.ai/post/nansen-and-ton-blockchain-partner-to-deliver-next-gen-on-chain-analytics)

К 10.09.2026 Nansen действительно индексирует TON:

- Token God Mode: flows, transfers, holders;

- Profiler: balance, transactions, related wallets;

- PnL: нет;

- Smart Money endpoints: TON отсутствует.

Это следует из текущей [матрицы сетей Nansen](https://docs.nansen.ai/reference/chains) и [перечня Smart Money networks](https://docs.nansen.ai/api/smart-money).

Следовательно, утверждение «Nansen уже даёт Smart Money-лейблы TON» **опровергается публичной документацией**. Для TON нужен собственный scoring поверх Dune, Arkham и raw Nansen/TON data.

## D4. Работает ли копирование Smart Money

### Аргументы «за»

Peer-reviewed исследование, опубликованное в сентябре 2025 года и охватившее более 180 млн Ethereum-token transactions, обнаружило связь между usage intensity и последующей доходностью, а также отрицательную связь exchange inflows/intention-to-trade с returns. Это доказывает информационность агрегированных ончейн-потоков, но не прибыльность копирования конкретного адреса. [Journal of International Money and Finance](https://www.sciencedirect.com/science/article/pii/S0261560625001433)

Nansen строит Smart Trader labels не только по размеру кошелька, а по win rate, ROI, realised profit, количеству прибыльных токенов, разнообразию и поведению в просадках. Используются окна 30/90/180 дней. [Nansen Smart Money 101](https://academy.nansen.ai/en/help/articles/2132837-smart-money-101)

### Аргументы «против»

Working paper на 2,7 млн Ethereum-адресов не обнаружил устойчивого сохранения прошлой торговой результативности и прямых доказательств informed trading. Это наиболее близкая найденная работа к проверке persistence кошельков, но она не прошла полноценный peer review. [SSRN](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4481308)

Препринт января 2026 года о memecoin copy trading выделяет manipulative bots, неопределённость будущей результативности выбранного адреса и execution lag. Его модель предлагает фильтрацию, но не доказывает универсальный net alpha после издержек. [arXiv 2601.08641](https://arxiv.org/abs/2601.08641)

Механические причины расхождения:

- транзакция становится видна только после включения в блок;

- копирующий получает другую цену и slippage;

- наблюдаемый адрес может быть одной ногой hedge;

- label по прошлой прибыли создаёт selection/survivorship bias;

- стратегия может быть не масштабируема на другой капитал;

- market-maker transfer легко спутать с directional bet;

- рыночный режим может измениться раньше, чем обновится label.

Независимого аудированного исследования, доказывающего устойчивую прибыльность именно Nansen Smart Money после latency, fees и slippage, найти не удалось. Систематический front-running кошельков именно вследствие публичного Nansen/Arkham label также **не подтверждён**.

## D5. Где искать профессионалов

Рабочие места поиска — публичные governance и curator-процессы:

- [Morpho Governance Forum](https://forum.morpho.org/);

- Aave governance и RFP для risk providers;

- Euler curator ecosystem;

- Gnosis/karpatkey treasury reports;

- публичные risk reviews Gauntlet, Chaos Labs и LlamaRisk;

- ETHCC, Consensus и специализированные stablecoin/risk мероприятия.

Aave использует публичный RFP-процесс с bidder threads, Q&A, Snapshot и AIP; для service-provider engagements свыше $250 тыс. описан формальный конкурс. [Aave RFP framework](https://governance.aave.com/t/temp-check-implementation-of-an-rfp-framework-for-service-provider-engagements/14643)

В августе 2026 года обновлённый Aave governance framework также требовал публичного business case и risk/technical assessment для листинга активов. [Aave framework v2](https://governance.aave.com/t/arfc-governance-framework-v2/25348)

Тезис «любой cold DM — моветон» слишком сильный и **не подтверждён**. Однако недоверие к unsolicited DM рационально: Discord запрещает массовые непрошеные сообщения, а криптосообщества регулярно предупреждают, что scam начинается с личного сообщения. [Discord Guidelines](https://discord.com/guidelines), [Discord anti-spam guidance](https://discord.com/safety/360044104071-Tips-against-spam-and-hacking)

Лучше работает публичная улика полезности: risk memo, воспроизводимый dashboard, governance contribution, найденная ошибка или тёплое знакомство. Персонализированное сообщение по конкретной публичной работе не равно веерному cold spam.

## D6. Скам-слой

### Реальные кейсы

- 29.05.2026 SEC обвинила Nathan Fuller в привлечении около $12,3 млн примерно от 150 инвесторов под обещания AI high-frequency arbitrage. По заявлению SEC, инвесторам обещались 40–50% за 30–45 дней и более 100% за 21 день; не менее $6,2 млн было присвоено, а около $5,5 млн использовано для Ponzi-like payments. [SEC](https://www.sec.gov/enforcement-litigation/litigation-releases/lr-26558); это обвинение, не окончательный приговор.

- 22.04.2025 SEC заявила, что PGI Global привлекла около $198 млн под гарантированную прибыль от crypto/FX и AI auto-trading; более $57 млн, по версии SEC, было присвоено. [SEC PGI Global](https://www.sec.gov/newsroom/press-releases/2025-69)

- 28.02.2025 DOJ описало fake investment platform, где пользователям показывали нарисованную прибыль, разрешали небольшой ранний вывод, а затем требовали фиктивный «налог». FBI идентифицировало 33 жертвы с потерями около $4,9 млн. [DOJ Ohio](https://www.justice.gov/usao-ndoh/pr/ohio-woman-loses-life-savings-cryptocurrency-investment-scam)

- FTC сообщила в апреле 2026 года о более чем $7,9 млрд заявленных потерь от investment scams за 2025 год и медианной потере более $10 тыс. [FTC](https://consumer.ftc.gov/consumer-alerts/2026/04/people-losing-big-investment-scams-learn-how-spot-and-avoid-them)

### Красные флаги

- гарантированный APY или отсутствие отрицательных периодов;

- «AI/arbitrage bot» без exchange statements и API audit trail;

- деньги отправляются на один EOA или кастодиальный аккаунт управляющего;

- нет verified contract, vault address, bytecode, multisig и withdrawal policy;

- прибыль существует только внутри собственного интерфейса;

- для вывода требуют новый депозит, «налог» или комиссию;

- referral/MLM economics;

- delta-neutral заявлена, но hedge legs и leverage не раскрыты;

- показываются только скриншоты, а не подписанные адреса и выгружаемые операции;

- маленький ранний вывод используется как доказательство платёжеспособности.

CFTC отдельно предупреждает, что AI не превращает trading bots в машины гарантированной прибыли. В кейсе Mirror Trading International более $1,7 млрд BTC от как минимум 23 тыс. людей ушли в схему, обещавшую не менее 10% в месяц; фактически значительная часть была Ponzi. [CFTC](https://www.cftc.gov/LearnAndProtect/AdvisoriesAndArticles/AITradingBots.html)

---

# E. Вопрос 4. Проверка гипотезы на разрушение

## E1. Где гипотеза ломается

### 1. Пик TON уже был, а не только предстоит

Monthly active wallets снизились с 12,06 млн 02.10.2024 до 2,23 млн 10.09.2026. Tap-to-earn обеспечил дешёвое привлечение адресов, но не доказал долговременное хранение капитала или покупку yield-продуктов. [TONStat](https://www.tonstat.com/), [исторический источник](https://www.thecoinrepublic.com/2025/05/03/ton-coin-active-addresses-drop-further-dips-ahead/)

### 2. Stablecoin pool TON уменьшился

USDT вырос с $94,4 млн в мае 2024 года до заявленных $1,4 млрд в феврале 2025 года, но на 10.09.2026 составлял около $605,7 млн. За последние 30 дней к 10.09.2026 весь stablecoin supply TON снизился на 3,21%, тогда как Tron, Solana и Base росли. [TON report](https://storage.googleapis.com/ton-strapi/ton_report_jan_feb2025_05079eade3/ton_report_jan_feb2025_05079eade3.pdf), [DefiLlama](https://defillama.com/stablecoins/ton)

### 3. TON — дистрибуционная сеть, но пока не глубокий доходный рынок

Около $49,3 млн DeFi TVL на 10.09.2026 недостаточно, чтобы безболезненно принять массовый retail flow размером, сопоставимым даже с небольшой долей $771 млн стейбкоинов. Большие суммы неизбежно меняют utilization, APY, slippage и risk concentration. [DefiLlama TON](https://defillama.com/chain/ton)

### 4. «Простой интерфейс» не убирает финансовый риск

Он лишь переносит управление риском от пользователя к кошельку, куратору, CEX или vault operator. После этого критическими становятся governance keys, лицензия, redemption, oracle, bridge и качество управляющего.

### 5. Smart Money не решает задачу найма автоматически

Публичный адрес не показывает личность, команду, CEX hedge, обязательства, чужой капитал и все проигравшие кошельки. На TON Nansen не даёт готового PnL/Smart Money слоя, поэтому стоимость собственной атрибуции будет существенной. [Nansen chain matrix](https://docs.nansen.ai/reference/chains)

## E2. Рынок уже занят

- TON Wallet уже интегрирует встроенные earn-сценарии через EVAA, STON.fi, Affluent и внешнюю Ethena-инфраструктуру. Это означает, что базовая идея «yield внутри Telegram» уже не greenfield. [TON Wallet archive, просмотр 10.09.2026](https://t.me/s/tonwallet_news_en/66)

- KAST на 10.09.2026 рекламировал два vault-а с APY до 7%, один из которых курируется Gauntlet; сам продукт подчёркивает отсутствие банковской страховки и наличие contract, liquidity и counterparty risk. [KAST Earn](https://www.kast.xyz/earn)

- Coinbase предлагает USDC rewards только подходящим пользователям и регионам; ставка показывается в аккаунте, а баланс не является FDIC/SIPC-insured. [Coinbase](https://help.coinbase.com/en/coinbase/coinbase-staking/rewards/usd-coin-rewards-faq)

- Binance 07.09.2026, уже после указанного заказчиком cutoff 01.09.2026, объявила временную акцию 08–22.09.2026: до 7% только на первые 800 USDT, из которых 4% — bonus и около 3% — real-time APR; выше лимита оставалась примерно базовая ставка. [Binance](https://www.binance.com/en/support/announcement/detail/2d943377ecae4ef18a33cc90047a1d80)

Следовательно, конкурировать придётся не с пустотой, а с кошельками, CEX, curator-vaults и карточно-платёжными приложениями, уже скрывающими DeFi под одной кнопкой.

## E3. Где на самом деле находится stablecoin-розница

На 10.09.2026 Tron содержал $94,17 млрд стейбкоинов против $769,84 млн на TON. USDT составлял $92,26 млрд Tron supply. [DefiLlama Tron](https://defillama.com/stablecoins/tron), [TON](https://defillama.com/stablecoins/ton)

Заказанный Tron Network отчёт за Q4 2025 указывал примерно 2,8 млн daily active users, из которых около 78% занимались P2P-переводами; 56% глобальных retail-sized USDT transfers до $1 000 приходилось на Tron. Это отчёт, заказанный Tron, поэтому он полезен как directional evidence, но не независим. [CoinDesk Research/Tron Q4 2025](https://www.coindesk.com/research/tron-network-q4-2025)

Artemis оценивал подтверждённые stablecoin payments за январь 2023 — август 2025 примерно в $136 млрд, а annualized run-rate августа 2025 — примерно в $118 млрд. В provider-attributed payment data Tron был ведущей сетью, но выборка охватывала 22 фирмы плюс оценки ещё 11 и не представляла всю экономику. [Artemis Stablecoin Payments](https://www.stablecoin.fyi/)

Chainalysis оценивал рост APAC crypto transaction volume с $1,4 трлн до $2,36 трлн между периодами июль 2023 — июнь 2024 и июль 2024 — июнь 2025, то есть примерно на 69%. Латинская Америка выросла примерно на 63%, Sub-Saharan Africa — на 52%; это общий crypto volume, а не число retail stablecoin users. [Chainalysis 2025 Adoption Index](https://www.chainalysis.com/blog/2025-global-crypto-adoption-index/)

В Аргентине, Колумбии и Бразилии стейблкоины составляли более половины покупок криптоактивов за июль 2024 — июнь 2025. При этом основной интерфейс часто был CEX или локальный финансовый сервис, а не самостоятельный DeFi wallet. [Chainalysis Latin America 2025](https://www.chainalysis.com/blog/latin-america-crypto-adoption-2025/)

Base на 10.09.2026 имел $5,05 млрд стейбкоинов, в 6,6 раза больше TON, и встроенную дистрибуцию через Coinbase. Solana имела $16,32 млрд, а BSC — $13,29 млрд. [DefiLlama Base](https://defillama.com/stablecoins/base), [Solana](https://defillama.com/stablecoins/solana), [BSC](https://defillama.com/stablecoins/bsc)

Наиболее сильная альтернативная гипотеза:

> массовая stablecoin-розница приходит не «в DeFi-сеть», а в удобный финансовый интерфейс — CEX, Telegram Wallet, необанк, remittance app или карточный продукт; блокчейн под ним может быть пользователю безразличен.

## E4. Что должно оказаться правдой

| Проверяемое условие | Метрика | Как проверить |
|---|---|---|
| 1. Новые пользователи действительно новые для DeFi | доля first-time funded humans без предыдущей DeFi-истории | KYC-linked cohort Wallet/CEX плюс кластеризованный onchain history; публичных данных сейчас нет |
| 2. Они хотят доход, а не только переводы | share stablecoin holders, сделавших первый earn deposit; median deposit; 30/90/180d retention | Wallet product analytics плюс vault deposit contracts |
| 3. Доход сохраняется после субсидий | 90/180-day stable-denominated TWR после gas, slippage, fees и realised reward sales | onchain event reconstruction; incentives показывать отдельной строкой |
| 4. TON выдерживает масштаб | APY/slippage/utilization/redemption time при росте TVL в 5–10 раз | EVAA/Affluent/STON pool data, stress simulation, oracle/liquidator capacity |
| 5. Smart-money поиск даёт дополнительную ценность | walk-forward PnL выбранных при t0 кошельков против control после latency/costs | заранее зарегистрировать адреса, включить проигравшие и sibling-wallets, не менять выборку задним числом |
| 6. Найденный farmer управляет риском, а не демонстрирует удачу | multi-cycle return, drawdown, leverage, tail losses и доля прибыли top-3 сделок | подписанные адреса, CEX statements, proof of liabilities и независимый reconciliation |

До прохождения этих тестов рынок можно считать интересной гипотезой дистрибуции, но не доказанным спросом на retail stablecoin yield.

Что следует из анализа:

- TON разумно рассматривать как канал привлечения, а не как доказанно лучший источник yield.

- Профессиональных curators и delta-neutral operators проще искать там, где уже есть глубокие рынки: Morpho, Euler, Aave, Ethena, Pendle и DAO treasury ecosystems.

- Smart-money данные следует использовать как shortlist, а не как рейтинг людей, которым можно доверить капитал.

- Главный недостающий эксперимент — не ещё один dashboard адресов, а cohort test: реальные первые депозиты, удержание и net yield после всех издержек.

Это вывод о структуре рынка, не инвестиционная рекомендация.

---

# F. Таблица основных источников

| Источник | Что взято | Дата данных/проверки | Надёжность |
|---|---|---|---|
| [TONStat](https://www.tonstat.com/) | текущие active/activated wallets и транзакции | 10.09.2026 | первичный сетевой счётчик; адреса не равны людям |
| [DefiLlama TON](https://defillama.com/chain/ton) | TVL, activity, protocol sizes | 10.09.2026 | агрегированные ончейн-данные |
| [DefiLlama stablecoins TON](https://defillama.com/stablecoins/ton) | supply USDT/USDe и 30d-динамика | 10.09.2026 | агрегированные ончейн-данные |
| [DefiLlama Tron](https://defillama.com/stablecoins/tron) | stablecoin comparison | 10.09.2026 | агрегированные ончейн-данные |
| [DefiLlama Solana](https://defillama.com/stablecoins/solana) | stablecoin comparison | 10.09.2026 | агрегированные ончейн-данные |
| [DefiLlama Base](https://defillama.com/stablecoins/base) | stablecoin comparison | 10.09.2026 | агрегированные ончейн-данные |
| [DefiLlama BSC](https://defillama.com/stablecoins/bsc) | stablecoin comparison | 10.09.2026 | агрегированные ончейн-данные |
| [DefiLlama lending rates](https://defillama.com/yields/rates) | base EVM lending rates | 10.09.2026 | агрегированные ончейн-данные |
| [TON ecosystem report](https://storage.googleapis.com/ton-strapi/ton_report_jan_feb2025_05079eade3/ton_report_jan_feb2025_05079eade3.pdf) | исторический USDT supply | май 2024 — февраль 2025 | пресс-релиз/отчёт заинтересованной стороны |
| [Tether methodology](https://tether.io/news/how-many-usdt-users-are-there/) | 3,3 млн receiving addresses и ограничения подсчёта | 16.10.2024 | первичный эмитент, заинтересованная сторона |
| [Telegram blockchain rules](https://core.telegram.org/bots/blockchain-guidelines) | обязательность TON для Mini Apps | с 21.01.2025 | первичный продуктовый норматив |
| [Wallet Help](https://help.wallet.tg/article/113-wallet-where-to-start) | custody model и география Crypto Wallet | открыт 10.09.2026 | первичный сервис, заинтересованная сторона |
| [Wallet DeFi Account](https://help.wallet.tg/article/1032-welcome-to-defi-account) | self-custody model и география | обновлено 01.09.2026 | первичный сервис |
| [Helika/The Block](https://www.theblock.co/news/web3/2025-02-07-telegram-games-had-trouble-earning-revenue-retaining-users-in-q4-report-339563) | gaming retention и Hamster MAU | 07.02.2025, данные Q4 2024 | отраслевая аналитика; Helika связана с экосистемой |
| [TON/Dune methodology](https://blog.ton.org/how-to-analyze-ton-users-and-token-flows-on-dune) | фильтрация custodial/bot/sybil addresses | 31.07.2025 | воспроизводимая методика заинтересованной стороны |
| [Aave documentation](https://aave.com/help/supplying/supply-tokens) | механика lending | открыто 10.09.2026 | первичная техническая документация |
| [Morpho security](https://docs.morpho.org/curate/concepts/security-considerations/) | oracle, bad debt и vault risks | открыто 10.09.2026 | первичная техническая документация |
| [Ethena docs](https://docs.ethena.fi/) | synthetic-dollar mechanics и ограничения | открыто 10.09.2026 | первичная документация заинтересованной стороны |
| [Pendle docs](https://docs.pendle.finance/pendle-v2/ProtocolMechanics/YieldTokenization/PT) | PT/YT mechanics | открыто 10.09.2026 | первичная техническая документация |
| [Federal Reserve](https://www.federalreserve.gov/econres/notes/feds-notes/in-the-shadow-of-bank-run-lessons-from-the-silicon-valley-bank-failure-and-its-impact-on-stablecoins-20251217.html) | USDC/SVB stress | 17.12.2025, событие марта 2023 | независимая регуляторная аналитика |
| [Aave liquidation report](https://aave.com/blog/historical-liquidations) | liquidation cascades | 06.02.2026 | self-report протокола |
| [Chainalysis Resolv](https://www.chainalysis.com/blog/lessons-from-the-resolv-hack/) | key compromise и USR depeg | 22.03.2026 | независимая forensic-аналитика |
| [GovInfo GENIUS Act](https://www.govinfo.gov/app/details/PLAW-119publ27) | текст закона | 18.07.2025 | первичный правовой источник |
| [Treasury NPRM](https://home.treasury.gov/news/press-releases/sb0605) | implementation status | 17.08.2026 | первичный регулятор |
| [EUR-Lex MiCA](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32023R1114) | Article 50 | действует на 09.2026 | первичный правовой источник |
| [EBA/ESMA](https://www.esma.europa.eu/sites/default/files/2025-01/ESMA75-453128700-1391_Joint_Report_on_recent_developments_in_crypto-assets__Art_142_MiCA_.pdf) | perimeter crypto lending | 16.01.2025 | официальный регуляторный отчёт |
| [Morpho curation](https://data.morpho.org/curation) | curators, AUC и fees | 10.09.2026 | protocol/onchain dashboard |
| [Nansen chain matrix](https://docs.nansen.ai/reference/chains) | TON feature coverage | 10.09.2026 | официальная продуктовая документация |
| [Nansen–TON announcement](https://nansen.ai/post/nansen-and-ton-blockchain-partner-to-deliver-next-gen-on-chain-analytics) | дата и первоначальный scope интеграции | 09.01.2025 | пресс-релиз заинтересованной стороны |
| [Arkham TON launch](https://info.arkm.com/announcements/ton-is-now-live-on-arkham) | TON coverage | 05.11.2024 | официальная продуктовая документация |
| [Dune TON launch](https://dune.com/blog/ton-blockchain-is-now-live-on-dune) | TON raw/decoded data | 19.12.2024 | официальная продуктовая документация |
| [JIMF research](https://www.sciencedirect.com/science/article/pii/S0261560625001433) | информационность ончейн-потоков | сентябрь 2025 | peer-reviewed independent research |
| [SSRN wallet persistence](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4481308) | слабая persistence wallet performance | открыт 10.09.2026 | working paper, не peer-reviewed |
| [SEC Nathan Fuller](https://www.sec.gov/enforcement-litigation/litigation-releases/lr-26558) | AI arbitrage allegations | 29.05.2026 | первичный регулятор; обвинение не равно приговору |
| [DOJ Ohio](https://www.justice.gov/usao-ndoh/pr/ohio-woman-loses-life-savings-cryptocurrency-investment-scam) | fake platform и withdrawal-tax scam | 28.02.2025 | первичный правоохранительный источник |
| [Artemis Stablecoin Payments](https://www.stablecoin.fyi/) | payments volume и chain distribution | данные до августа 2025 | независимая аналитика с неполной выборкой |
| [Chainalysis Adoption Index](https://www.chainalysis.com/blog/2025-global-crypto-adoption-index/) | региональная динамика | июль 2024 — июнь 2025 | независимая blockchain-аналитика |
| [CoinDesk/Tron report](https://www.coindesk.com/research/tron-network-q4-2025) | P2P и retail-sized transfer metrics | Q4 2025 | исследование, заказанное Tron |

---

# G. Чего узнать не удалось

- Полная сопоставимая помесячная серия активности TON за январь 2024 — сентябрь 2026 с одной методикой.

- Текущее число уникальных людей, держащих USDT или USDe на TON.

- Доля stablecoin transfer volume в полном экономическом обороте TON.

- Доля пользователей TON после полного исключения неизвестных ботов, сибилов и custodial addresses.

- Какая доля пользователей действительно «не знает DeFi».

- 30/90/180-day cohort retention всех tap-to-earn-пользователей.

- Причинная связь между французским делом Дурова и изменениями Wallet/TON.

- Реализованная доходность типичного retail-пользователя после gas, bridge, slippage, reward sales, bad debt и налогов.

- Проверяемый средний PnL индивидуальных farmers и delta-neutral desks.

- Масштабная TON-native стратегия класса Pendle или собственный TON basis/funding engine.

- Полный TON leaderboard с realised multi-wallet PnL, leverage и drawdown.

- Nansen Smart Money labels для TON; официальная документация их, наоборот, не подтверждает.

- Полная публичная цена Arkham API и Kaito Pro.

- Независимое доказательство, что Nansen/Arkham labels сами систематически создают front-running.

- Универсальное правило, что любой персонализированный cold DM запрещён или считается моветоном во всех закрытых DeFi-сообществах.

- Единый ответ о легальности stablecoin-yield для всей розницы США или ЕС без анализа конкретной структуры, лицензии и юрисдикции.

🧒 Простыми словами: TON получил огромную толпу через Telegram-игры, но большая часть толпы ушла после раздач. Доллары в сети есть, однако их сейчас меньше пика, а настоящий DeFi-рынок TON очень маленький. «Умный кошелёк» может подсказать, куда смотреть, но не доказывает, что его хозяин стабильно умеет зарабатывать. Значит, сначала нужен маленький проверяемый тест на реальных новичках и чистой доходности, а не вера в красивые адреса и APY.

Конец среза: 10.09.2026, 12:59 BST.
