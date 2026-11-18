# DR26-11-10-HUB-04-1439 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR26-11-10-HUB-04-1439 — ChatGPT Deep research leg

source: https://приватный чат
mode: Deep research chip, pill Extra High; widget_state.status=completed; research_started_at 2026-11-10T16:59:48Z, research_stopped_at 2026-11-10T17:02:13Z (2.5 min: suspiciously short for Deep Research, treat depth with suspicion; see memory chatgpt-dr-mini-counter-lies)
status: completed
collected: 2026-11-10T18:00Z by Claude H97899 via /backend-api/conversation widget_state.report_message -> textarea -> Ctrl+A/Ctrl+C -> Get-Clipboard


---

# Независимая проверка: Web Summit Lisbon, Stage 10 — «Why nuclear compute can’t wait a decade»

## Что можно и нельзя утверждать о самой панели

Панель была запланирована на **10 ноября 2026 года, Stage 10 – Energy Summit**. Официальная страница Web Summit на момент проверки показывала слот **14:10–14:30**, тогда как профиль Takaya Taguchi на Web Summit показывал **14:05–14:25**, а официальный японский анонс Helical Fusion — **14:10–14:35**. То есть даже у первичных источников есть расхождение по продолжительности. Японский анонс Helical Fusion прямо перечисляет трех участников: **Takaya Taguchi, Jake Jurewicz и Andrew Freedman**, тогда как доступный индекс официальной страницы Web Summit в выдаче надежно показывает Jurewicz и Taguchi, но не дает достаточного текста для восстановления роли Freedman в этой конкретной сессии. citeturn561224search0turn990608search1turn561224search3

На момент проверки, около **17:00 по Лиссабону 10 ноября 2026 года**, сессия уже должна была завершиться, но я **не обнаружил публично индексируемую полную запись панели с таймкодами**. Поэтому ниже нет ни одного утверждения вида «Jurewicz сказал на сцене…» или «Taguchi признал…». Все выводы относятся к опубликованным документам компаний, регуляторов, организатора и независимым системным данным, а не к содержанию состоявшегося разговора. citeturn885025time0turn990608search0

Это важно, потому что маркетинговое описание Web Summit уже содержит сильный тезис: AI-load якобы требует «immediate answer» в виде fast-deployable modular nuclear. Это описание — **редакционный framing организатора**, а не доказательство технической реализуемости проекта Blue Energy и не подтверждение того, что такое утверждение прозвучало на сцене. citeturn561224search0

## Спикеры, роли и документированные интересы

| Спикер | Проверенная роль | Первичные факты | Документированный интерес / потенциальный конфликт |
|---|---|---|---|
| **Jake Jurewicz** | Co-Founder & CEO, Blue Energy | Blue Energy указывает MSc in Nuclear Science & Engineering MIT, опыт в energy/climate/infrastructure; компания основана как платформа для ускоренного развертывания nuclear. citeturn156865search1 | Прямая экономическая заинтересованность: CEO и сооснователь компании, чей основной продукт — финансирование и развертывание новых nuclear projects. Blue Energy публично продвигает Victoria и заявляет о private project finance. Компания также перечисляет венчурных инвесторов и стратегическое финансирование. citeturn156865search9turn156865search14 |
| **Takaya Taguchi** | Co-Founder & CEO, Helical Fusion | Helical Fusion подтверждает карьеру в Mizuho, JBIC, PwC, Dai-ichi Life и последующее сооснование Helical Fusion в 2021 году; Taguchi также Vice Chair J-Fusion. citeturn156865search2 | Прямая экономическая и институциональная заинтересованность: руководит компанией, которая собирает капитал и подписывает будущие fusion PPAs; Helical Fusion сообщала примерно о $38 млн совокупного финансирования и PPA с Aoki Super. citeturn220764search2 |
| **Andrew Freedman** | Senior Climate & Weather Reporter, CNN | CNN объявила его назначение в апреле 2025 года; до CNN — Axios, Washington Post и другие climate/science roles. citeturn156865search0 | В проверенных employer/organizer sources **не найден финансовый интерес в Blue Energy, Helical Fusion или nuclear/fusion developers**. Корректно считать его журналистом/модератором по профессии, а не отраслевым инвестором; более сильный вывод был бы спекуляцией. citeturn156865search0turn561224search3 |

Отдельно: Helical Fusion в собственных материалах называет Helical Stellarator подходом с коммерческим потенциалом и делает ряд claim-level формулировок вроде «world’s first commercially viable fusion power plant». Это **company statement**, а не независимый технический факт. citeturn220764search2turn943389search0

## Что реально может дать питание AI-дата-центрам к 2030 году

