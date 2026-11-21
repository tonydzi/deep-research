---
dr_id: DR26-11-15-MACSTUDIOM4-01-1401
title: "Синтез ДР: перелив аудитории между TikTok, Instagram, Threads и X"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Синтез ДР: перелив аудитории между TikTok, Instagram, Threads и X

Вошли четыре ноги: ChatGPT, Claude.ai, Grok, Mistral. Gemini на момент синтеза не готов, кворум 4 из 5. Основа: исходное задание `dr-prompt.md` и предварительная проверка Codex `codex-review.md`. Сокращения ног в таблицах: **G** ChatGPT, **C** Claude.ai, **K** Grok, **M** Mistral.

## 1. Короткий вывод простыми словами

1. **Точно известно:** ни одна платформа не обещает бонус за то, что вы приводите к ней людей из другой сети. Задокументированные «штрафы» узкие: Instagram не рекомендует ролики с чужим водяным знаком и аккаунты, которые в основном репостят чужое; TikTok не рекомендует чужие перезаливки; X блокирует опасные и спамные ссылки. Согласны все четыре ноги.
2. **Самый сильный встроенный мост:** Meta сама показывает посты Threads пользователям Instagram и Facebook, даёт кросспост из Instagram в Threads и значок Threads в профиле Instagram. Это механизм показа, а не доказанный рост подписчиков.
3. **Главный спор:** есть ли сейчас «штраф за ссылку» на X. Mistral считает подавление доказанным, Grok считает его опровергнутым, ChatGPT и Claude ставят «спорно». Buffer видит нулевую медиану реакций у ссылочных постов обычных аккаунтов, а Маск и Бир в июле 2026 заявили, что штрафа нет больше года. Чистого A/B нет ни у кого.
4. **Ловушка измерения:** в Threads общие клики по ссылкам из Insights убраны (Engadget, 08.09.2026, проверено мной); клики видны только в Podcast Insights. Две ноги из четырёх (ChatGPT, Mistral) этого не знают и советуют смотреть клики в Insights. Считать переходы из Threads придётся своими UTM-ссылками.
5. **Агентских кейсов о переливе именно между этими четырьмя сетями с цифрами переходов нет.** Все четыре ноги пришли к этому независимо; громкие кейсы показывают просмотры, а не перетекание людей.
6. **Проверять первым:** Instagram → Threads (кросспост, значок, Stories), ссылка в теле поста против ссылки в ответе на X и в Threads, чистая перезаливка своих роликов TikTok ↔ Reels без водяного знака.

## 2. Бустят и штрафуют: реестр по платформам

Уровни: **О** официальное правило, **З** независимый или крупный замер, **К** кейс агентства, **Г** гипотеза, **Х** опровергнуто, **Н** данных недостаточно.

