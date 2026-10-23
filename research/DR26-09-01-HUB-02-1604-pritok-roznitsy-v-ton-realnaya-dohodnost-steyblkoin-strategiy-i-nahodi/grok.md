# DR26-09-01-HUB-02-1604 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Сначала подниму живые источники по TON, стейблам и smart-money, затем сверю цифры по первоисточникам.Дальше открою первоисточники: DefiLlama, Tether, Messari, Nansen docs и живые дашборды.Нужны ещё живые цифры по Aave/Pendle/RWA, покрытие Arkham/Dune по TON и статус скамов.A. Краткий вердикт

Гипотеза «в TON идёт обычная розница со стейблкоином, которой нужен простой доход, значит надо снова следить за smart money и искать фармеров» на сентябрь 2026 подтверждается только в одной узкой части: Telegram реально держит огромную офчейн-аудиторию и кастодиальный @wallet/самокастодиальный TON Wallet, а USDT на TON существует как «лежащий» доллар внутри мессенджера. Она ломается в трёх местах. Во-первых, ончейн-активность TON после пика tap-to-earn 2024 стабилизировалась на низком уровне, а конверсия Mini Apps в ончейн — около 0,01%; прямого доказательства притока «обычных людей, не знающих DeFi» нет. Во-вторых, стейблкоин-доходность на самой TON почти отсутствует: DeFi TVL сети ~$51 млн против ~$49 млрд у Ethereum, а живой yield-слой — единицы миллионов долларов. В-третьих, Nansen индексирует TON на уровне макро/Token God Mode, но флагманский Smart Money TON не покрывает; Arkham TON в 2026 снял. Следить за фармерами имеет смысл на Ethereum/Base/Solana, не на TON. Проверяемые условия, при которых гипотеза ещё может ожить, связаны с нативным Gram Wallet (бета с 31 августа 2026) и ростом мелких P2P-переводов USDT, а не с TVL.

B. Вопрос 1 — идёт ли розница в TON

B.1. Активность сети 2024 → сентябрь 2026