Самый сильный системный контрпример к тезису «новый nuclear — непосредственный ответ» дает IEA. В свежем анализе по Energy and AI IEA прогнозирует рост глобального потребления дата-центров примерно с **460 TWh в 2024 году до более 1,000 TWh в 2030 году**. При этом **почти половину прироста до 2030 года дают renewables**, за ними идут natural gas и coal; nuclear начинает играть заметно более крупную роль **ближе к концу десятилетия и после него**. IEA отдельно подчеркивает, что считает **физически потребляемую электроэнергию**, а не контрактный PPA mix компаний. citeturn548579search1

Последний LBNL bottom-up forecast для США оценивает потребление дата-центров в **649 TWh в 2030 году** в reference case, с диапазоном compounded uncertainty **521–843 TWh**, то есть примерно **9.5–15.3%** общей электроэнергии США в зависимости от сценария. Это уже не вопрос одного-двух gigawatt campuses, а системный load-growth problem. citeturn548579search2turn548579search8

LBNL также показывает, почему «announced capacity» нельзя приравнивать к «power available». На конец 2025 года в transmission queues находилось около **1,312 GW generation плюс 749 GW storage**; только **13% мощности**, подавшей interconnection requests в 2000–2020 годах, дошло до commercial operation к концу 2025 года. Медианное время от interconnection request до COD для проектов, завершенных в 2025-м, превышало **пять лет**. Даже executed interconnection agreement не равен работающей электростанции: LBNL насчитывает около **549 GW** проектов с draft/executed IA, еще не достигших commercial operation. citeturn548579search5turn548579search6

Поэтому к 2030 году наиболее реалистичный стек выглядит так:

| Источник | Что может реально прийти к 2030 | Ограничение |
|---|---|---|
| **Existing grid generation + transmission upgrades** | Основной объем; физический mix зависит от региона | Grid queues, transformer/transmission lead times, load interconnection |
| **Solar/wind + storage** | Крупнейший новый блок по IEA в aggregate | Нужны transmission, storage/firming и достаточная локальная deliverability |
| **Natural gas** | Один из самых быстрых firm-power вариантов; queue газовых проектов резко вырос | Turbine lead times, gas pipelines, emissions, permits |
| **Existing nuclear / life extension / restart** | Реально сотни MW на проект в 2027–2029 | Plant-specific NRC work и refurbishment |
| **Advanced nuclear new-build** | Возможны отдельные FOAK projects около 2030 | Licensing, construction, fuel, financing, interconnection |
| **Commercial fusion** | Практически нет доказательной базы для meaningful 2030 supply | Еще нет integrated net-electricity power plant |

Этот вывод согласуется и с IEA Electricity 2026: во всем мире более **2,500 GW** generation/storage/large-load projects застряли в grid connection queues, а annual grid investment, по оценке IEA, должен вырасти примерно на **50% к 2030 году**, чтобы соответствовать спросу. citeturn548579search7

Сильный countercase новой nuclear-build стратегии — **перезапуски существующих реакторов**. Palisades уже имеет историческую operating license и проходит возврат из decommissioning status; NRC в 2025 году одобрила ключевые licensing actions для restart, хотя в 2026 году fuel-loading столкнулся с отдельным инцидентом. Crane Clean Energy Center, бывший TMI Unit 1, планируется к restart примерно в 2027–2028 диапазоне; Constellation имеет 20-летний PPA с Microsoft, но электричество будет поступать **в PJM grid**, а Microsoft использует договор для matching своего load, а не как выделенный физический кабель от реактора к серверной. citeturn942253search0turn942253search1turn942253search3turn942253search4

Из новых advanced reactors наиболее осязаемые benchmark-проекты к 2030 году — не Blue Energy, а **OPG Darlington BWRX-300** и **Kairos Hermes 2**. У OPG уже есть канадская Licence to Construct, construction underway, цель — grid connection к концу 2030 года; Licence to Operate еще требуется. Kairos Hermes 2 имеет NRC construction permit, ground was broken в апреле 2026 года, target — до **50 MWe** через TVA grid в 2030 году, с последующим fleet target до 500 MWe к 2035 году для Google. citeturn942253search9turn942253search10turn942253search7turn942253search8

Это и есть важное различие: **2030 nuclear is plausible at the margin, but not yet the dominant incremental source for AI load; 2030s nuclear can matter much more.**

## Blue Energy Victoria: что одобрено NRC, а что нет

Самое существенное искажение, которое стоит исключить из NotebookLM эпизода: **NRC не выдала Blue Energy construction permit для Victoria Unit 1 и тем более operating license**. На текущем официальном NRC page Blue Energy находится в режиме **pre-application / phased application work**. NRC действительно одобрила два topical reports — включая **“Resequencing Balance-of-Plant and Nuclear Island – Construction for Blue Energy Deployments”** и Quality Assurance Program Description. Но topical report approval означает согласование определенного regulatory methodology/approach, а не разрешение строить конкретный реактор. citeturn432502search0turn822075search0