| Платформа | Механизм | Уровень | Источник | Ноги согласны |
|---|---|---|---|---|
| Instagram | Ролики и посты с видимым водяным знаком не допускаются в рекомендации (не удаляются) | О, ✅ проверено мной | https://about.fb.com/br/news/2024/04/ajudando-o-criador-de-conteudo-a-encontrar-novos-publicos/ | G C K M |
| Instagram | Свой логотип не вредит, мешает логотип другого приложения | О (слова Моссери) | https://www.socialmediatoday.com/news/instagram-clarifies-including-your-own-logo-on-a-reel-is-ok/730852/ | C K |
| Instagram | Агрегатор: 10+ чужих публикаций за 30 дней без переработки → вне рекомендаций на 30 дней | О, ✅ проверено мной | тот же URL Meta 2024 | G C M; K без порога |
| Instagram | С 30.04.2026 правило агрегаторов распространено на фото и карусели; 75% рекомендаций в США оригинальные (цифра Instagram) | О через прессу | https://www.mediapost.com/publications/article/414773/instagram-algorithm-update-discourages-reposted-co.html | C K; G даёт 75% по Meta, январь 2026 |
| Instagram | «Оригинал получает +40-60% охвата» | Н | блоги, Instagram цифру не публиковал | разобрал только C |
| Instagram | Кликабельные ссылки в подписях доступны всем | Х | https://www.engadget.com/social-media/meta-is-testing-clickable-links-in-instagram-captions-for-verified-subscribers-184555406.html (тест для Meta Verified, 12.03.2026) | C M; G осторожно |
| Instagram | «Волна банов за ссылки с 29.09.2026» | Н (только сообщения сервисов ссылок) | вторичные, Meta не подтверждал | M |
| Threads | Посты Threads рекомендуются в Instagram и Facebook, есть выключатель | О | https://about.fb.com/news/2025/03/new-threads-features-more-personalized-experience-you-control/ | G C K M |
| Threads | Эта рекомендация растит подписки Threads | Н | публичных замеров нет | G C K M |
| Threads | Ссылки намеренно понижаются | Х по словам платформы | https://www.socialmediatoday.com/news/meta-says-link-posts-ranked-properly-threads-reach/750126/ | C M; G «данных нет» |
| Threads | Ссылки с мая 2025 показываются в рекомендациях чаще | заявление платформы | тот же URL SMT; https://www.engadget.com/social-media/threads-is-finally-embracing-links-150012499.html | M; C цитирует Моссери |
| Threads | Переходов из Threads мало (Chartbeat: менее 1% реферального трафика издателей; у отдельных изданий рост в разы) | З | https://www.engadget.com/social-media/threads-users-still-barely-click-links-170139103.html ; https://digiday.com/media/traffic-referrals-from-threads-surge-for-some-publishers-this-year/ | C M |
| Threads | Клики по ссылкам убраны из Insights; клики остались в Podcast Insights | О через прессу, ✅ проверено мной | https://www.engadget.com/2251592/threads-is-overhauling-its-in-app-analytics-with-more-details-except-for-link-clicks/ ; https://about.fb.com/news/2026/09/threads-expands-podcast-toolkit-for-creators-and-listeners/ | C K; **G M против** (раздел 5) |
| TikTok | Перезаливка без творческих правок, в т.ч. с чужим водяным знаком, не идёт в «Для вас» | О | https://www.tiktok.com/community-guidelines/en/integrity-authenticity ; https://newsroom.tiktok.com/en-us/how-tiktok-recommends-videos-for-you | C K M; G такого правила не нашёл |
| TikTok | Призыв «подпишись в Instagram» штрафуется | Н | в официальном объяснении ранжирования такого нет: https://support.tiktok.com/en/using-tiktok/exploring-videos/how-tiktok-recommends-content | G C K M |
| TikTok | Кнопки Instagram и YouTube в профиле удалены (ноябрь 2025), осталось одно поле «Сайт» | вторичный разбор | https://socialday.live/features/tiktok-removes-instagram-and-youtube-profile-links-forcing-creators-onto | C M; G описывает ссылку на соцсеть как доступную |
| TikTok | Ссылка в профиле: от 1 000 подписчиков или бизнес-аккаунт | О (через вторичные цитаты) | https://in.bio/blog/tiktok-link-in-bio-requirements | G C M |
| TikTok | Destination Links: кликабельная ссылка у органического видео, только Verified Business с пикселем | О, ✅ проверено мной | https://ads.tiktok.com/resources/help/article/about-destination-links | только G |
| TikTok | Сентябрьские правила 2026 о поведении вне платформы вводят бан за кросс-промо | Н, скорее нет | https://socialday.live/features/tiktoks-september-community-guidelines-formalise-off-platform-account-bans | M |
| X | Блокировка и предупреждение для вредоносных, фишинговых, спамных URL | О | https://help.x.com/en/safety-and-security/phishing-spam-and-malware-links | G C K M |
| X | Ссылочные посты обычных аккаунтов: медиана вовлечённости 0% после марта 2025 | З (наблюдение, не A/B), ✅ проверено мной | https://buffer.com/resources/links-on-x/ | G C M; K цитирует другие цифры |
| X | Один тест октября 2024: тот же пост со ссылкой 3 670 просмотров против 65 400 без (около −94%) | З низкого качества (одна пара постов) | https://ppc.land/how-xs-algorithm-silently-kills-your-links-without-explicitly-penalizing-them/ | M; C называет 94% без методики |
| X | «Ссылки не понижаются больше года» | заявление платформы, ✅ проверено мной | https://ppc.land/x-drops-year-old-link-penalty-musk-tells-zuckerberg-on-platform/ | C K M |
| X | Ссылка в ответе обходит штраф | Г | Buffer советует без теста; Бир 28.07.2026: ставить ссылки в ответы больше не нужно | G C K M |
| X | Premium поднимает медианный охват в 6-10 раз | З (корреляция, возможен отбор) | https://buffer.com/resources/x-premium-review/ | M; C, G упоминают разрыв по Premium у ссылок |
| X | X замедлял переходы на Threads, Instagram (2,5-5 секунд) | З, 2023 год; сейчас не проверено | Washington Post 15.08.2023, The Verge 15.09.2023 (по данным M) | M |
| X | Hootsuite доказал штраф за ссылки | Х как современное доказательство | https://blog.hootsuite.com/linkless-tweets-experiment/ (269 твитов, 2020-2021, часть платных) | G C K M |
| все | Кросспост своего чистого ролика не роняет оригинал, копии добавляют просмотры (+41% медиана) | З низкого качества (данные продавца инструмента) | https://postonce.to/crossposting-statistics | только K; M приводит малые самотесты против (3 из 10 видео не хуже исходника) |
| все | Входящий трафик из другой сети улучшает ранжирование у получателя | Г / Н | ни одна платформа этого не утверждает | G C K M |
| все | Высокий охват сам даёт подписки | Х по кейсам | Digital Minds: 1,1 млн просмотров → 2,5 тыс. подписчиков; Zenfulnote: около 0,08% кликов | C M |