Пик был в сентябре 2024 и совпал с tap-to-earn, не с «платежами обычных людей». По сводке TonStat через CoinEx: средний дневной актив кошельков в 2024 ~461 955, пик ~2,48 млн 13 сентября 2024; транзакции пиковали ~18,15 млн 27 сентября 2024. В 2025 среднее DAW ~149 069, в 2026 YTD ~111 438.1(https://www.coinex.land/en/insight/REPORT/69fd58fad32a4a6b11f25ced)

Messari (State of TON Q1 2026, отчёт 8 июня 2026, данные до 31 марта 2026): 171,9 млн транзакций в квартале (−4,0% QoQ), среднее ~1,91 млн tx/день; среднее DAA ~90 790 (−8,8% QoQ с 99 540). Отчёт прямо пишет: счётчик транзакций шёл вниз весь 2025 от пика 2024, который «гнался играми вроде Notcoin и Hamster Kombat»; Q4 2025 и Q1 2026 — два квартала стабилизации, не нового роста.2(https://messari.io/report/state-of-the-open-network-q1-2026)

Живые срезы на 1 сентября 2026 расходятся по методике, и это важно не смешивать:

- DefiLlama chain/ton: DeFi TVL $51,01 млн; 24h active addresses 134 315; new addresses 37 696; transactions 3,26 млн. Дата съёма: 1 сентября 2026.3(https://defillama.com/chain/ton)
- Dune TON Weekly (неделя 35, 24–30 августа 2026): 29,2 млн tx, 1,8 млн accounts, 697,2k wallets, 310,7k new wallets. Неделя 31: 23,4 млн tx / 519,9k wallets / 250,1k new. То есть новые кошельки всё ещё штампуются сотнями тысяч в неделю при активных кошельках <1 млн.4(https://dune.com/ton_foundation/ton-weekly)
- Nansen Chain God Mode (макросравнение сетей, снимок в интерфейсе 2026): Ton 570 285 active addresses / 3 979 547 transactions / $55,06k fees за выбранное окно (похоже на 7d-агрегат; второй снимок: 526 019 / 3 805 886 / $47,82k). Для сравнения в том же окне: Tron ~8 млн адресов, Solana ~20 млн.5(https://app.nansen.ai/macro/blockchains)
- CoinLaw / TonStat на 27 июня 2026: 2 912 254 daily tx, 55,3 млн activated wallets, 102 856 daily actives, 4 738 537 monthly active wallets. Это TON Foundation/TonStat, не независимый аудитор.6(https://coinlaw.io/toncoin-statistics/)

Тренд СЕЙЧАС (сентябрь 2026): не коллапс и не новый розничный бум. Транзакции держатся в диапазоне ~2–4 млн/день (иногда всплески до 5–6 млн), дневные активные адреса по независимым источникам — порядка 90–170 тысяч, а не миллионы 2024 года. Месячные активные кошельки на уровне нескольких миллионов при десятках миллионов «активированных когда-либо» — классический разрыв «потрогали сеть / остались». CryptoQuant (15 июня 2026) сам формулирует это: ~2,16 млн daily tx и ~155k daily active wallets при 162+ млн кумулятивных активаций.7(https://cryptobriefing.com/cryptoquant-ton-blockchain-native-metrics/)

Помесячный ряд 2024→2026 в одном каноническом датасете публично не выложен целиком (The Block держит график DAA 7DMA за пейволлом). Доступные якоря: пик сентябрь 2024 → обвал 4Q 2024 → плато 2025 на ~150k DAW → Q1 2026 ~91k DAA Messari → лето 2026 100–170k в зависимости от определения «wallet vs account».

B.2. Telegram × TON после 2024: продукт, а не риторика

Что реально изменилось.

1. Кастодиальный Crypto Wallet (@wallet, The Open Platform) остаётся отдельным продуктом. По Forbes (22 апреля 2026) «примерно 15% базы Telegram держит кошелёк» — это ~150 млн аккаунтов при заявленном 1 млрд MAU. Цифра от стороны экосистемы, не ончейн. Кастодиальный режим недоступен в США; американцам с июля 2025 дают самокастодиальный TON Wallet.8(https://www.forbes.com/sites/digital-assets/2026/04/22/inside-telegrams-plan-to-turn-a-billion-people-into-crypto-users/)

2. География кастодиального @wallet жёсткая. Официальный user agreement wallet.tg относит к Restricted Jurisdictions США и почти весь ЕС/ЕЭЗ/Великобританию, плюс Иран, КНДР, Кубу, Афганистан и длинный хвост. Help-центр (обновлён 1 сентября 2026) подтверждает: US, UK, большинство EU — restricted для custodial; self-custody доступнее. Германия/Италия/Испания в сторонних сводках числятся исключениями для части функций — это не официальный список TON Foundation, а вторичная компиляция. Россия в списке Restricted Jurisdictions кастодиального Wallet как страна-запрет не стоит; зато zero-fee покупка USDT картой в DeFi Account недоступна в России, Беларуси, Украине.9(https://wallet.tg/user_agreement)

3. Арест Дурова (август 2024) не «убил» продукт, но юридический хвост жив. К июню 2026 французское дело описывается как формальное расследование с риском до 10 лет и штрафа; Telegram при этом в мае 2026 стал крупнейшим валидатором TON и «основным драйвером» сети вместо TON Foundation (анонс Дурова 4 мая 2026). Это смена стейварда, не смена протокола.10(https://crypto.news/telegram-takes-back-ton-inside-the-2026-takeover/)

4. Ребрендинг Toncoin → Gram (июнь 2026, референдум 81,22%) — смена тикера 1:1, контракты не переезжали.6(https://coinlaw.io/toncoin-statistics/)

5. Нативный некостодиальный Gram Wallet. Анонс Дурова 21 июля 2026: «largest rollout of a non-custodial wallet». На 1 сентября 2026 это уже не чистый пресс-релиз: TON Strategy Company (Nasdaq: TONX) 1 сентября 2026 пишет, что 31 августа Дуров объявил Gram Wallet доступным ограниченной группе, раскатка на базу — «следующие несколько недель»; Config -123 задаёт архитектуру кошелька на уровне сети. Кастодиальный бот параллельно ребрендируют в Walt (торговый хаб). Это заявленный продуктовый сдвиг, но ончейн-эффект на 1 сентября измерить нельзя: раскатка началась вчера.11(https://www.tonstrat.com/ton-strategy-company-highlights-ton-upgrade-establishing-network-level-infrastructure-for-telegrams-native-gram-wallet/)

6. Что НЕ изменилось в продукте. Ончейн-конверсия Mini Apps в Q1 2026: в среднем 152 Mini Apps с ончейн-активностью в день и 13 980 Mini App DAA при 150–190 млн MAU Mini Apps на стороне Telegram. Implied conversion ~0,01%. Mini App DAA = 15,4% от 90 789 DAA сети. Большая часть Telegram-экономики остаётся офчейн (игры, реклама, Stars).2(https://messari.io/report/state-of-the-open-network-q1-2026)

B.3. Стейблкоины на TON vs Tron / Solana / Base / BSC

Живые цифры на 1 сентября 2026.

USDT по сетям (stables.cool, 1 сентября 2026, 09:00–14:30 UTC; глобальный USDT $181,56 млрд, ATH $189,89 млрд 8 мая 2026):

- Tron $91,79 млрд — 50,6%
- Ethereum $84,79 млрд — 46,7%
- Solana $2,83 млрд — 1,6%
- Aptos $731,7 млн — 0,4%
- TON $605,8 млн — 0,3%
- Avalanche $324,3 млн — 0,2%

DefiLlama Tether breakdown на ту же дату: Tron 91,785 млрд; Ethereum 73,685 млрд (здесь DefiLlama режет bridged/child; не смешивать с stables.cool); BSC 9,184 млрд; Solana 2,835 млрд; TON (GRAM) 605,79 млн (−9,69% за месяц, +3,41% за 7д).12(https://stables.cool/USDT)

Стейблкоины всей сети TON (DefiLlama /stablecoins/ton, 1 сентября 2026): $751,21 млн всего (−4,98% за 30д, +3,13% за 7д). Состав: USDT $605,64 млн (доля 80,62%); USDe $145,56 млн (+20,83% за месяц); FDUSD ~$1,1k.13(https://defillama.com/stablecoins/ton)

Tether’s own page usdt.network/blockchains/ton (без даты в шапке, контент живой на момент открытия 1 сентября 2026): supply $606 млн; 11% «savers» / 71% «senders» / 18% «services»; 6 млн estimated on-chain users; 2 млн держат >$1; 100k weekly active; $51,5 млн daily transfer volume; 130k transfers/day; «0% of USD₮ transfers across blockchains»; fee ~$0,0004 до $500; +6% on-chain users в Q2 2026. График market cap на той же странице показывает горб к ~$1,2–1,4 млрд и спад к $606 млн. Это данные эмитента.14(https://usdt.network/blockchains/ton)

Историческая динамика USDT на TON (проверяемые точки, не маркетинг):

- 5 сентября 2024: ~$730 млн supply, ~1,39 млн адресов-держателей (Tonviewer через Cryptopolitan).15(https://www.cryptopolitan.com/1-million-addresses-hold-usdt-on-ton/)
- Tether transparency 26 января 2026: net circulation on Ton $733,8 млн (authorized $1,43 млрд минус unissued).
- Q4 2025 Messari: USDT on TON $762,8 млн.
- Q1 2026 Messari: $555,3 млн (−27,2% QoQ); все стейблы TON $727,7 млн (−24,3% с $961,6 млн). Средний дневной USDT transfer volume $77,0 млн (с $114,1 млн, −32,5%); 73 600 трансферов/день; 31 260 daily active USDT addresses (−1,1%). Средний размер трансфера $1 050 (с $1 580). Velocity 0,11 (с 0,16). Доля TON в кроссчейн USDT volume Q1: 0,13% при 0,30% доли в supply — «held-balance asset, not a payment rail». Крупные DEX-свопы упали, мелкий P2P через @Wallet/Tonkeeper удержался. Два Tether burn: $80 млн 3 января 2026 и $250 млн 17 марта 2026.2(https://messari.io/report/state-of-the-open-network-q1-2026)
- 1 сентября 2026: $605–606 млн.

Вывод по доле: доля TON в мировом USDT не выросла — она стагнирует около 0,3% и в Q1 2026 ставила multi-quarter low. Tron — платежный рельс (30d avg daily USDT transfers $22,3 млрд на конец августа 2026; 400,5 млн аккаунтов; ~$94 млрд USDT). TON на этом фоне — карман внутри Telegram, не конкурент Tron.16(https://blockchain.news/flashnews/tron-surpasses-400-5m-accounts-94-27b-usdt)

Конфликт источников: Tonscan.info 28 августа 2026 показывал USDT supply 1,4 млрд и 3,4 млн holders. Это не бьётся ни с DefiLlama, ни с stables.cool, ни с Tether. Для этого отчёта принимаю Tether+DefiLlama; Tonscan.info по supply помечаю как ненадёжный.

B.4. Люди vs сибилы vs боты

Прямого «человеческого» счётчика нет. Есть три независимых сигнала, и все они бьют в одну сторону.

Сигнал 1 — разрыв activated vs daily. 55 млн activated wallets vs ~100–170k daily actives (TonStat июнь 2026 / DefiLlama сентябрь 2026). Это не «розница живёт в сети»; это «когда-то коснулись».

Сигнал 2 — когортная деградация. Dune TON User Analytics (данные февраль–июль 2025, дашборд обновлялся 26 августа 2026, но таблицы — 2025): все сегменты активности сжимаются месяц к месяцу. 1–2 tx: 976k → 444k; 25+ tx: 163k → 97k. Балансы: 13,7 млн кошельков с 0 TON, 27,4 млн с 0–1 TON. За 30 дней (срез дашборда) top contract categories по unique users: merchant 122 941, gaming 44 944, DEX 38 471, CEX 36 673, tradingbot 5 497. DEX+lending не доминируют.17(https://dune.com/lover_of_lady_chatterley/ton-user-analytics)

Сигнал 3 — новые кошельки без пропорционального DAU. Dune Weekly конец августа 2026: 250–347k new wallets в неделю при 520–697k active wallets. Это совместимо с фармом/сибилами/ретеншн-дырой, не с «каждая активация = новый человек, который остался».

Оценки «реальных людей» в tap-to-earn — анекдот, не метод: инсайдер в марте 2025 (TechFlow) утверждал, что у поздних мини-игр «40% реальных уже хорошо», а среди депозиторов «90% адресов — одна ферма». Это слова одного человека, не воспроизводимый пайплайн. Неподтверждено как цифра; подтверждено как направление (retention после аирдропа убит).

B.5. Прямое доказательство «обычных людей, не знающих DeFi» — есть или нет

Нет прямого доказательства. Есть косвенные куски, которые гипотеза переоценивает.

Что похоже на розницу: мелкий P2P USDT внутри Telegram, средний чек трансфера в Q1 2026 упал до $1 050, при том что count трансферов почти не упал. Messari прямо отделяет «small-value flows from P2P USDT in Telegram via @Wallet and Tonkeeper» от «high-value DeFi swaps». Это единственный кусок, который гипотеза может честно зачесть.2(https://messari.io/report/state-of-the-open-network-q1-2026)

Что объясняет наблюдаемый «рост» 2024 и его исчезновение: Notcoin пик ~35 млн MAU, к апрелю 2026 трекеры ~13,1 млн (потеря ~2/3). Hamster Kombat пик ~300 млн, к 2026 оценки 10–27 млн (потеря >90%). TON DAA с >1 млн в сентябре 2024 к <500k в декабре 2024. TON Foundation сам в 2025 сместил фокус с clicker-игр. Аудитория tap-to-earn в основном ушла; платформа Mini Apps как оболочка осталась, ончейн-слой — нет.18(https://minitap.club/telegram-mini-game-hype-cycle-2026/)

Messari Q1 2026: Telegram-issued products прогнали $88,5 млн через TON за квартал — это consumer-product revenue (Stars/ads/NFT-примитивы внутри Telegram), не депозиты в стейбл-vault. Не путать «деньги Telegram» и «розница кладёт USDT в DeFi».

C. Вопрос 2 — живые стейблкоин-стратегии и реальная доходность

C.1. Инвентарь классов на сентябрь 2026

Ниже APY — заявленные/наблюдаемые на дату, не «ваша чистая прибыль». Где есть и base, и reward — разделяю.

1) Лендинг (Aave и аналоги).
Механика: отдаёшь USDC/USDT в пул, заёмщики платят процент. Доход физически = ставка заёмщика минус резерв протокола.
Сентябрь 2026: Aave V3 Ethereum USDT 1,81% APY при $1,64 млрд TVL; USDC 2,07% при $944,7 млн (stablecoinyield.co, 1 сентября 2026). Maple USDC 4,28% / $3,40 млрд; Maple USDT 3,84% / $2,03 млрд — это уже private credit, не одноранговый пул. Morpho USDC Base 5,39% / $426 млн. Медиана «всего в yield» 4,11% при $23,49 млрд TVL в скринере. 88% yield-bearing стейблов зарабатывают <5% 7D APY (Stablewatch, 1 сентября 2026).19(https://stablecoinyield.co/)
Как потерять: плохой залог + оракул (USDe на Binance 10 октября 2025), curator misconfig, эксплойт, depeg залога. Не «APY обнулился», а «NAV просел за часы».

2) Дельта-нейтральный базис / funding-carry (тип Ethena).
Механика: спот лонг + перп шорт, карман = funding + стейкинг-доход по коллатералу. Не «процент банка».
Сентябрь 2026: sUSDe 7d APY 4,57–4,81%; supplied ~1,34 млрд USDe (Aavescan 1 сентября 2026: 4,81%, 1 341 188 442 USDe). USDe market cap ~$4,1 млрд. Lifetime average на сайте Ethena ~10,6% — это среднее по бычьему 2024, не текущее. 30d APY сентябрь 4,26%; май 2026 supplied было 2,11 млрд, декабрь 2025 — 3,92 млрд: объём стейкинга сдулся. В июне 2026 crypto basis в коллатерале Ethena был уже 1,0% портфеля при −0,1% APY; основная масса ушла в DeFi lending и liquid stables.20(https://aavescan.com/rates/ethena-susde)
Как потерять: отрицательный funding + тонкий CEX-стакан. 10 октября 2025 USDe на Binance торговался ~$0,65 при живом mint/redeem и паритете на других площадках; каскад ликвидаций >$19 млрд. Это не « Ethena сломался», это «оракул Binance = свой стакан». Для розницы отличие академическое: если её залог в USDe на этой бирже — она ликвидирована.

3) Yield-tokenization и петли (Pendle-подобные).
Механика: режут позицию на PT (фикс к дате) и YT (плавающий купон). Петля: PT как залог в Aave/Morpho → ещё стейбл → ещё PT. Доход = roll yield + incentives минус ставка займа.
Pendle core TVL ~$1,15–1,2 млрд на конец августа 2026 (пик 2025 $7–10 млрд). Q1 2026 average TVL $2,57 млрд (−48% QoQ). PT-петли в мае 2026 рекламировались как 20%+ effective на скромном 4–5% underlying — это плечо, не «безрисковый фикс». FIVA на TON — клон идеи, TVL $42k.21(https://coinmagnetic.com/en/defi/pendle)
Как потерять: ставка займа > PT yield, депег underlying, maturity mismatch, incentive cliff.

4) RWA / токенизированные T-bills.
Механика: короткий US Treasury / MMF, токен = доля фонда. Доход = купон векселей минус fee менеджера.
BUIDL (BlackRock/Securitize): RWA.xyz 1 сентября 2026 — total asset value $2,772 млрд, 7D APY 3,44%, 30D APY 3,43%, holders 106, eligible = US Qualified Purchaser. Это не розничный продукт. Tokenized Treasuries рынок ~$15,1–15,6 млрд (май–август 2026). USYC ~$2,8 млрд, USDY ~$2,2 млрд, sUSDS Sky $4,7 млрд @ 3,52% (Stablewatch 1 сентября). NBER w35412: март 2024 $2,4 млрд → май 2026 $15,6 млрд.22(https://app.rwa.xyz/assets/BUIDL)
Как потерять: доступ/KYC, вторичная ликвидность, регуляторный запрет обёртки, не «депег казначейки».

5) LP в стейбл-пулах.
Механика: комиссия свопов ± incentives. Для близких стейблов IL мал, пока нет депега.
Как потерять: депег одной ноги (UST 2022 — учебник), incentive token падает быстрее комиссий.

6) Structured / curator vaults (Morpho, Euler).
Механика: вкладчик кладёт USDC в vault; куратор (Steakhouse, Gauntlet, Sentora, Galaxy, Wintermute/Armitage, kpk, Re7) выбирает рынки и LLTV. Доход = borrow interest ± rewards минус performance fee куратора 5–15%.
Morpho app: Total Deposits ~$10,69 млрд (снимок страницы vaults). Крупные «спокойные» vaults 3,5–5,5% APY при сотнях миллионов TVL (Steakhouse Prime USDC V2 $192 млн @ 4,17%; Sentora RLUSD $215 млн @ 5,33%; Gauntlet USDC Prime $89 млн @ 3,65%). Хвост 50–127% APY сидит в $2–8 млн TVL — это не масштабируемая розничная ставка. Euler: Prime/Sentora 3–8%, хвост 17,8% на $1 млн.23(https://app.morpho.org/vault?network=base&amp%3Bvault=0x5866c602a98b85b83d7bc558e5ca1cd2507aade4)
Как потерять: curator operational risk. Mixbytes (август 2026): в 2025–2026 сотни миллионов bad debt из oracle-misconfig и auto-allocator, который пихает свежий капитал в горящий рынок, потому что «APY подскочил». Аудированный контракт ≠ живой риск.

C.2. Что из этого есть НА TON

Сравнение масштаба, DefiLlama 1 сентября 2026:

Сеть | DeFi TVL | Стейблы
---|---|---
Ethereum | $49,0 млрд | $148,4 млрд
Solana | $5,84 млрд | $15,8 млрд
Base | $5,56 млрд | $5,02 млрд
BSC | $5,43 млрд | $13,9 млрд
Tron | $5,18 млрд | $93,7 млрд
TON | $51,0 млн | $751 млн

TON DeFi ≈ 0,10% Ethereum DeFi и ≈ 1% Base. Это не «тонкая экосистема». Это другая лига.24(https://defillama.com/chains)

Протоколы TON (DefiLlama chain/ton, 1 сентября 2026):

- Tonstakers (liquid staking GRAM, не стейбл): $176,5 млн TVL. «Earn до 199% APY» — маркетинг протокола в X, 25 июня 2026. Не стейблкоин-доход.
- STON.fi DEX: $25,48 млн
- Stakee LST: $24,94 млн
- Hipo LST: $10,97 млн
- EVAA lending: $9,72 млн. Dune: ~28–33k unique users за жизнь протокола. Quantstamp audit «undergoing» в описаниях дашбордов; bug bounty HackenProof. Это не Aave.
- Storm Trade perps: $4,79 млн
- DeDust DEX: $4,58 млн
- Ethena tsUSDe on TON: $3,22 млн — единственный заметный «настоящий» стейбл-yield на сети
- Affluent (vault-of-vaults Mini App): $2,48 млн
- FIVA (Pendle-клон): $42,4 тыс. Сайт обещает «up to 10% USDT fixed» и «до 25x на YT»; DefiLlama TVL это опровергает как рынок. Closed mainnet / Genesis Pass.
- Yield на TON суммарно: $3,35 млн (tsUSDe + SettleTON $83k + FIVA $42k)

Аудиты: публично подтверждённые многократные аудиты уровня Aave/Morpho у EVAA/FIVA в первоисточниках этого прогона не всплыли как завершённый пакет с датами; CertiK Skynet страницы для Storm/Tonstakers существуют как скоринг, не как замена аудита. Пометка: незакрытый audit-trail.

Итог: розница на TON физически не может «вкладываться в стратегии стейбл-дохода» того же класса, что на Ethereum. Ей доступны: держать USDT в Wallet, стейкать GRAM, тонкий EVAA, обёртка tsUSDe на $3 млн.

C.3. Доходность после вычетов

Правило разделения, без которого цифры врут:

- Headline APY на дашборде ≠ base APY ≠ APY после падения reward-токена ≠ realized APY по share price.
- AlphaYields (май 2026): 29 крупных стейбл-vaults, 16 месяцев с января 2025. Equal-weight портфель +7,98% за 16 месяцев; медиана 3-месячной аннуализированной ~6,7%. Восемь vaults >10% annualized — и это уже отбор выживших плюс кредит/экзотика, не «обычный USDC».
- 88% TVL yield-bearing стейблов <5% 7D APY (Stablewatch, 1 сентября 2026). «Безопасный» рынок = T-bill ± 50–150 б.п., не 15–20%.
- Комиссии: L1 Ethereum газ $10–50 на вход/выход ( Morpho-обзоры 2026); Base/TON — центы. Для розницы $500 это съедает месяцы 4% APY.
- Налоги: в общем виде — в большинстве юрисдикций стейкинг/лендинг = taxable event при начислении или при продаже reward-токена. Не проверял конкретную ставку ни по одной стране в этом прогоне.
- Инфляционные награды: Tonstakers 199%, EVAA TGE октябрь 2025 — типичный «APY из эмиссии». GRAM/TON за год к 1 сентября 2026: ~$3,12 → ~$1,35 (−56,7% YoY, MetaMask price page). Награда в падающем токене может дать отрицательный real.

C.4. Исторический стресс: какие «безопасные» стейбл-доходности ломались

- UST/Anchor ~20% (2022): эмиссия, не кэш-флоу. Класс мёртв как «безрисковый».
- 10 октября 2025 («10/10»): >$19 млрд ликвидаций за ~24ч, ~1,6 млн аккаунтов. USDe на Binance до $0,65, WBETH/BNSOL −80% на внутреннем оракуле. Mint/redeem Ethena работал. NY Fed Liberty Street (23 июня 2026) разбирает USDe как системный узел: funding flip, redemption friction, venue-specific depeg. «Стейбл с доходностью» в этот день был залогом, который биржевой оракул переоценил.25(https://libertystreeteconomics.newyorkfed.org/2026/06/synthetic-stablecoins-and-financial-stability/)
- Stream Finance / xUSD, 4 ноября 2025: «external fund manager» потерял ~$93 млн; xUSD −85% за сутки; зацикленный mint с Elixir deUSD. Opaque «delta-neutral / HFT / market making» без публичного аудита позиций.
- HyperVault, 25 сентября 2025: внутренний ledger вместо ERC-4626, rug ~$3,6–6,3 млн. Аудиты «скоро» не подтверждались аудиторами.
- PiggyBank (Solana), 6 июня 2026: «delta-neutral» vault, хедж LAB сломался, NAV USDC vault −15%. ZachXBT: «gambling on blatant scam coins».
- Lazy Summer USDC vaults, 6 июля 2026: манипуляция share price, ~$6,04 млн. Displayed APY на блок подскочил до ~2,08 млн % — артефакт NAV, не доход.
- Balance (алгостейбл), 22 июля 2026: оракул BTC, −99%.
- Euler v1 $197 млн 2023 (вернули); v2 — другой код, короткий track record.

Вывод: ломается не «процент», ломается предположение «NAV=1, пока не доказано иное». Особенно в продуктах, где доход зависит от перпов, оракула одной биржи или «куратора, которому вы доверяете».

C.5. Граница «нажал кнопку» vs «нужен оператор»

Нажал кнопку (розница без DeFi-навыка):
- CEX Earn: Binance USDT Flexible ~1,5% real-time APR + 3% bonus на первые 200 USDT (14 августа 2026) → blended ~1,56% на $10k. Coinbase USDC rewards baseline 4,1%, Coinbase One 4,5%, промо 6,5% на месяц до $500k (23 августа 2026) — это маркетинг поверх GENIUS-дыры, не DeFi. Bitpanda Earn on USDC/EURCV до 7% (июнь 2026), недоступно в AT/UK.
- Telegram Wallet: держать USDT, P2P, (если есть) простой earn внутри Walt — глубина не подтверждена ончейном.
- RWA BUIDL: кнопка есть только у Qualified Purchaser, 106 holders.

Нужен оператор:
- PT-loop на Pendle+Aave, curator choice на Morpho, delta-neutral с хеджем перпов, любой продукт с «до 25x» (FIVA).
- Всё, что требует читать LLTV, oracle source, utilization kink и performance fee.

Граница проходит не по сети, а по тому, видит ли пользователь share price и источник дохода. Если UI говорит «APY» без разложения base/reward/fee — это CEX-обёртка или маркетинг.

C.6. Регуляторный слой к сентябрю 2026

США, GENIUS Act (подписан 18 июля 2025).
- Эмитент payment stablecoin не может платить interest/yield holder’у «за холд».
- Implementing deadline 18 июля 2026 пропущен: есть NPRM, нет финальных правил. OCC NPRM 25 февраля 2026: rebuttable presumption против affiliate-yield (Coinbase–Circle). Treasury NPRM 17 августа 2026, комментарии до середины октября. Effective: earlier of 18 января 2027 или 120 дней после final rules. Запрет DASP продавать нелицензированные payment stablecoins US persons — 18 июля 2028.
- CLARITY Act (черновик 1 мая 2026): банит yield «economically equivalent to bank deposit»; bona fide activity rewards (как кэшбек за траты) оставляют. Сенат ушёл 8 августа без floor vote, возврат 14 сентября 2026.
- Практический статус 1 сентября 2026: эмитент платить не может; биржа/DeFi пока могут; OCC хочет закрыть дыру; финального запрета розничного «earn на USDC» нет. Tokenized deposits банков — легальный обход с FDIC.26(https://crypto.news/us/genius-act/)

ЕС, MiCA.
- Art. 40 (ART) и Art. 50 (EMT): запрет interest эмитентам И CASP, плюс любые benefits «related to the length of time» holding, включая third-party. Это жёстче GENIUS.
- На 1 сентября 2026: 23 EMT-эмитента в реестре ESMA, USDT не в списке; EEA-биржи delist’или USDT spot в 2025. USDC/EURC/EURCV/USDG — compliant EMT.
- Обход, который живёт: токенизированные MMF под MiFID II (Spiko EUTBL, Franklin Templeton BENJI, Amundi) платят yield, потому что это фонд, не EMT. «Wrapper war» (Micahub, май 2026).
- Комиссия ЕС в консультации 2026 (вопрос 20) спрашивает, снимать ли ban — ban пока держится.
- Bitpanda/Bybit EU/Meria предлагают 6–7% (иногда промо 14–20% на короткий фикс) на MiCA-стейблах как CASP lending, не как issuer interest. Юридическая грань тонкая и, судя по Art. 50, спорная; рынок её использует.27(https://www.springlex.eu/en/packages/mica/mica-regulation/article-40/)

Следствие для гипотезы: легально обещать рознице ЕС «доход на стейблкоине» как issuer/CASP interest на EMT — нельзя. Обещать как fund/MMF или как не-EMT (USDT вне ЕС) — серая зона. В США — пока да, через биржу, с риском закрытия дыры в 2026–27. TON Wallet как канал такого продукта в ЕС/США упирается ещё и в Restricted Jurisdictions.

D. Вопрос 3 — «фермеры», как их находить, чем доказывать

D.1. Кто это сегодня

Реальные роли, не фольклор:

- Индивидуальные фермеры: мультиакк, поинты, airdrop, ребаланс vaults. Живут на EVM/Solana. На TON после 2024 их меньше: поинт-ферм не residual.
- Кураторы vaults: Gauntlet, Steakhouse Financial, Sentora, SparkDAO/Sky, Galaxy Curation, Armitage by Wintermute, kpk, Re7, Flowdesk, AlphaPing. Заработок = performance fee 5–15% от yield + иногда токен. AuC у топов — сотни миллионов / миллиарды на Morpho (Total Deposits $10,7 млрд).
- Дельта-нейтральные дески: Ethena-подобные, basis на CEX/HL, funding. После 10/10 часть basis в Ethena сжалась до 1% коллатераля.
- Private credit / Maple: институциональный заёмщик платит 4–8%, вкладчик берёт credit risk.
- Treasury DAO: idle stables → Aave/Morpho/RWA.
- CEX inventory desks: не «фермеры», но они и есть smart money по объёму.

«Человек, который умеет выжимать 15% из USDT без риска» как массовая профессия в 2026 не подтверждается: медиана рынка ~4%, хвост >10% либо мелкий, либо с кредитом/плечом/эмиссией.

D.2. Как проверить результат уликами

Что работает:
- Share price ERC-4626 / vault token vs time (не скрин APY). AlphaYields-метод: realized по цене доли.
- Публичный адрес + полный history: inflows marked (CEX vs bridge vs internal), leverage (borrow events), reward token sales, related wallets (clustering).
- Для перпов: Hyperliquid on-chain fills — PnL воспроизводим. CEX copy-trading leaderboard — нет.

Где врут:
(а) Survivorship: 10 кошельков, показывают один. CMU CHI’24 по TraderWagon/Bybit: лидеры публикуют пачку портфелей; Gold-портфели нижнего квартиля теряют >$15k за 10 дней после попадания в топ; copiers следуют дефолтному рангу, и это не максимизирует прибыль; лидеры могут собирать followers почти без риска.
(б) Эмиссия: APY в токене, который −50% за год.
(в) Чужие деньги + скрытое плечо: PT-loop 5% underlying → 20% «effective».
(г) Один удачный заход / airdrop, далее flat.

Публичные лидерборды с проверяемой историей: DeBank/Zerion (EVM, не TON), Hyperliquid leaderboard (перпы, не стейбл-yield), Morpho vault pages (on-chain deposits/APY, не персональный skill). Честность метрик: vault APY честнее CEX copy; персональный «я 47% годовых на стейблах» без адреса — не доказательство.

D.3. Инструменты на сентябрь 2026

Nansen.
Цена: Free (лимит кредитов) + Pro $49/мес годовой / $69 месяц (сводка сентября 2025, жива в обзорах июня 2026).
Покрытие Smart Money API (docs, обновление ~3–28 августа 2026): Arbitrum, Avalanche, Base, BNB, Ethereum, HyperEVM, IotaEVM, Linea, Mantle, Monad, Optimism, Plasma, Polygon, Robinhood, Sei, Solana, Sonic. TON в списке Smart Money нет.
Token God Mode flows/transfers: в enum chain есть `ton` и `tron`. То есть агрегаты потоков по токену на TON API принимает.
Chain God Mode: TON в таблице сетей есть (см. B.1).
Партнёрство Nansen × TON, 9 января 2025: Query + Growth Dashboards (user activity, tx volumes, ecosystem trends). В пресс-релизе обещаны «Hot Contracts, Smart Alerts, Token God Mode, Wallet Profiler» как следующие шаги. На сентябрь 2026: макро по сети — да; Smart Money labels для TON-адресов в публичном SM API — нет. Партнёрство не равно «Nansen Smart Money на TON». Это опровержение практической части гипотезы (б).28(https://docs.nansen.ai/api/smart-money)

Arkham.
13 мая 2026 (анонс 12 мая) / параллельно фигурировала дата 8 апреля 2026 — Arkham снял TON «из-за user demand and ecosystem importance». Для гипотезы «следить за кошельками на TON как Nansen/Arkham» это прямой стоп: один из двух массовых лейблеров TON выпилил.29(https://x.com/arkham/status/2054175425555787788)

Dune: сырые таблицы TON есть, 335 дашбордов. Бесплатно с лимитами. Smart-money лейблов уровня Nansen нет; всё самому.

DefiLlama: TVL/стейблы/yield, TON покрыт. Бесплатно. Адресов не профилирует.

DeBank / Zerion: EVM (+частично Solana у Zerion). TON — не их дом.

Artemis: макро Ethena/сети. TON — точечно.

Kaito: mindshare/infoFi, не ончейн-атрибуция кошельков. Для поиска «кто пишет про yield» полезен, для PnL — нет.

CryptoQuant: TON dashboard с 9 января 2025 (exchange flows, DEX). Не smart-money labels.

Чего нельзя получить бесплатно: Nansen SM labels, исторический SM holdings, агрегированный «кто умный на TON», Arkham entity graph по TON (снят).

D.4. Критика smart-money подхода — обе стороны

Против (данные, не мнение):
- DeepBlueAlpha (20 мая 2026): симуляции copy-whale в большинстве сценариев не бьют buy-and-hold после latency, slippage, archetype mismatch. Преимущество кита (капитал, инфра, контекст портфеля) не переносится.
- Классика 13F: альфа после раскрытия живёт квартал и сгорает.
- Nansen CEO Svanevik (интервью апрель 2026): лейбл размыт; 3AC и Alameda когда-то были Smart Money; «трудно последовательно применять прибыльную стратегию на разных таймфреймах»; поэтому они делают Smart Money 2.0 как прогноз «кто заработает на следующей неделе», а не «кто заработал в прошлом». Это признание, что ретроспективный лейбл — слабый торговый сигнал.
- Front-running самих лейблов: Nansen redistribution guide прямо запрещает near-real-time SM copy services. Сам факт запрета = рынок это делает.
- CMU CHI’24: copy-trading CEX структурно мотивирует лидеров наигрывать метрики.

За:
- На Hyperliquid копирование аргументируют тем, что PnL публичный, не «отчёт биржи» (ARX, май 2026). Это про перпы, не про стейбл-yield.
- Агрегированные SM netflows как исследовательский слой (куда концентрируется капитал фондов) — не то же самое, что розничное копирование сделок.
- Для поиска фермеров как людей (не для копитрейда) лейблы полезны как фильтр «этот адрес вообще в DeFi», не как «повтори его USDT APY».

Синтез: smart-money как радар контрагентов — рабочий инструмент на EVM/Solana. Smart-money как стратегия копирования стейбл-дохода — не подтверждено. На TON радара почти нет.

D.5. Где эти люди сидят и как знакомиться

Физически/онлайн: Morpho governance и Discord кураторов; Ethena gov forum; Pendle Discord; Ethereum RWA (Securitize, Superstate); конференции типа Permissionless/ETHCC, не TON-only тусовки. На TON: EVAA/STON/Tonstakers TG, это другой социальный слой (мини-аппы, не curator desks).

Холодные личные сообщения в элитных крипто-комнатах — да, антипаттерн, и это не «мнение заказчика», это норма 2025–2026:
- Админы крипто-TG явно пинят: «admins will never DM first» — потому что основной вектор кражи = фейк-админ в личке. Chainfuel, май 2026.
- Web3-практика: mass cold DM в 2025 «reputation killer»; anti-spam в группах жёстче; 5–7 репортов за сутки = restriction аккаунта. CRMChat 2026; Flexe «cold mass-DM tactics die fast in 2026».
- Это не запрет на знакомство. Рабочий путь: публичный вклад (governance comment, risk note, открытый vault), интро через общего человека, короткий reply в треде. Холодная личка «я фармер, давайте поговорим про 20% на USDT» в закрытой комнате читается как прелюдия к скаму.

D.6. Скам-слой 2025–2026 под «гарантированный стейбл-доход»

Типовые схемы и признаки:

1) Opaque managed account / «наш деск арбитрит funding».
Кейс: Stream Finance, ноябрь 2025, $93 млн, xUSD −85%. Признак: не видно позиций, «external manager», APY 18% на «lending arb + HFT», взаимный mint с другим синтетическим долларом.

2) Фейковый vault без share token.
Кейс: HyperVault, сентябрь 2025, внутренний ledger, $3,6 млн+. Признак: «аудиты скоро», аудитор не подтверждает, не ERC-4626, admin keys.

3) «Дельта-нейтраль» с скрытым направленным риском.
Кейс: PiggyBank, июнь 2026, −15% NAV USDC vault. Признак: в документации delta-neutral, в позиции — мид-кап токен; funding «стал irrational» = перевод «хедж не взлетел».

4) Vault с накрученным APY на один блок.
Кейс: Lazy Summer, июль 2026, $6 млн, APY «2 млн %». Признак: APY, который не может существовать экономически.

5) Алгостейбл / оракульный vault «как доллар».
Кейс: Balance, июль 2026, −99%. Признак: пег держится маркетингом, не redemption 1:1 в банки.

Чеклист отличия от живого фермера: публичный vault token; независимый share-price график; названный источник дохода (funding / T-bill / borrow demand); аудит с датой и фирмой, которую можно спросить; нет «гарантии»; нет DM-первым.

E. Вопрос 4 — что гипотеза упускает (адвокат дьявола)

E.1. Почему связка может не сработать / уже опоздала

1) Аудитория Telegram ≠ ончейн TON. Conversion Mini Apps 0,01%. 150 млн «wallet accounts» (экосистемная цифра) не оставляют следа в DAA 90–170k.
2) Стейбл на TON — rest, не rail. Velocity 0,11; 0,13% мирового USDT volume. Люди, которым нужен доллар, уже на Tron.
3) Доходности, которые розница согласится «просто нажать», уже стоят на CEX и в USDC rewards (4%+) / T-bill токенах (3,4%). TON не даёт спреда.
4) Инструментарий smart money TON демонтирован (Arkham) или не доведён до SM (Nansen). Строить «Nansen для TON-фермеров» = строить индексатор с нуля в сеть, где DeFi $51 млн.
5) Регулятор режет именно тот продукт, который гипотеза хочет дать рознице: «доход за холд стейбла». ЕС уже запретил; США запрещают эмитенту и целится в биржу.

Кто пробовал похожее в 2024–2026:
- Tap-to-earn как «онбординг в TON» — онбордил в клейм, не в DeFi. Retention мёртв.
- Ethena на TON (апрель 2025): жив, $146 млн USDe + $3,2 млн tsUSDe. Это не розничный TON-native рынок, это хвостик Ethereum-протокола.
- Affluent Mini App vaults: $2,5 млн. Не product-market fit.
- Telegram Wallet earn/perps: custodial perps исключены в US/UK; это CEX внутри мессенджера, не ончейн-ферма.

E.2. Кто уже сидит в нише (плотность высокая)

- CEX Earn (Binance, Coinbase, OKX, Bybit EU, Bitpanda, Nexo). Для «обычного человека со стейблом» это default.
- Neobank/карты: USDT/USDC cards с кэшбеком (MEXC card 31 августа 2026 — оффшорный обход MiCA, не лицензия ЕС).
- Base + Coinbase: розница US с USDC, Morpho/Aave в один клик из Coinbase Wallet.
- Tron: P2P USDT, не yield.
- EVM-агрегаторы: Yearn, Summer.fi (после эксплойта июля 2026 — с ожогом), vaults.fyi как витрина.
Плотность: на «простой 4% на долларе» мест нет. На «простой 4% на долларе именно в TON» мест полно, потому что спроса с TVL не видно.

E.3. Если розница со стейблом есть, но сеть неверная

Где «обычный человек со стейблкоином» сидит в сентябре 2026:

1) Tron — главный ответ. $91,8 млрд USDT (50,6% мира), 3,88 млн active addresses / 24h (DefiLlama chains), 400,5 млн accounts, $22,3 млрд/день USDT transfers (30d avg, конец августа 2026). CryptoQuant FY2025 TRON: monthly active addresses пик 35,5 млн, декабрь 31,3 млн; <$1k USDT transfers = 60–74% в часы Americas/Asia = P2P/ремиттанс Global South. DeFi TVL Tron $5,2 млрд при $93,7 млрд стейблов: они не фермерят, они платят.24(https://defillama.com/chains)

2) CEX-балансы. Tether Q4 2025: 139,1 млн on-chain USDT holders + оценка >100 млн на централизованных сервисах. «Обычный человек» часто даже не в сети.

3) Ethereum/Base USDC. Ethereum стейблы $148 млрд; Base $5,0 млрд стейблов и $5,56 млрд DeFi — это уже yield-розница Coinbase.

4) Solana: $15,8 млрд стейблов, $5,84 млрд DeFi, 2,27 млн daily active addresses — спекуляция + Jupiter Lend (USDC 3,41%, $522 млн).

5) Локальные P2P (Россия/СНГ/Африка/LATAM): ончейн-слепок = Tron USDT, не TON. TON Wallet в РФ жив как продукт, но zero-fee card buy USDT там выключен; Telegram в РФ в 2026 сторонние карты описывают как повторный throttle — это вторичный источник, не регуляторный первоисточник.

E.4. Что должно быть правдой, чтобы гипотеза сработала (проверяемые условия)

1) Ончейн-конверсия Telegram → TON растёт с 0,01% хотя бы к 1%.
Замер: Mini App DAA / Telegram Mini App MAU, дашборд TON Foundation на Dune + публичные MAU Telegram. Сейчас 13 980 / ~150–190 млн.

2) USDT на TON перестаёт быть «лежащим»: velocity и доля в global transfer volume растут вместе, не только holder count.
Замер: Tether usdt.network (daily volume, weekly active) + Messari/Dune transfer volume share. Сейчас 0,13% volume vs 0,30% supply; daily volume ~$50–80 млн vs Tron ~$22 млрд.

3) На TON появляется стейбл-yield TVL не $3 млн, а хотя бы сотни миллионов в протоколах с публичным share price (не LST GRAM).
Замер: DefiLlama protocols on TON, категории Lending+Yield, exclude liquid staking. Сейчас EVAA $9,7 млн + tsUSDe $3,2 млн + FIVA $0,04 млн.

4) Nansen (или аналог) включает TON в Smart Money, либо независимый лейблер покрывает TON-адреса с проверяемой методикой.
Замер: docs.nansen.ai/api/smart-money supported chains. Сейчас TON нет. Arkham — минус.

5) Регулятор оставляет канал «рознице можно видеть 3–5% на стейбле без лицензии банка» в тех юрисдикциях, где живёт целевая аудитория Telegram.
Замер: GENIUS final rules + OCC presumption; MiCA Art. 50 enforcement vs CASP earn. Сейчас канал сужается, не расширяется.

Если (1) и (2) вдруг станут правдой после раскатки Gram Wallet — гипотеза про розницу оживёт. Гипотеза про «значит следим за smart money и фармерами на TON» всё равно упирается в (3) и (4).

F. Таблица источников

Источник | Что взято | Дата | Надёжность
---|---|---|---
DefiLlama chain/ton, /stablecoins/ton, /chains, /stablecoin/tether | TVL $51.01m, стейблы $751m, USDT $606m, DAA 134k, tx 3.26m, сравнение сетей | 1 сентября 2026 | Ончейн-агрегатор, лучший публичный слой
stables.cool/USDT | USDT по сетям, TON 0,3%, Tron 50,6% | 1 сентября 2026 | Ончейн-дашборд (Tether supplies)
usdt.network/blockchains/ton | $606m, 6m users (оценка Tether), $51.5m daily volume, 100k WAU | открыто 1 сентября 2026 | Эмитент, holders/users — модель Tether, не explorer
Messari State of TON Q1 2026 | DAA 90.8k, tx 1.91m/d, USDT $555m, velocity 0.11, Mini App 0.01%, TVL $56m | отчёт 8 июня 2026, данные Q1 | Независимая аналитика (Blockworks/Messari)
Dune TON Weekly / TON User Analytics | 29.2m tx week 35, 697k wallets, cohort decay 2025 | week 35 2026 / таблицы 2025 | Ончейн, методика кошельков TON Foundation
Nansen API docs (smart-money, tgm/flows) + Chain God Mode UI | SM chains без TON; TGM enum с ton; макро TON 526–570k addresses | август 2026 / снимок UI | Первичка продукта
nansen.ai post партнёрство TON | Query/Growth Dashboards, обещания TGM | 9 января 2025 | Пресс-релиз заинтересованной стороны
Arkham @arkham | Снятие TON 13 мая 2026 | 12 мая 2026 | Первичка продукта
wallet.tg user agreement + help article 240 | Restricted jurisdictions, AML | соглашение + 1 сентября 2026 | Первичка продукта
TON Strategy Co. PR | Gram Wallet limited access 31 августа 2026 | 1 сентября 2026 | Пресс-релиз казначейской компании TON
Decrypt / Defiant / Forbes | Wallet 150m, US self-custody Jul 2025, Durov 21 Jul 2026 | апр–авг 2026 | Журналистика, цифры 150m — от экосистемы
stablecoinyield.co, Stablewatch, vaults.fyi, Aavescan, app.morpho.org, rwa.xyz | APY/TVL Aave, Ethena, Morpho, BUIDL | 1 сентября 2026 | Ончейн-витрины
NY Fed Liberty Street, Galaxy, CoinDesk, CoinGlass via secondary | 10/10 $19B, USDe $0.65 Binance | окт 2025 / разборы 2026 | Независимая аналитика + журналистика
crypto.news GENIUS explainer, OCC/Steptoe, Freshfields, Federal Register | GENIUS dates, yield ban, NPRM | июл 2025 – авг 2026 | Юридические разборы + первичка регулятора
Springlex MiCA Art.40, ESMA EMT list via casptracker | Interest ban, 23 EMT issuers, USDT not listed | 1 сентября 2026 | Юридический текст + реестр
CMU Kawai CHI’24 PDF, DeepBlueAlpha, PANews Svanevik | Copy trading не работает / лейблы ретроспективны | 2024 / 2026 | Академия + симуляция + слова CEO
The Defiant, Quadriga, Mixbytes, Summer.fi post-mortem | Stream, HyperVault, PiggyBank, Lazy Summer | 2025–2026 | Журналистика + постмортемы
CoinEx / CoinLaw / CryptoQuant | Ряды DAW 2023–2026, 155k DAW | 2026 | Вторичная компиляция TonStat; использовать как якорь, не как аудированный ряд
minitap.club / cryptomunies | Notcoin/Hamster retention | май/апр 2026 | Отраслевые трекеры, не ончейн

G. Чего узнать не удалось

- Полный помесячный канонический ряд DAA/tx TON с января 2024 по август 2026 в одном открытом CSV (The Block — пейволл; TonStat не отдаёт историю одним файлом в этом прогоне).
- Независимый (не Tether) count уникальных людей среди 6 млн «USDT on-chain users on TON».
- Доля ботов/сибилов с воспроизводимой методикой на 2026 (есть эвристики 2024–2025, нет peer-reviewed sybil rate TON 2026).
- Живой APY EVAA USDT/USDC на 1 сентября 2026 (TVL есть, ставка пула в открытом виде не считалась с контракта в этом прогоне).
- Полный список стран, где custodial @wallet сейчас green, кроме inverse-списка Restricted. Официальный «where Wallet operates» в help ссылается отдельной статьёй, не раскрытой целиком.
- Факт, включён ли Token God Mode UI Nansen для конкретного jetton USDT на TON с лейблом Smart Money, или API `chain=ton` + `label=smart_money` возвращает пусто. Docs разрешают параметр; SM chain list — нет. Нужен живой API-вызов с ключом.
- Ончейн-эффект Gram Wallet: раскатка 31 августа 2026, на 1 сентября данных нет.
- Юридический статус французского дела Дурова на 1 сентября 2026 точнее, чем «ongoing as of June 2026».
- Покрытие DeBank/Zerion/Kaito по TON на уровне продукта (TGM-эквивалент) — не проверялось логином.
- Точная базовая (не промо) ставка Coinbase USDC rewards и Binance Flexible вне бонусного транша на 1 сентября — есть августовские срезы, не сегодняшний тикер.
- Налоговая квалификация стейбл-yield в РФ/ЕС/США построчно — сознательно оставлена на уровне «в общем виде».
- Подтверждённый первоисточником повторный блок Telegram в России в 2026 (есть карта Mappr, нет файла Роскомнадзора в этом прогоне).