15 сентября 2026 года Blue Energy подала **только Part 1 phased Construction Permit Application**, включая request for a **Limited Work Authorization**. Federal Register notice от 8 октября прямо говорит, что это **receipt** первой части заявки; Blue Energy намерена подать full CPA не позднее чем через 18 месяцев. NRC также прямо указывает, что лишь после определения acceptability for docketing начнется detailed technical review. То есть статус значительно раньше, чем «construction permit approved». citeturn822075search1turn822075search9

Проект Victoria необходимо разделять на два совершенно разных блока:

| Блок | Мощность | Текущий статус | Target |
|---|---:|---|---|
| **Natural gas** | ≈1 GW, две GE Vernova 7HA.02 | Engineering/licensing collaboration; FID еще впереди | 2030 по актуальному Blue Energy schedule |
| **Nuclear** | до ≈1.5 GW, до пяти BWRX-300 | Part 1 CPA/LWA submitted; full CPA еще не подан; permit не выдан | первая nuclear power с 2032 по актуальному schedule |
| **Итого announced** | до ≈2.5 GW | Не installed | phased deployment |

Blue Energy и GE Vernova прямо пишут, что весь проект **subject to final investment decision in 2027**. Societe Generale в сентябре 2026 была нанята **финансовым консультантом** для структурирования и поиска project financing; это не financial close и не committed debt financing. citeturn981527search2turn981527search3

Есть также существенная timeline revision. В октябре 2025 Blue Energy и Crusoe говорили о power delivery **as early as 2028** и expected nuclear generation by **2031**. К августу–сентябрю 2026 Blue Energy уже указывает gas turbines в **2030** и первый SMR в **2032**. Это не провал проекта, но это фактическое смещение публичного графика примерно на два года для bridge gas и на год для nuclear. citeturn981527search0turn981527search2turn981527search3

Отдельно нужно осторожно употреблять слово **offtake**. Публично существует strategic partnership Blue Energy–Crusoe и заявленная nearby AI campus. Но в просмотренных первичных источниках я не нашел раскрытого bankable PPA с price, tenor, minimum take, credit support или termination conditions. Следовательно, корректная формулировка — **announced customer partnership / intended supply relationship**, а не доказанный fully bankable nuclear offtake. citeturn981527search0turn981527search10

И еще одна критическая неопределенность — **physical interconnection**. Публичные материалы говорят о nearby data center и газовой/ядерной генерации, но не дают достаточной доказательной базы, чтобы утверждать, что 1 GW или 1.5 GW уже имеет завершенный ERCOT/load interconnection path, energized substation, approved transmission upgrades и physical deliverability. До появления таких документов gigawatts должны считаться **planned capacity**, не delivered power.

## Helical Fusion: HARUKA не равен net-electric fusion plant

Helical Fusion имеет реальный engineering result, но его масштаб часто легко переинтерпретировать. В 2025 году компания совместно с NIFS испытала double-pancake HTS coil на основе UROCOIC: **40 kA**, external field **7 T**, local field до **8.9 T**, test duration порядка **280 секунд**, без quench в заявленных условиях. В 2026 году результаты были оформлены в peer-reviewed conference proceedings. Это валидный magnet-engineering milestone. citeturn220764search6turn943389search0

Но это **не** интегрированный stellarator, не fusion plasma test, не Q>1, не net electricity и не grid power.

Текущий roadmap Helical Fusion сам это разделяет. **Helix HARUKA** — integrated demonstration device; Phase 1 ориентирована на helical HTS magnet system и имеет energization target в 2027 году. Phase 2 должна интегрировать magnet, blanket/divertor и sustained high-temperature plasma, причем компания прямо пишет: **“No power generation will be conducted in Phase 2.”** citeturn220764search4

**Helix KANATA** — отдельный, более поздний plant concept, который, по плану компании, должен в 2030-х впервые объединить три условия: steady-state operation, net electricity и maintainability. Следовательно, HARUKA нельзя описывать как prototype power plant, уже близкий к grid sales; это engineering/integration demonstrator. citeturn220764search0turn220764search1

Нужно также жестко разделять три разных метрики:

| Метрика | Что означает | Статус Helical Fusion |
|---|---|---|
| **Plasma gain Q** | fusion power / externally injected plasma heating power | Не продемонстрирован Helical Fusion |
| **Engineering/net-electric gain** | электричество на выходе > всей собственной электрической нагрузки facility | Цель KANATA, не продемонстрировано |
| **Grid sales** | лицензированная работа power plant и фактическая поставка энергии в сеть/покупателю | Только будущий коммерческий target |