## 3. Матрица 12 маршрутов

«Шаги» = касаний до экрана подписки. Уверенность: первая буква про механику, вторая про эффект (В/С/Н).

| Маршрут | Способ | Главный риск | Уверенность |
|---|---|---|---|
| TikTok → Instagram | поле «Сайт» (с 1 000 подписчиков или бизнес), устный призыв, Destination Link у бизнес-аккаунта; параллельно чистые Reels | 4-6 шагов; ролик с водяным знаком TikTok выпадает из рекомендаций IG | В / Н |
| TikTok → Threads | веб-адрес Threads в поле «Сайт», призыв | длинный путь, данных нет | В / Н |
| TikTok → X | то же | данных нет | В / Н |
| Instagram → TikTok | стикер-ссылка в Stories (1 касание), 5 ссылок в профиле | Stories видит уже тёплая база; реклама Meta со ссылкой в TikTok запрещена с 08.10.2026 в 7 странах (Португалии в списке нет, по данным C) | В / Н |
| Instagram → Threads | кросспост, значок Threads (подписка в 1 тап), рекомендации Threads в ленте IG, реклама Threads в Advantage+ | дублирование принять за перелив; кликов в Insights нет | В / С |
| Instagram → X | Stories-стикер, профиль | обычная воронка, без нативной помощи | В / Н |
| Threads → TikTok | ссылка в теле поста, 5 ссылок в профиле | Threads мало кликают; клики не видны | В / Н |
| Threads → Instagram | ссылка в посте, связка профилей Meta | не путать с репостом Thread в свои Stories | В / Н |
| Threads → X | ссылка в посте | нет нативной помощи | В / Н |
| X → TikTok | ссылка в теле, в ответе, поле сайта | спорное снижение показов у ссылочных постов | В / С (спор) |
| X → Instagram | то же | то же; задержка переходов в 2023 | В / С (спор) |
| X → Threads | то же; Meta тестировала импорт списка подписок из X (G) | импорт помогает найти людей, но не переносит подписки | В / С |

Различие, которое размывает нога Grok: «нативная загрузка своего ролика» на другой платформе это параллельное распространение, а не перелив людей. Оно снимает риск водяного знака, но само по себе никого не переводит.