ITER прямо объясняет, что даже Q=10 относится к fusion heat относительно plasma heating, а не к net electricity всей установки; ITER вообще не проектировался как электростанция. LLNL, несмотря на repeated ignition и target gain >1, тоже подчеркивает, что NIF facility потребляет намного больше энергии, чем производит fusion target. Это хороший антидот к путанице «scientific breakeven = commercial electricity». citeturn644801search4turn644801search1turn644801search2

Для KANATA остаются как минимум четыре больших gap-класса.

**Licensing.** В публичном Helix roadmap, который я проверил, нет названного site-specific operating licence, полного licensing basis или разрешения на коммерческий fusion plant. Это не доказывает, что regulatory work отсутствует, но означает, что public evidence пока не позволяет считать licensing pathway закрытым. citeturn220764search0turn220764search4

**Fuel cycle.** D–T fusion требует tritium handling/breeding. IAEA продолжает рассматривать tritium inventory, permeation, breeding blanket, Li-6 enrichment и closed fuel-cycle validation как незрелые engineering/regulatory areas для будущих fusion plants. citeturn644801search5turn644801search6turn644801search7

**Maintenance/materials.** Нейтронное повреждение, blanket/divertor life, remote maintenance и component replacement остаются основными технологическими вопросами commercial fusion. Именно поэтому Helical Fusion включает maintainability как отдельную из трех required conditions, а не как решенную задачу. citeturn644801search11turn220764search1

**Capital scale.** Helical Fusion сообщала о суммарном funding порядка десятков миллионов долларов, тогда как стоимость KANATA публично не установлена. Нельзя экстраполировать текущий fundraising в финансируемость full commercial plant; evidence для bankable project finance пока отсутствует. citeturn220764search2turn943389search3

PPA с Aoki Super — полезный demand signal, но не доказательство того, что fusion electricity физически будет существовать по конкретной дате. Это будущий contractual commitment, зависящий от возникновения самого generating asset. citeturn220764search2

## Сводный timeline, falsifiers и вывод

| Событие | Факт / план | Дата | Уровень уверенности |
|---|---|---:|---|
| Blue Energy topical report on construction resequencing approved | **Факт** | Dec 2025 / approved version issued 2026 | Высокий; NRC |
| Blue Energy Victoria Part 1 CPA + LWA submitted | **Факт** | 15 Sep 2026 | Высокий; NRC/Federal Register |
| Blue Energy full construction permit | **Не выдан** | Unknown | Высокий |
| Blue Energy FID | **План** | 2027 | Company target |
| Victoria gas ≈1 GW | **План** | 2030 | Company target, subject to FID |
| Victoria first nuclear BWRX-300 | **План** | 2032 | Company target |
| OPG Darlington first BWRX-300 | **Строится** | grid target end-2030 | Более зрелый comparator |
| Kairos Hermes 2 | **Строится / permitted** | target 2030 | 50 MWe |
| Palisades restart | **Restart work underway** | ~2027 range | Existing licensed plant |
| Crane/TMI-1 restart | **Refurbishment/licensing underway** | 2027–2028 target | Existing plant |
| Helical UROCOIC coil test | **Факт** | 2025, paper 2026 | Engineering component test |
| HARUKA Phase 1 energization | **План** | 2027 | Company target |
| HARUKA integrated Phase 2 | **План** | around 2030 / 2030s wording varies | No electricity generation |
| KANATA net-electric plant | **План** | 2030s | High technical/schedule uncertainty |
| Commercial fusion contribution to AI load by 2030 | **Нет доказанной поставки** | 2030 | Очень низкая вероятность material scale |

Наиболее полезные **фальсификаторы** для будущего NotebookLM эпизода — события, после которых текущий скептический вывод нужно будет пересмотреть:

| Фальсификатор | Что изменит оценку |
|---|---|
| NRC выдает Blue Energy полноценный construction permit, а затем operating authorization с графиком, совместимым с 2032 | Существенно повышает credible nuclear timeline Victoria |
| Blue Energy достигает 2027 FID/financial close и раскрывает binding offtake + EPC/turbine/reactor supply commitments | Переводит Victoria из advanced development в financed execution |
| ERCOT/utility documents подтверждают local load interconnection и deliverability полного 1 GW gas load к 2030 | Убирает главный physical-delivery gap |
| HARUKA демонстрирует integrated long-duration fusion plasma + blanket/divertor performance в заявленные сроки, а KANATA получает site/licensing/funding basis | Делает 2030s fusion schedule существенно более credible |
| KANATA либо другой stellarator реально показывает **net electricity at plant boundary**, а не только plasma Q | Это главный технический falsifier тезиса «fusion слишком далека от commercial power» |

**Сильнейший countercase** к риторике «nuclear compute cannot wait a decade» следующий: AI действительно не может ждать десять лет, но именно поэтому большая часть электроэнергии к 2030 году, согласно IEA и LBNL, будет приходить из **существующей сети, renewables, storage, gas и модернизации/перезапуска уже построенной nuclear**, а не из FOAK nuclear или fusion. New nuclear имеет реальную стратегическую ценность для 2030-х и далее и отдельные проекты могут успеть к 2030, но system-level evidence не подтверждает, что он станет главным ответом на near-term AI load. citeturn548579search1turn548579search5turn942253search8turn942253search10

Для Blue Energy наиболее точная формулировка сегодня: **Victoria — интересный, но еще не разрешенный и не профинансированный 2.5-GW gas-plus-nuclear development project; 1 GW gas — план на 2030, до 1.5 GW nuclear — план начиная с 2032, installed capacity сейчас равна нулю.** NRC approval topical report нельзя описывать как construction approval. citeturn822075search0turn822075search1turn981527search3

Для Helical Fusion: **реальный результат — validated HTS coil technology и переход к HARUKA hardware; HARUKA не будет генерировать электроэнергию в Phase 2; net-electricity, maintainable commercial stellarator остается задачей KANATA в 2030-х.** Любая формулировка, превращающая 40-kA coil test в доказательство близости grid fusion, будет технически неверной. citeturn220764search4turn943389search0turn220764search1

### Проверенные URL и где именно они полезны

| Источник | Дата / раздел | URL |
|---|---|---|
| Web Summit session | 10 Nov 2026; session page | https://websummit.com/sessions/lis26/e6999a45-6198-4071-8cd3-73923d061aa3/why-nuclear-compute-can-t-wait-a-decade/ |
| Web Summit — Taguchi | 2026 attendee profile | https://websummit.com/attendees/lis26/35353f23-78f6-4d41-87c5-2f6e208e9963/takaya-taguchi/ |
| Helical Fusion Japanese event notice | 2026; 登壇セッション情報 | https://www.helicalfusion.com/post/%E3%80%8Cweb-summit-2026%E3%80%8D%E3%81%AB-ceo-%E7%94%B0%E5%8F%A3%E3%81%8C%E7%99%BB%E5%A3%87%E3%81%97%E3%81%BE%E3%81%99 |
| Blue Energy / NRC | current; Pre-Application Activities | https://www.nrc.gov/facilities-safety/new-reactors/advanced-reactors/who-were-working-with/pre-application-activities/blue-energy |
| Federal Register Victoria CPA/LWA | 8 Oct 2026; Discussion | https://public-inspection.federalregister.gov/2026-20678.pdf |
| Blue Energy GE Vernova agreement | 13 Aug 2026 | https://www.blueenergy.co/news/project-acceleration-agreement-with-ge-vernova-hitachi |
| Blue Energy Societe Generale | 1 Sep 2026 | https://www.blueenergy.co/news/socgen-financing-advisor-release |
| Crusoe–Blue Energy Victoria | 2025 partnership announcement | https://www.crusoe.ai/resources/newsroom/blue-energy-and-crusoe-partner-to-develop-nuclear-powered-ai-data-center |
| Port of Victoria | 14 Oct 2025 | https://www.portofvictoria.com/news-and-media/p/item/65061/victoria-navigation-district-partners-with-blue-energy-to-advance-small-modular-nuclear-power-project |
| Helical Fusion Helix Program | current roadmap | https://www.helicalfusion.com/helixprogram?lang=en |
| HARUKA construction/phases | Phase 1 / Phase 2 | https://global.helicalfusion.com/post/helical-fusion-announces-construction-site-for-phase-1-of-helixharuka-marking-transition-to-manuf |
| Helical HTS test | 27 Oct 2025 | https://global.helicalfusion.com/post/helical-fusion-achieves-milestone-toward-commercial-fusion-energy-advancing-to-integrated-demonstra |
| UROCOIC peer-reviewed paper announcement | Aug 2026 | https://global.helicalfusion.com/post/helical-fusion-publishes-peer-reviewed-paper-on-hts-magnet-technology-validated-through-collaboratio |
| Peer-reviewed earlier conductor paper | 2025 | https://www.jspf.or.jp/PFR/PDF2025/pfr2025_20-1405043.pdf |
| IEA Energy and AI | Energy supply for AI | https://www.iea.org/reports/energy-and-ai/energy-supply-for-ai/ |
| IEA Key Questions on Energy and AI | 2026; executive summary | https://www.iea.org/reports/key-questions-on-energy-and-ai/executive-summary |
| LBNL US Data Center Energy Usage | 2026 | https://datacenters.lbl.gov/publications/united-states-data-center-energy-2025 |
| LBNL Queued Up | 2026 edition | https://emp.lbl.gov/queues |
| LBNL Speed to Power | Jun 2026 | https://datacenters.lbl.gov/publications/speed-power-solutions-accelerating |
| OPG Darlington SMR | current project status | https://www.opg.com/projects-services/projects/nuclear/smr/darlington-smr/ |
| Kairos Hermes 2 | project status | https://www.kairospower.com/iterative-development |
| Kairos–Google | fleet agreement | https://www.kairospower.com/google |
| NRC Palisades | current restart status | https://www.nrc.gov/facilities-safety/facility-finder/reactors/pali |
| Constellation Crane/Microsoft PPA | 20 Sep 2024 | https://www.constellationenergy.com/news/2024/Constellation-to-Launch-Crane-Clean-Energy-Center-Restoring-Jobs-and-Carbon-Free-Power-to-The-Grid.html |
| ITER Q FAQ | plasma gain vs engineering breakeven | https://www.iter.org/faqs?thematic=68 |
| LLNL NIF ignition | target gain / facility energy | https://lmf.llnl.gov/science/achieving-fusion-ignition |
| IAEA fusion technology gaps | fuel cycle/licensing/maintenance | https://conferences.iaea.org/event/417/program |