## 4. Агентские кейсы с источником

Цифры только из отчётов ног со ссылкой на первоисточник. Это самореклама агентств, вендоров или TikTok, независимой проверки нет ни у одного кейса. Ни один не измеряет переход «подписчик сети A → подписчик сети B».

| Агентство | Клиент | Маршрут | Цифры (как опубликовано) | Не раскрыто | Ссылка | Нога |
|---|---|---|---|---|---|---|
| VaynerMedia | MUG Root Beer (США) | органика TikTok + IG | >1 млрд показов; >500 тыс. подписчиков по соцсетям за квартал | разбивка по сетям, бюджет, атрибуция | https://vaynermedia.com/case_study/mug-root-beer-case-study-reviving-an-icon/ | G M (единственное совпадение) |
| Ubiquitous | Zilla (зоотовары, США) | авторы TikTok + платное усиление → сайт | >9,1 млн просмотров; 58 826 платных кликов; CPM около $11 | бюджет, база, подписки | https://www.ubiquitousinfluence.com/post/zilla-case-study | G |
| Ubiquitous | NightCap | TikTok + IG → сайт | около 27,8 млн просмотров TikTok и 37,1 млн IG; около 56 тыс. посетителей сайта за месяц | причинная связь, клики | https://www.ubiquitousinfluence.com/post/nightcap-case-study | G |
| Ubiquitous | Hers (2022) | TikTok + Spark Ads → сайт | 5 млн показов; 11 тыс.+ пользователей в воронку; CPM $5 | бюджет, подписки; кейс старый | https://www.ubiquitousinfluence.com/post/hers-case-study | M |
| House of Marketers | Pulsenmore (8 рынков) | платный TikTok + Meta | 5,7 млн просмотров; 11 551 подписчик суммарно; CPF от $0,93 | разбивка по сетям, бюджет | https://houseofmarketers.com/pulsenmore-multimarket-paid-social-campaign/ | G |
| House of Marketers | EuroLeague | рост внутри TikTok | 30 млн+ просмотров; 238 тыс. подписчиков TikTok | дата, бюджет; это не перелив | https://houseofmarketers.com/euroleague-basketball-tiktok-influencer-case-study/ | M |
| Movers+Shakers | SmartyPants | один креатив в TikTok и Reels | 68 млн просмотров; +12,3 п.п. запоминаемость рекламы | клики, подписки | https://moversshakers.co/smartypants-vitamins | G |
| NoGood | Culligan | TikTok | 44 млн просмотров; 54 200 новых подписчиков; $0,98 за подписчика; $3,30 за лид | окно, контроль | https://nogood.io/case-study/culligan/ | G |
| Socially Powerful | Medik8 | авторы + TikTok | +486% подписчиков; 13 млн просмотров | абсолютная база, клики | https://sociallypowerful.com/our-work/medik8-social-marketing-agency | G |
| Publicis | Milka (Нидерланды) | реклама TikTok → лендинг | кликов на 67% больше прогноза; CPC €0,19; **первая волна провалилась: кликали, но не вовлекались** | бюджет, подписки | https://ads.tiktok.com/business/en/inspiration/milka | C |
| Adwise | Takkenkamp Groep | Spark Ads → подписки TikTok | база около 5 000; +88,5% подписчиков за 1,5 месяца; подписчик дешевле €1 | лиды, деньги | https://ads.tiktok.com/business/en/inspiration/takkenkamp-groep-tiktok-success-stories | C |
| Social & Impact | Ninja Kitchen Italia | Smart+ → лендинг | CPC €0,07; CTR 0,33% | бюджет | https://ads.tiktok.com/business/en/inspiration/ninja-x-social-and-impact-tiktok-success-story | C |
| без агентства | Zenfulnote | TikTok → ссылка профиля | 1,7 млн просмотров → около 1 400 кликов (около 0,08%) | агентство | https://ads.tiktok.com/business/en-US/inspiration/zenfulnote-drives-clicks | C |
| SoldOut (Япония) | 147 аккаунтов e-commerce | реклама в Threads (Advantage+) | CPM Threads около 60% от IG; конверсия около 2x против FB/IG (май-август 2025) | бюджет; без контроля; один рынок | https://note.com/soldout_official/n/n18219f94b812 | M |
| Sendible (вендор, не агентство) | сам Sendible | органика внутри Threads | 300 → 197 тыс. просмотров за квартал; 4 956 подписчиков за 14 месяцев; клики не раскрыты | лиды | https://sendible.com/insights/how-to-grow-on-threads-and-build-a-human-brand-presence | M |
| Digital Minds | ARC (полуназван) | TikTok с перетоком в IG | 1,1 млн просмотров; +2,5 тыс. подписчиков | клиент, лиды, бюджет | https://digitalminds.agency/tiktok-marketing-success-story/ | K M |
| The Shelf | Engine (B2B) | скетчи IG Reels + TikTok + LinkedIn | 510 тыс.+ показов | лиды, подписки | https://www.theshelf.com/portfolio/case-study-engine/ | M |
| Quimby Digital | бренд детского питания (не назван) | органика TikTok + IG | 675 тыс. просмотров; +272% подписчиков TikTok | клиент, клики | https://quimbydigital.com/baby-nutrition-brand-tiktok-case-study/ | K |
| Spark Social | Paperage | TikTok + IG | 380 тыс. просмотров; +4,9 тыс. подписчиков IG | атрибуция | https://www.sparksocialagency.com/work/paperage | K |

Самые полезные для решения: **Zenfulnote** (огромный охват, 0,08% кликов), **Milka** (клики не дали вовлечения), **Sendible** (охват Threads без кликов) и **SoldOut** (единственные цифры по дешевизне рекламы Threads, но агентские). MOMENTiQ (продажи внутри TikTok Shop) и кейсы Grok из `leadspro.ai` не взяты: это не перелив или клиент не назван.

## 5. Расхождения между ногами

**Прямые опровержения:**

1. **Клики в Threads Insights (2 против 2).** ChatGPT и Mistral: общие клики по ссылкам в Insights есть (обновление Meta от мая 2025), Podcast Insights просто отдельный продукт. Claude и Grok: клики из Insights удалены к сентябрю 2026. Я открыл Engadget: «the company has eliminated data around links clicks altogether» (08.09.2026). **ChatGPT и Mistral устарели**: они опираются на пресс-релиз 2025 года. Их план «смотреть клики в Insights Threads» и приоритет Mistral №2 «Insights уже считает клики» не работают.
2. **Штраф за ссылку на X (полярные метки).** Mistral: «доказано, высокая уверенность» (Buffer + один тест с −94%). Grok: «опровергнуто» (открытый код, вес `open_link = +0.2`, слова Маска). ChatGPT: «данных недостаточно». Claude: «спорно». Я проверил: README `xai-org/x-algorithm` упоминает ссылку только в списке предсказываемых кликов, веса +0.2 и правила против URL там нет; PPC Land пишет, что явного правила нет, но подавление «возникает косвенно» из модели. **Вердикт синтеза: спорно; обе крайние метки сильнее, чем позволяют данные.** Наблюдения Buffer до августа 2025 и один тест 2024 года не описывают X после заявлений июля 2026; заявления руководства не являются замером.
3. **Цитата Маска.** Mistral в кратком выводе пишет, что Маск заявил, будто X «штрафуют уже больше года». По первоисточнику (проверено) он сказал обратное: «We haven't for over a year». В таблице Mistral смысл передан верно; ошибка в одном месте, но в ключевом выводе.
4. **Цифры Buffer про X.** Grok пишет «текст 3,56% против ссылок 2,25%, 18,8 млн постов». В статье Buffer про ссылки (проверено) у обычных аккаунтов медиана ссылочных постов 0%, у текста около 0,40%. Похоже, Grok смешал выборку статьи про ссылки с цифрами другого отчёта Buffer. Цифры Grok здесь не использовать.
5. **Ссылки на соцсети в профиле TikTok (3 против 1).** Claude и Mistral: нативные кнопки Instagram и YouTube удалены в ноябре 2025, осталось поле «Сайт» с веб-адресом. ChatGPT описывает «social/profile link» как доступный путь. Источник большинства вторичный; проверить в приложении на аккаунте Антона.