Главная нерешенная доказательная лакуна на сегодня — **отсутствие доступной публичной записи Stage 10 с таймкодами**. Пока она не появится, любые прямые атрибуции тезисов Jurewicz, Taguchi или Freedman именно этой панели следует считать неподтвержденными.

## SAFE_URLS (report_message.metadata.safe_urls)
https://cnnpressroom.blogs.cnn.com/2025/04/21/andrew-freedman-joins-cnn-as-a-senior-reporter/amp/
https://cnnpressroom.blogs.cnn.com/2025/04/21/andrew-freedman-joins-cnn-as-a-senior-reporter/amp/?utm_source=chatgpt.com
https://conferences.iaea.org/event/385/timetable/?print=1&view=standard_numbered
https://conferences.iaea.org/event/385/timetable/?print=1&view=standard_numbered&utm_source=chatgpt.com
https://conferences.iaea.org/event/417/program
https://conferences.iaea.org/event/417/program?utm_source=chatgpt.com
https://conferences.iaea.org/event/425/contributions/37849/
https://conferences.iaea.org/event/425/contributions/37849/?utm_source=chatgpt.com
https://datacenters.lbl.gov/publications/united-states-data-center-energy-2025
https://datacenters.lbl.gov/publications/united-states-data-center-energy-2025?utm_source=chatgpt.com
https://emp.lbl.gov/publications/queued-2026-edition-characteristics?trk=public_post_comment-text
https://emp.lbl.gov/publications/queued-2026-edition-characteristics?trk=public_post_comment-text&utm_source=chatgpt.com
https://emp.lbl.gov/queues
https://emp.lbl.gov/queues?utm_source=chatgpt.com
https://global.helicalfusion.com/post/helical-fusion-announces-construction-site-for-phase-1-of-helixharuka-marking-transition-to-manuf
https://global.helicalfusion.com/post/helical-fusion-announces-construction-site-for-phase-1-of-helixharuka-marking-transition-to-manuf?utm_source=chatgpt.com
https://global.helicalfusion.com/post/helical-fusion-publishes-peer-reviewed-paper-on-hts-magnet-technology-validated-through-collaboratio
https://global.helicalfusion.com/post/helical-fusion-publishes-peer-reviewed-paper-on-hts-magnet-technology-validated-through-collaboratio?utm_source=chatgpt.com
https://global.helicalfusion.com/post/helical-fusion-signs-japan-s-first-power-purchase-agreement-for-fusion-energy-with-aoki-super
https://global.helicalfusion.com/post/helical-fusion-signs-japan-s-first-power-purchase-agreement-for-fusion-energy-with-aoki-super?utm_source=chatgpt.com
https://global.helicalfusion.com/team
https://global.helicalfusion.com/team?utm_source=chatgpt.com
https://investors.constellationenergy.com/news-releases/news-release-details/constellation-reports-second-quarter-2026-results
https://investors.constellationenergy.com/news-releases/news-release-details/constellation-reports-second-quarter-2026-results?utm_source=chatgpt.com
https://lmf.llnl.gov/science/energy-security
https://lmf.llnl.gov/science/energy-security?utm_source=chatgpt.com
https://public-inspection.federalregister.gov/2026-20678.pdf
https://public-inspection.federalregister.gov/2026-20678.pdf?utm_source=chatgpt.com
https://thefederalregister.org/documents/2026-20678/blue-energy-global-inc-blue-energy-center-port-of-victoria-nuclear-unit-1-phased-construction-permit-application-limited
https://thefederalregister.org/documents/2026-20678/blue-energy-global-inc-blue-energy-center-port-of-victoria-nuclear-unit-1-phased-construction-permit-application-limited?utm_source=chatgpt.com
https://websummit.com/attendees/lis26/35353f23-78f6-4d41-87c5-2f6e208e9963/takaya-taguchi/
https://websummit.com/attendees/lis26/35353f23-78f6-4d41-87c5-2f6e208e9963/takaya-taguchi/?utm_source=chatgpt.com
https://websummit.com/sessions/lis26/e6999a45-6198-4071-8cd3-73923d061aa3/why-nuclear-compute-can-t-wait-a-decade/
https://websummit.com/sessions/lis26/e6999a45-6198-4071-8cd3-73923d061aa3/why-nuclear-compute-can-t-wait-a-decade/?utm_source=chatgpt.com
https://www-pub.iaea.org/MTCD/Publications/PDF/TE-2047web.pdf
https://www-pub.iaea.org/MTCD/Publications/PDF/TE-2047web.pdf?utm_source=chatgpt.com
https://www.ans.org/news/2025-10-29/article-7500/helical-fusion-marks-milestone-in-progress-toward-fusion-power/
https://www.ans.org/news/2025-10-29/article-7500/helical-fusion-marks-milestone-in-progress-toward-fusion-power/?utm_source=chatgpt.com
https://www.blueenergy.co/
https://www.blueenergy.co/?utm_source=chatgpt.com
https://www.blueenergy.co/about
https://www.blueenergy.co/about?utm_source=chatgpt.com
https://www.blueenergy.co/news/blue-energy-receives-strategic-investment-from-constellation-to-accelerate-commercialization-of-novel-shipyard-manufacturing-and-project-financing-model-for-new-nuclear
https://www.blueenergy.co/news/blue-energy-receives-strategic-investment-from-constellation-to-accelerate-commercialization-of-novel-shipyard-manufacturing-and-project-financing-model-for-new-nuclear?utm_source=chatgpt.com
https://www.blueenergy.co/news/blue-energy-submits-construction-permit-application-to-nuclear-regulatory-commission
https://www.blueenergy.co/news/blue-energy-submits-construction-permit-application-to-nuclear-regulatory-commission?utm_source=chatgpt.com
https://www.blueenergy.co/news/project-acceleration-agreement-with-ge-vernova-hitachi
https://www.blueenergy.co/news/project-acceleration-agreement-with-ge-vernova-hitachi?utm_source=chatgpt.com
https://www.blueenergy.co/news/socgen-financing-advisor-release
https://www.blueenergy.co/news/socgen-financing-advisor-release?utm_source=chatgpt.com
https://www.constellationenergy.com/news/2024/Constellation-to-Launch-Crane-Clean-Energy-Center-Restoring-Jobs-and-Carbon-Free-Power-to-The-Grid.html
https://www.constellationenergy.com/news/2024/Constellation-to-Launch-Crane-Clean-Energy-Center-Restoring-Jobs-and-Carbon-Free-Power-to-The-Grid.html?utm_source=chatgpt.com
https://www.crusoe.ai/resources/newsroom/blue-energy-and-crusoe-partner-to-develop-nuclear-powered-ai-data-center
https://www.crusoe.ai/resources/newsroom/blue-energy-and-crusoe-partner-to-develop-nuclear-powered-ai-data-center?utm_source=chatgpt.com
https://www.energy.gov/documents/united-states-data-center-energy-usage-report-2025-update
https://www.energy.gov/documents/united-states-data-center-energy-usage-report-2025-update?utm_source=chatgpt.com
https://www.helicalfusion.com/helixprogram
https://www.helicalfusion.com/helixprogram?lang=en
https://www.helicalfusion.com/helixprogram?lang=en&utm_source=chatgpt.com
https://www.helicalfusion.com/helixprogram?utm_source=chatgpt.com
https://www.helicalfusion.com/post/%E3%80%8Cweb-summit-2026%E3%80%8D%E3%81%AB-ceo-%E7%94%B0%E5%8F%A3%E3%81%8C%E7%99%BB%E5%A3%87%E3%81%97%E3%81%BE%E3%81%99
https://www.helicalfusion.com/post/%E3%80%8Cweb-summit-2026%E3%80%8D%E3%81%AB-ceo-%E7%94%B0%E5%8F%A3%E3%81%8C%E7%99%BB%E5%A3%87%E3%81%97%E3%81%BE%E3%81%99?utm_source=chatgpt.com
https://www.helicalfusion.com/post/helical-fusion%E3%80%81%E9%AB%98%E6%B8%A9%E8%B6%85%E4%BC%9D%E5%B0%8E%E3%83%9E%E3%82%B0%E3%83%8D%E3%83%83%E3%83%88%E6%8A%80%E8%A1%93%E3%81%AE%E9%87%8D%E8%A6%81%E3%83%9E%E3%82%A4%E3%83%AB%E3%82%B9%E3%83%88%E3%83%BC%E3%83%B3%E3%81%AB%E9%96%A2%E3%81%99%E3%82%8B%E5%85%B1%E5%90%8C%E7%A0%94%E7%A9%B6%E6%88%90%E6%9E%9C%E3%81%8C%E5%9B%BD%E9%9A%9B%E5%AD%A6%E8%A1%93%E8%AA%8C%E3%81%AB%E6%8E%B2%E8%BC%89
https://www.helicalfusion.com/post/helical-fusion%E3%80%81%E9%AB%98%E6%B8%A9%E8%B6%85%E4%BC%9D%E5%B0%8E%E3%83%9E%E3%82%B0%E3%83%8D%E3%83%83%E3%83%88%E6%8A%80%E8%A1%93%E3%81%AE%E9%87%8D%E8%A6%81%E3%83%9E%E3%82%A4%E3%83%AB%E3%82%B9%E3%83%88%E3%83%BC%E3%83%B3%E3%81%AB%E9%96%A2%E3%81%99%E3%82%8B%E5%85%B1%E5%90%8C%E7%A0%94%E7%A9%B6%E6%88%90%E6%9E%9C%E3%81%8C%E5%9B%BD%E9%9A%9B%E5%AD%A6%E8%A1%93%E8%AA%8C%E3%81%AB%E6%8E%B2%E8%BC%89?utm_source=chatgpt.com
https://www.iea.org/reports/electricity-2026/executive-summary?__cf_chl_tk=RepB83VE.Ub7EcMYblkRHf.tt1Tx1J36WbEpyY69MEs-1793285975-адрес узла-kIUq0gheB1tgfaT4Acqb09vtOzCHaBnEQJODQunW.UY
https://www.iea.org/reports/electricity-2026/executive-summary?__cf_chl_tk=RepB83VE.Ub7EcMYblkRHf.tt1Tx1J36WbEpyY69MEs-1793285975-адрес узла-kIUq0gheB1tgfaT4Acqb09vtOzCHaBnEQJODQunW.UY&utm_source=chatgpt.com
https://www.iea.org/reports/energy-and-ai/energy-supply-for-ai/
https://www.iea.org/reports/energy-and-ai/energy-supply-for-ai/?utm_source=chatgpt.com
https://www.iter.org/faqs?thematic=68
https://www.iter.org/faqs?thematic=68&utm_source=chatgpt.com
https://www.kairospower.com/google
https://www.kairospower.com/google?utm_source=chatgpt.com
https://www.kairospower.com/updates/kairos-power-breaks-ground-on-hermes-2-demonstration-plant
https://www.kairospower.com/updates/kairos-power-breaks-ground-on-hermes-2-demonstration-plant?utm_source=chatgpt.com
https://www.llnl.gov/article/50801/llnls-breakthrough-ignition-experiment-highlighted-physical-review-letters
https://www.llnl.gov/article/50801/llnls-breakthrough-ignition-experiment-highlighted-physical-review-letters?utm_source=chatgpt.com
https://www.nrc.gov/about-nrc/news-releases/2025/nrc-approves-major-licensing-actions-required-restart-palisades-npp
https://www.nrc.gov/about-nrc/news-releases/2025/nrc-approves-major-licensing-actions-required-restart-palisades-npp?utm_source=chatgpt.com
https://www.nrc.gov/facilities-safety/facility-finder/reactors/pali
https://www.nrc.gov/facilities-safety/facility-finder/reactors/pali?utm_source=chatgpt.com
https://www.nrc.gov/facilities-safety/new-reactors/advanced-reactors/who-were-working-with/pre-application-activities/blue-energy
https://www.nrc.gov/facilities-safety/new-reactors/advanced-reactors/who-were-working-with/pre-application-activities/blue-energy?utm_source=chatgpt.com
https://www.opg.com/power-generation/our-power/nuclear/operating-licences/
https://www.opg.com/power-generation/our-power/nuclear/operating-licences/?utm_source=chatgpt.com
https://www.opg.com/projects-services/projects/nuclear/smr/darlington-smr/
https://www.opg.com/projects-services/projects/nuclear/smr/darlington-smr/?utm_source=chatgpt.com