**Разные критерии, а не прямой спор:**

- **Что считать кейсом.** ChatGPT берёт 7 агентств с их сайтов; Claude строго 3 кейса с TikTok for Business; Grok добавляет продавца инструмента PostOnce; Mistral смешивает агентства с вендорами (Sendible) и честно помечает это. Пересечение кейсов между ногами почти нулевое: VaynerMedia × MUG у ChatGPT и Mistral, Digital Minds у Grok и Mistral. Поэтому таблица склеена, а не усреднена.
- **Что считать маршрутом.** Grok считает перезаливку ролика маршрутом; остальные отделяют её от перевода людей.
- **Уникальные находки одной ноги:** Destination Links TikTok (G, проверено мной); запрет Meta на рекламу со ссылкой в TikTok и уход Бира с поста главы продукта X 05.08.2026 (C); импорт подписок из X в Threads (G); Premium 6-10x, замедление ссылок X в 2023, дешёвый CPM Threads, разбор сентябрьских правил TikTok (M). Кроме Destination Links, я их не перепроверял.
- **Ссылки в подписях Instagram.** ChatGPT упоминает платный «Meta One» с сентября 2026; Claude подтверждает только тест для Meta Verified (март 2026). Считать «Meta One» неподтверждённым.
- **Глубина поиска.** Claude: 18 запросов, часть официальных страниц не открыта. ChatGPT: 31 открытая страница. Mistral: 42 запроса, 29 открытых страниц, но справка TikTok вернула ему 404. Grok перечисляет много URL без разделения «открыл» и «нашёл».

## 6. Приоритетные маршруты для Антона и эксперимент

Антон ведёт личные аккаунты инженера, база Лиссабон. Ни одна нога не знает его стартовых чисел, поэтому порядок ниже про стоимость и короткий путь, а не про обещанный результат. Instagram → Threads стоит первым у ChatGPT, Claude и Mistral; у Grok вторым.

1. **Instagram → Threads.** Кросспост, значок Threads в профиле, проверить, что показ Threads в IG не выключен. Издержка почти ноль. Мерить рост подписчиков Threads по дням и опросом, кликов в Insights нет.
2. **Ссылка в теле поста на X и в Threads → сайт, рассылка, подкаст или другой профиль.** Издержка ноль; главный неизвестный параметр: ссылка в теле против ссылки в ответе. Premium на X в эксперименте не менять (по Buffer он сам по себе меняет охват).
3. **Stories Instagram со стикером → X, Threads или сайт.** Один шаг, но работает только на тёплой базе IG.
4. **Чистые ролики TikTok ↔ Reels без водяного знака.** Не перелив, но снимает единственный задокументированный штраф. Переходов от призыва в ролике много не ждать (Zenfulnote: около 0,08%).
5. **Если есть подкаст:** карточки эпизодов в Threads, единственное место, где Threads сейчас показывает клики.

**План на 3 недели (сведён из планов ChatGPT, Claude и Mistral):**

- **Гипотезы.** H1: на X ссылка в теле даёт больше уникальных переходов на 1 000 показов, чем ссылка в первом ответе, без падения показов более 30% к медиане аккаунта. H2: то же в Threads. H3: недели с кросспостом IG → Threads и Stories-стикером дают больше новых подписок Threads в день, чем недели без них.
- **Варианты для X и Threads:** A ссылка в теле; B ссылка в первом ответе; C «ссылка в профиле» без URL; D контроль без призыва.
- **Рандомизация:** единица = пост; блоки по 4 поста (по одному варианту), порядок в блоке случайный; темы из размеченного пула; без платного продвижения; статус Premium на X не менять весь тест.
- **UTM:** `utm_source=x|threads|instagram|tiktok`, `utm_medium=organic_social`, `utm_campaign=dr261115`, `utm_content=<postID>_<A|B|C|D>`. Переход в соцсеть вести через свой короткий редирект с логом кликов. На referrer не опираться: встроенные браузеры его теряют, на X он заменяется на t.co.
- **Метрики:** основная = уникальные UTM-сессии на 1 000 показов; вторичная = новые подписки у получателя и лиды за 72 часа (лиды до 7 дней); защитная = показы поста ÷ медиана показов аккаунта за 4 прошлые недели. Плюс вопрос новым подписчикам «откуда узнали».
- **Окно:** клики 72 часа, подписки и лиды 7 дней; если поток мал, добавить 1-2 недели наблюдения отложенных подписок (предложение Mistral).
- **Достаточность:** не меньше 20 постов на вариант (около 80 на платформу); для доли «клик → подписка» стремиться к 100-200 кликам на вариант (ChatGPT), минимум 300 UTM-кликов суммарно (Mistral). Меньше = «мощности не хватило», а не «штрафа нет».
- **Остановка:** вариант снимается, если после 10 постов его медиана показов ниже 50% контроля D.

## 7. Ограничения и что не проверено

- Кворум 4 из 5: Gemini не готов.
- Ни одного причинного A/B по переливу между этими сетями ни одна нога не нашла; все «штрафы за ссылку» держатся на наблюдениях, одном тесте 2024 года и заявлениях руководства.
- Агентские кейсы: самореклама, без независимой проверки, в основном платные и в основном TikTok → сайт.
- Не перепроверены мной: запрет Meta на рекламу со ссылкой в TikTok, уход Бира, порог 1 000 подписчиков TikTok, удаление кнопок соцсетей в профиле TikTok, «Meta One», импорт подписок X → Threads, данные PostOnce, Premium 6-10x, тест с −94%, CPM Threads у SoldOut, расширение правил Instagram 30.04.2026.
- Веса модели X (`param.rs`) не открывал; README не подтверждает и не опровергает штраф.
- Цифры вовлечённости разных платформ несопоставимы: у них разные знаменатели (Buffer считает по охвату, X отдаёт показы; Mistral показывает три разных числа для одной Threads).

## Что я проверил сам (16.11.2026, WebFetch)

| URL | Результат |
|---|---|
| https://www.engadget.com/2251592/threads-is-overhauling-its-in-app-analytics-with-more-details-except-for-link-clicks/ | ✅ подтвердилось: «eliminated data around links clicks altogether», 08.09.2026; когда именно убрали, не сказано |
| https://buffer.com/resources/links-on-x/ | ✅ подтвердилось: 14.10.2025, 18,8 млн постов, 71 тыс. аккаунтов; медиана у обычных аккаунтов «stuck at zero»; это вовлечённость, данных о показах нет |
| https://ppc.land/x-drops-year-old-link-penalty-musk-tells-zuckerberg-on-platform/ | ✅ подтвердилось: Маск 29.07.2026 «We haven't for over a year»; Бир 28.07.2026 про ответы; 94% = анализ PPC Land без методики |
| https://about.fb.com/br/news/2024/04/ajudando-o-criador-de-conteudo-a-encontrar-novos-publicos/ | ✅ подтвердилось: 30.04.2024, водяной знак исключает из рекомендаций; агрегатор 10+ раз за 30 дней |
| https://ads.tiktok.com/resources/help/article/about-destination-links | ✅ подтвердилось: «You must be a Verified Business Account», нужен пиксель с событием оплаты |
| https://github.com/xai-org/x-algorithm | ⚠️ частично: ссылка только в списке предсказываемых кликов; веса +0.2 и правила против URL в README нет, утверждение Grok не подтверждено |

## Дополнение 20.11.2026
- Пятая нога Gemini собрана хабом и разобрана отдельно: insight-DR-DR26-11-19-MAYAK-01-перелив-аудитории-между-tiktok-instagram-threads-и. Выводы этой заметки подтверждает; новое (SmartSites 0,04 против 0,88, потеря реферера в WebView, кодовое слово в DM) вписано в synthesis.md раздел 8 на Студии.
