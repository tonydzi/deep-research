# DR26-08-24-HUB-06-2040 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

плейбук упаковки: как поднимают крупно при слабом adoption, 2024–2026

срез данных: 24.08.2026. «не раскрыто» означает отсутствие проверяемых публичных данных, а не нулевое значение.

executive summary — 7 строк

1. во всех восьми кейсах выручку в момент раунда заменял не один красивый показатель, а связка из трех элементов: дефицитный актив, проверяемый артефакт и известный финансовый якорь; при этом публично проверяемой выручки не было ни в одном объявлении раунда.
2. миллиардные исключения — ssi и thinking machines — являются рынком покупки конкретных людей и команд, а не воспроизводимым playbook продукта: $1 млрд и $2 млрд были получены до публичного продукта благодаря бывшим руководителям openai и собранным вокруг них исследователям ([reuters, 04.09.2024](https://www.investing.com/news/stock-market-news/exclusiveopenai-cofounder-sutskevers-new-safetyfocused-ai-startup-ssi-raises-1-billion-3600613), [reuters, 18.02.2025](https://www.investing.com/news/stock-market-news/former-openai-technology-chief-mira-muratis-ai-startup-taps-top-researchers-from-competitors-3875339), [techcrunch, 15.07.2025](https://techcrunch.com/2025/07/15/mira-muratis-thinking-machines-lab-is-worth-12b-in-seed-round/)).
3. наиболее воспроизводимый аналог для неизвестной security-команды — codeintegrity: публично продемонстрированная уязвимость, пилоты в регулируемых отраслях и профильный cyber-фонд дали $5 млн seed без раскрытой выручки и без звездного founder pedigree ([geekwire, 27.05.2026](https://www.geekwire.com/2026/codeintegrity-raises-4-8m-to-put-permanent-guardrails-on-unpredictable-ai-agents/)).
4. runlayer показывает, что для прыжка от $5 млн к $10 млн+ одного исследования уже мало: там были повторный фаундер, опыт создания zapier mcp, соавтор mcp как ангел/эдвайзор и заявленные восемь крупных клиентов ([techcrunch, 17.11.2025](https://techcrunch.com/2025/11/17/mcp-ai-agent-security-startup-runlayer-launches-with-8-unicorns-11m-from-khoslas-keith-rabois-and-felicis/)).
5. github-звезды не умерли как верхневороночный индикатор, но перестали быть самостоятельным доказательством adoption: исследование 800 коммерческих oss-компаний считает community criticality более предсказательной, чем raw stars ([linux foundation, 2025](https://www.linuxfoundation.org/hubfs/Research%20Reports/lfr_serena_capital_report_082225b.pdf?hsLang=en)).
6. настоящий audience-first рынок появился, но пока не подтверждает чек $5 млн+: slow ventures пишет чеки $1–3 млн примерно за 10% creator-holdco и прямо требует конверсии аудитории, обычно около $1 млн уже существующей выручки ([inc., 25.02.2025](https://www.inc.com/annabel-burba/vc-firm-slow-ventures-is-investing-60-million-in-content-creators-heres-what-its-looking-for/91152178)).
7. для описанного профиля лаборатории доказуемый диапазон сегодня — $3–5 млн; $6–8 млн становится правдоподобным только после двух-трех именуемых design-partner’ов, независимой верификации exploit-корпуса и первого признанного cyber/infra-якоря; оснований для чека выше $8 млн без этих добавок в найденных аналогах нет.

таблица кейсов

| команда | что подняли: сумма, стадия, дата | оценка | лид-инвестор | что реально было в момент раунда | чем заменили выручку | структура сделки | founder-story якорь | от основания до чека | воспроизводимо без имени? |
|---|---|---|---|---|---|---|---|---|---|
| safe superintelligence, ssi | $1 млрд cash, первый раунд, 04.09.2024 ([reuters, 04.09.2024](https://www.investing.com/news/stock-market-news/exclusiveopenai-cofounder-sutskevers-new-safetyfocused-ai-startup-ssi-raises-1-billion-3600613)) | около $5 млрд по источникам reuters; компания оценку не подтвердила ([reuters, 04.09.2024](https://www.investing.com/news/stock-market-news/exclusiveopenai-cofounder-sutskevers-new-safetyfocused-ai-startup-ssi-raises-1-billion-3600613)) | единый лид не раскрыт; nfdg, a16z, sequoia, dst, sv angel | 10 сотрудников; продукта и пользователей не было; компания собиралась провести «пару лет» в r&d до рынка ([reuters, 04.09.2024](https://www.investing.com/news/stock-market-news/exclusiveopenai-cofounder-sutskevers-new-safetyfocused-ai-startup-ssi-raises-1-billion-3600613)) | pedigree, дефицит исследователей, тезис «одна цель — safe superintelligence», сознательный отказ от промежуточных product cycles ([ap, 20.06.2024](https://apnews.com/article/c6b48a3675fb3fb459859dece2b45499)) | обычная for-profit компания и денежный раунд; конкретный класс акций и preferences не раскрыты ([reuters, 04.09.2024](https://www.investing.com/news/stock-market-news/exclusiveopenai-cofounder-sutskevers-new-safetyfocused-ai-startup-ssi-raises-1-billion-3600613)) | илья суцкевер — сооснователь и бывший chief scientist openai; daniel gross ранее руководил ai-инициативами apple | около 77 дней: анонс основания 19.06.2024 → раунд 04.09.2024 ([time, 19.06.2024](https://time.com/6990076/safe-superintelligence-inc-announced/)) | нет; копируемы только узкая миссия и отказ от распыления |
| thinking machines lab | $2 млрд seed, закрыт 14–15.07.2025 ([reuters, 15.07.2025](https://finance.yahoo.com/news/mira-muratis-thinking-machines-lab-valued-170344492.html), [techcrunch, 15.07.2025](https://techcrunch.com/2025/07/15/mira-muratis-thinking-machines-lab-is-worth-12b-in-seed-round/)) | $12 млрд | andreessen horowitz | публичного продукта еще не было; первый продукт обещался «в следующие пару месяцев»; команда насчитывала около 30 исследователей и инженеров из openai, meta и mistral ([reuters, 18.02.2025](https://www.investing.com/news/stock-market-news/former-openai-technology-chief-mira-muratis-ai-startup-taps-top-researchers-from-competitors-3875339), [techcrunch, 15.07.2025](https://techcrunch.com/2025/07/15/mira-muratis-thinking-machines-lab-is-worth-12b-in-seed-round/)) | имя mira murati, коллективное pedigree, возможность купить долю в редкой команде до появления продукта | названа seed round; точный инструмент, preferences и доля не раскрыты | бывший cto openai плюс несколько бывших руководителей и исследователей openai ([wired, 15.07.2025](https://www.wired.com/story/thinking-machines-lab-mira-murati-funding/)) | около 5 месяцев после публичного запуска 18.02.2025 | нет; это talent round |
| reflection ai | одновременно объявлены $25 млн seed и $105 млн series a, всего $130 млн, 07.03.2025 ([bloomberg, 07.03.2025](https://news.bloomberglaw.com/private-equity/ex-deepmind-researchers-new-startup-aims-for-superintelligence)) | источники расходятся: около $545–555 млн ([reuters, 09.10.2025](https://www.investing.com/news/stock-market-news/nvidiabacked-reflection-ai-raises-2-billion-in-funding-boosts-valuation-to-8-billion-4279981), [forge, данные раунда 07.03.2025](https://forgeglobal.com/reflection-ai_ipo/)) | seed: sequoia и crv; series a: lightspeed и crv | компания выходила из stealth; заявляла неназванных платящих клиентов, но публичный продукт asimov появился только 16.07.2025 как «first product milestone» ([sequoia, 16.07.2025](https://sequoiacap.com/article/reflection-ai-asimov)) | alpha­go/gemini pedigree, автономное программирование как ступень к superintelligence, ранние закрытые клиенты | preferred equity; forge указывает 1x non-participating liquidation preference и 6% non-cumulative dividend для series a ([forge, данные 07.03.2025](https://forgeglobal.com/reflection-ai_ipo/)) | сооснователь ioannis antonoglou был founding engineer deepmind и работал над alphago; команда связывала себя с alphazero, muzero, palm, chatgpt и gemini ([reflection, 07.03.2025](https://reflection.ai/blog/reflection-a-path-to-superintelligence/)) | примерно 12 месяцев после основания в 2024 году ([sequoia, доступ 24.08.2026](https://sequoiacap.com/companies/reflection-ai)) | частично: воспроизводимы закрытые design-partner’ы и реальная демонстрация; pedigree — нет |
| cognition / devin | около $175 млн series b, апрель 2024, через месяц после series a ([the information, апрель 2024](https://www.theinformation.com/articles/six-month-old-ai-coding-startup-valued-at-2-billion-by-founders-fund), [forge, данные 24.04.2024](https://forgeglobal.com/cognition_ipo/)) | $2 млрд post-money | founders fund | devin существовал как видео-демо и limited early access; желающие должны были вступить в waitlist; general availability появилась только 10.12.2024 ([cognition, 12.03.2024](https://cognition.com/blog/introducing-devin), [cognition, 10.12.2024](https://cognition.com/blog/devin-generally-available)) | вирусная демонстрация, заявленные 13,86% на swe-bench, competitive-programming pedigree, повторный founders fund | priced series b; точные preferences публично не подтверждены | ранняя команда заявляет десять золотых медалей international olympiad in informatics и людей из deepmind, scale, cursor, waymo и nuro ([cognition, доступ 24.08.2026](https://cognition.com/about)) | около 6 месяцев; сама публикация называла компанию six-month-old | частично: воспроизводимы демонстрация и benchmark; недоступны концентрация олимпиадного pedigree и founders fund с первого дня |
| sierra | свежие $85 млн series b 13.02.2024; pr-цифра $110 млн включала предыдущие $25 млн benchmark ([forge, данные 13.02.2024](https://forgeglobal.com/sierra-ai_ipo/), [techcrunch, 19.02.2024](https://techcrunch.com/2024/02/19/sierra-ai-agents-customer-service/)) | около $950 млн post-money / «почти $1 млрд» | sequoia; первый $25 млн чек был benchmark | продукт был; четыре именуемых партнера — weightwatchers, siriusxm, sonos, olukai; компания сама заявляла, что агент weightwatchers обрабатывал почти 70% сессий при csat 4,6/5; выручка не раскрывалась ([sierra, 13.02.2024](https://sierra.ai/es/blog/introducing-sierra)) | design-partner’ы с известными брендами, production-метрика и исключительное founder pedigree | series b preferred equity; forge указывает 1x non-participating preference ([forge, данные 13.02.2024](https://forgeglobal.com/sierra-ai_ipo/)) | bret taylor: бывший co-ceo salesforce, cto facebook, соавтор google maps, председатель openai; clay bavor — 18 лет в google и бывший руководитель google labs ([sierra, доступ 24.08.2026](https://sierra.ai/about)) | около 12 месяцев до $85 млн; первый $25 млн чек — примерно через 4 месяца после начала в 2023 году ([forge](https://forgeglobal.com/sierra-ai_ipo/)) | частично: design-partner playbook копируем; доступ к таким партнерам и founder pedigree — нет |
| runlayer | $11 млн seed, 17.11.2025 ([techcrunch, 17.11.2025](https://techcrunch.com/2025/11/17/mcp-ai-agent-security-startup-runlayer-launches-with-8-unicorns-11m-from-khoslas-keith-rabois-and-felicis/)) | не раскрыта | khosla ventures / keith rabois и felicis | рабочий продукт четыре месяца находился в stealth; компания заявляла десятки клиентов и восемь unicorn/public-company клиентов, называя gusto, dbt labs, instacart и opendoor; выручка не раскрыта ([techcrunch, 17.11.2025](https://techcrunch.com/2025/11/17/mcp-ai-agent-security-startup-runlayer-launches-with-8-unicorns-11m-from-khoslas-keith-rabois-and-felicis/)) | customer logos, бывшая команда zapier ai, соавтор mcp david soria parra как ангел/эдвайзор, категория «mcp control plane» | инструмент и preferences не раскрыты | andrew berman — третий стартап и продажа vowel компании zapier; команда ранее создавала zapier mcp и ai actions ([felicis, 17.11.2025](https://www.felicis.com/blog/runlayer-announcement)) | около 3–4 месяцев после ухода команды из zapier | частично: customer sprint и mcp-security wedge копируемы; статус соавтора протокола и repeat-founder — нет |
| codeintegrity | $5 млн seed, 27.05.2026 ([codeintegrity, 27.05.2026](https://www.codeintegrity.ai/blog/codeintegrity-seed-round)) | не раскрыта | syn ventures | продукт находился в пилотах у неназванных компаний регулируемых отраслей; широкий запуск планировался позднее; именуемых клиентов и выручки не опубликовано ([geekwire, 27.05.2026](https://www.geekwire.com/2026/codeintegrity-raises-4-8m-to-put-permanent-guardrails-on-unpredictable-ai-agents/)) | offensive-research: команда показала компрометацию notion менее чем за четыре часа; затем упаковала это в deterministic runtime-control layer ([geekwire, 27.05.2026](https://www.geekwire.com/2026/codeintegrity-raises-4-8m-to-put-permanent-guardrails-on-unpredictable-ai-agents/)) | seed; точный инструмент и preferences не раскрыты | не звездное имя, а «security researchers who found the problem before the market»; antler и boost vc дали раннюю институциональную маркировку | около 24 месяцев: основана в мае 2024 года → seed в мае 2026 года ([geekwire, 27.05.2026](https://www.geekwire.com/2026/codeintegrity-raises-4-8m-to-put-permanent-guardrails-on-unpredictable-ai-agents/)) | да; это самый близкий воспроизводимый образец |
| gonka / product science — эталон-строка | bitfury 02.12.2025 объявила не закрытый раунд, а commitment инвестировать $50 млн; то же сообщение упоминает одобренную community-покупку gnk на $12 млн ([business wire через venturebeat, 02.12.2025](https://venturebeat.com/business/bitfury-announces-50-million-investment-in-gonkaa-decentralized-network-for-highefficiency-ai-compute/)) | не раскрыта | bitfury | mainnet работал около трех месяцев; заявлена supply-мощность, эквивалентная более чем 6 000 h100, предоставленная распределенными hosts для inference; платный спрос, utilization, клиенты и выручка не раскрыты ([business wire, 02.12.2025](https://venturebeat.com/business/bitfury-announces-50-million-investment-in-gonkaa-decentralized-network-for-highefficiency-ai-compute/)) | supply-side compute, on-chain governance, institutional token buyer, pedigree product science | подтверждена покупка community-токенов на $12 млн; отношение этой покупки к $50 млн commitment, график финансирования и остаточная структура не раскрыты | проект инкубирован product science, ранее поддержанной coatue, slow и k5; семья фаундеров ранее работала в snap и поднимала $17–18 млн для product science ([business insider, архив](https://www.libermans.co/press/business-insider-product-science)) | около 3 месяцев от запуска mainnet; дата начала инкубации не подтверждена | частично: воспроизводимы on-chain supply и работающая сеть; якорь bitfury и предыдущий founder track record — нет |

1. общий паттерн: семь инвариантов упаковки

1. капитал покупает не текущую выручку, а право на дефицитный будущий актив. у ssi и thinking machines активом были люди; у reflection и cognition — команда плюс техническая демонстрация; у sierra и runlayer — доступ к известным design-partner’ам; у codeintegrity — найденная атакующая поверхность; у gonka — вычислительная supply-сеть. описание рынка без такого принадлежащего компании актива не сработало ни в одном кейсе.

2. один острый wedge предшествует большой платформе. «safe superintelligence», «autonomous coding», «customer-service agent», «mcp control plane», «deterministic runtime security» и «decentralized ai compute» — все истории начинались с одного предложения и лишь затем расширялись до «lab», «operating system» или «control layer». это уменьшает воспринимаемый риск входа, сохраняя большой tam.

3. доказательство упаковано в артефакт, который инвестиционный комитет может пересказать. это может быть имя фаундера, 13,86% swe-bench, четыре design-partner’а, восемь крупных клиентов, exploit notion или 6 000 h100-equivalent; вопрос не в абсолютной силе цифры, а в ее способности пережить пересказ между партнером фонда и investment committee.

4. у каждого кейса есть финансовый или институциональный переводчик. публичная первая строка всегда содержит известное имя: a16z, sequoia, founders fund, benchmark, lightspeed, khosla/felicis, syn или bitfury. без такого переводчика технический сигнал остается интересным исследованием, а не раундом.

5. скорость сама становится суррогатом. ssi получила первый раунд примерно за 77 дней, thinking machines — за пять месяцев, cognition — примерно за шесть, runlayer — за четыре, gonka — через три месяца после mainnet. инвестор покупает не завершенность, а скорость превращения команды в редкий сигнал.

6. публичная метрика выбирается раньше, чем публичная экономика. компании называли benchmark, клиентские logos, число gpu, customer sessions или pilot sectors, но почти нигде не раскрывали arr, gross margin, retention, paid utilization или contract value. это не обязательно обман; это контролируемое раскрытие сильнейшего доступного факта.

7. раунд объявляется одновременно с новой категорией или идентичностью. reflection вышла из stealth вместе с двумя раундами; cognition — вместе с devin; runlayer — вместе с «enterprise mcp»; codeintegrity — с «agent runtime control». деньги здесь работают как часть product launch: известный инвестор подтверждает, что заявленная категория институционально существует.

2. три корзины

а) чистая механика — законно и воспроизводимо

- ответственное раскрытие настоящей уязвимости: воспроизводимый poc, affected versions, vendor acknowledgement, remediation и безопасная демонстрация. codeintegrity превратила именно такой исследовательский сигнал в seed, оставаясь в пилотах и без звездного имени ([geekwire, 27.05.2026](https://www.geekwire.com/2026/codeintegrity-raises-4-8m-to-put-permanent-guardrails-on-unpredictable-ai-agents/)).

- именуемые design-partner’ы с конкретным production use case. у sierra важны не размеры самих weightwatchers или siriusxm, а право назвать их, описать рабочий процесс и показать долю обработанных сессий ([sierra, 13.02.2024](https://sierra.ai/es/blog/introducing-sierra)).

- одна метрика, привязанная к рабочей нагрузке: предотвращенные атаки, доля безопасно разрешенных tool calls, time-to-detect, false-positive rate, число production agents под policy, время установки, expansion внутри клиента. bessemer считает business-grounded accuracy, latency, hallucination rate и customer satisfaction более полезными, чем публичные leaderboard’ы ([bessemer, 13.08.2025](https://www.bvp.com/atlas/the-state-of-ai-2025)).

- последовательность «закрытый пилот → разрешение назвать партнера → доказательство результата → открытие раунда». это воспроизводимая часть sierra, runlayer и codeintegrity.

- четкое разделение свежего раунда, cumulative funding и commitment. корректное раскрытие само по себе создает доверие; reflection и sierra показывают, как headline total может включать несколько траншей ([bloomberg, 07.03.2025](https://news.bloomberglaw.com/private-equity/ex-deepmind-researchers-new-startup-aims-for-superintelligence), [forge, данные sierra](https://forgeglobal.com/sierra-ai_ipo/)).

б) требует репутации или имени

- «фаундер и есть продукт»: ssi и thinking machines. такую механику нельзя получить красивым deck; она образована предыдущими системами, должностями, research attribution и личной способностью нанять десятки редких исследователей.

- коллективное pedigree: reflection могла говорить не только «ex-deepmind», но и связывать команду с alphago, alphazero, muzero, palm и gemini ([reflection, 07.03.2025](https://reflection.ai/blog/reflection-a-path-to-superintelligence/)).

- повторный институциональный кредит: founders fund инвестировал в cognition снова через месяц; benchmark вошел в sierra до sequoia; прежние инвесторы product science делали gonka понятнее новому стратегу.

- авторство протокола. у runlayer присутствие соавтора mcp закрывало вопрос «почему именно эта команда имеет право владеть слоем вокруг стандарта» ([felicis, 17.11.2025](https://www.felicis.com/blog/runlayer-announcement)).

- бывшая высокая должность не равна обычному advisor-logo: рынок покупает накопленную attribution — кто действительно создавал систему, стандарт или компанию.

в) серо-черное

- серое: выдавать supply за demand. количество gpu, nodes или theoretical capacity полезно как доказательство готовности сети, но без paid utilization, уникальных покупателей и gross margin не доказывает бизнес. именно такое различие требуется держать в строке gonka.

- серое: объявлять cumulative funding как один свежий чек. у sierra headline $110 млн состоял из ранних $25 млн и свежих $85 млн; у reflection $130 млн объединяли $25 млн seed и $105 млн series a ([forge — sierra](https://forgeglobal.com/sierra-ai_ipo/), [bloomberg — reflection, 07.03.2025](https://news.bloomberglaw.com/private-equity/ex-deepmind-researchers-new-startup-aims-for-superintelligence)).

- серое: называть commitment «raised». сообщение gonka говорит, что bitfury «committed to investing $50 million», но не раскрывает график получения средств, условия или связь с покупкой gnk на $12 млн ([business wire, 02.12.2025](https://venturebeat.com/business/bitfury-announces-50-million-investment-in-gonkaa-decentralized-network-for-highefficiency-ai-compute/)).

- черное: приписывать пользователей, клиентов, контракты, revenue или security findings, которых не было; это уже ложное существенное заявление, а не aggressive storytelling.

- черное: wash trading токена для создания фиктивного объема и цены. doj в октябре 2024 года предъявил обвинения market-maker’ам и token-компаниям именно за sham trades, которые создавали видимость спроса и позволяли продавать токены по искусственно раздутой цене ([doj, 09.10.2024](https://www.justice.gov/usao-ma/pr/eighteen-individuals-and-entities-charged-international-operation-targeting-widespread)).

- черное или близкое к нему: retail/ambassador-модель, где вознаграждение зависит преимущественно от рекрутирования новых участников или их обязательных покупок, а не от продаж независимым конечным пользователям. именно этот тест использует ftc для отличения обычного mlm от pyramid scheme ([ftc business guidance, доступ 24.08.2026](https://www.ftc.gov/business-guidance/resources/business-guidance-concerning-multi-level-marketing)).

3. метрики-суррогаты

| суррогат | принимается в 2026? | условие |
|---|---|---|
| именуемый платящий design-partner | да, сильный | инвестор или клиент может подтвердить договор, production scope и продолжение использования |
| пилот у банка, airline, frontier lab или госоргана | условно сильный | нужен владелец бюджета, конкретный workflow и дата перехода из pilot в production |
| expansion внутри одного клиента | да | рост числа защищенных agents, seats, tool calls или подразделений важнее числа logos |
| production retention и повторное использование | да, один из сильнейших | a16z после анализа ai-компаний называет retention главным входом в долгосрочный ltv/cac; usage patterns и feature adoption важнее регистрации ([a16z, 2025](https://a16z.com/ai-retention-benchmarks/)) |
| воспроизводимый exploit / cve / vendor patch | да для security | особенно силен, если есть vendor acknowledgement, независимое повторение и remediation |
| внешняя таксономия или стандарт | да | включение finding в owasp, nist, protocol spec или policy снижает риск «самоприсвоенного класса угроз» |
| авторство протокола или признанный maintainer status | да, очень сильный | вклад должен быть проверяемым; advisor-logo без реальной работы слаб |
| private eval на данных design-partner’а | да | a16z и bessemer отмечают переход enterprise-покупателей от публичных benchmark’ов к собственным use-case evals ([a16z, 2025](https://a16z.com/ai-enterprise-2025/), [bessemer, 13.08.2025](https://www.bvp.com/atlas/the-state-of-ai-2025)) |
| публичный benchmark | условно | работает как launch artifact, но не как доказательство retention; cognition — хороший пример силы и ограниченности такого сигнала |
| github stars | нет как самостоятельная метрика | stars измеряют внимание; linux foundation считает criticality/community health более предсказательными для стоимости oss-компаний ([linux foundation, 2025](https://www.linuxfoundation.org/hubfs/Research%20Reports/lfr_serena_capital_report_082225b.pdf?hsLang=en)) |
| merged pr от независимых организаций, contributor diversity, issue throughput, release cadence | да, условно | это уже evidence использования и зависимости, особенно если можно назвать компании-контрибьюторы; oxx в 2025 году использовала именно связку stars с contributor velocity, issue throughput и release cadence ([oxx, 2025](https://www.oxx.vc/rising-stars/rising-stars-2025/)) |
| downloads / installs | слабо | нужны unique organizations, активность после 30/90 дней и переход в production |
| waitlist | почти нет | работает только вместе с conversion rate, verified domains и временем до активации |
| зарегистрированные users / mau | условно | нужны retention cohorts, paid conversion и исключение ботов/airdrop hunters |
| followers, telegram-подписчики, views | нет как venture traction сами по себе | настоящий creator-фонд slow прямо говорит, что follower count «means less and less» и требует monetization/conversion, обычно около $1 млн уже существующей выручки ([inc., 25.02.2025](https://www.inc.com/annabel-burba/vc-firm-slow-ventures-is-investing-60-million-in-content-creators-heres-what-its-looking-for/91152178)) |
| audience-first holdco | да, но пока малые чеки | slow предлагает $1–3 млн примерно за 10%; первый раскрытый чек был $2 млн creator’у с 600 тыс. followers, 75 млн views и уже существующей линейкой инструментов ([techcrunch, 11.08.2025](https://techcrunch.com/2025/08/11/slow-ventures-cuts-first-check-from-60m-creator-fund-into-woodworking-founder/)) |
| loi / mou / partnership logo | слабо | не подтверждает покупку; необходимы budget owner, scope, срок и условия выхода |
| число gpu / nodes / hosts | условно для depin | должно дополняться paid utilization, revenue per gpu, external demand и retention поставщиков |
| token holders / fdv / trading volume | нет | это рыночная активность актива, не выручка компании; wash trading способен искусственно создавать объем ([doj, 09.10.2024](https://www.justice.gov/usao-ma/pr/eighteen-individuals-and-entities-charged-international-operation-targeting-widespread)) |
| «pipeline $x млн» | нет | принимается только booked/contracted часть с probability weighting и сроком close |
| press mentions / awards / product hunt | нет | годятся как distribution, но не как demand или retention |
| скорость найма редких исследователей | да только для frontier lab | сигнал того, что лидер способен собрать команду; без собственного pedigree почти не переносится |
| стратегический инвестор-клиент | да | наиболее силен, когда инвестор одновременно предоставляет compute, distribution, reference customer или стандарт, а не только логотип |

итог по github: «звезды больше не работают» слишком категорично. правильный вывод — raw star count больше не является достаточным доказательством. merged contributions, количество реально зависимых организаций, criticality, production installs и conversion в enterprise работают; stars остаются только верхневороночным attention signal.

4. роль якорного инвестора

у всех восьми кейсов в первой строке объявления присутствует известный финансовый или стратегический якорь; в семи из восьми публично назван lead или co-lead, а у ssi объявлен консорциум без подтвержденного единственного лидера.

в трех кейсах «первое имя» видно как последовательность:

- sierra: benchmark сначала вложил $25 млн; затем sequoia дала свежие $85 млн при оценке около $950 млн ([forge, данные раундов](https://forgeglobal.com/sierra-ai_ipo/)).

- reflection: sequoia и crv закрыли $25 млн seed; затем lightspeed и crv — $105 млн series a ([bloomberg, 07.03.2025](https://news.bloomberglaw.com/private-equity/ex-deepmind-researchers-new-startup-aims-for-superintelligence)).

- cognition: founders fund возглавил $21 млн series a, а примерно через месяц — следующий раунд около $175 млн при $2 млрд post-money ([cognition, 12.03.2024](https://cognition.com/blog/introducing-devin), [the information, апрель 2024](https://www.theinformation.com/articles/six-month-old-ai-coding-startup-valued-at-2-billion-by-founders-fund)).

механика первого имени:

1. первый якорь берет на себя не только финансовый, но и категорийный риск: он публично утверждает, что «mcp security», «autonomous coding» или новая ai-lab является отдельной инвестируемой категорией.

2. его diligence становится сжатым сертификатом для последующих участников. публичные данные не позволяют утверждать, что follower-инвесторы отказались от собственной проверки, но последовательность раундов показывает, что первый бренд резко меняет читаемость компании.

3. лучший якорь закрывает конкретный пробел: syn — cyber credibility; mcp-author — protocol legitimacy; sequoia/benchmark — enterprise-company-building; bitfury — depin/token infrastructure; a16z — frontier-lab financing.

4. известный ангел полезен только тогда, когда его связь отвечает на вопрос «почему эта команда имеет право выиграть». список случайных знаменитостей такого эффекта не дает.

5. механически «купить первое имя» через пассивного advisor’а нельзя. можно купить время сильного специалиста или нанять его, но в раунде работает проверяемое участие: инвестиция, board/advisory mandate, совместная работа, customer intro или соавторство стандарта.

вывод: все восемь историй держатся на institutional anchor, но только три публично показывают чистую лестницу «первое имя → существенно больший следующий чек». точные переговоры и причинность публично не подтверждены.

5. приложение к вашей лаборатории

что уже можно исполнить

1. «угроза → воспроизводимый артефакт → контроль». имеющийся exploit-корпус следует упаковывать не как «рынок страшный», а как три полностью документированных attack chain: вход, tool permissions, blast radius, affected implementation, vendor response, patch и проверка защиты. это наиболее близко к механике codeintegrity.

2. «oss proximity → earned protocol legitimacy». ценны не сами github-звезды и не фраза «нам отвечают мейнтейнеры», а merged pr, acknowledgements, число независимых организаций, использующих исправление, и изменения upstream-spec/test suite. именно community criticality, а не attention, лучше превращается в институциональный сигнал ([linux foundation, 2025](https://www.linuxfoundation.org/hubfs/Research%20Reports/lfr_serena_capital_report_082225b.pdf?hsLang=en)).

3. «узкий wedge → большая control-plane история». входная формулировка должна оставаться узкой: zero-trust execution boundary для ai-agent и mcp tool calls. расширение до «безопасности всех агентов» допустимо только после демонстрации wedge. так же действовали codeintegrity и runlayer.

что можно купить или нанять

- бывшего ciso, head of ai security или protocol maintainer не для размещения логотипа, а с конкретным mandate: threat model, design-partner introductions, review публичных findings, участие в diligence.

- независимую лабораторную проверку или red-team reproduction. это превращает собственное заявление «мы нашли 30 cve» в externally reproducible corpus.

- одного сильного enterprise/security seller или forward-deployed engineer, способного довести два-три пилота до именуемых design-partner’ов и договориться о праве раскрыть workflow и результат.

- профильного первого инвестора: небольшой чек от cyber-фонда, бывшего ciso или реального автора/мейнтейнера mcp полезнее длинного списка общих ai-ангелов.

что недоступно

- задним числом получить pedigree уровня sutskever, murati, bret taylor или создателей alphago.

- честно объявить себя автором mcp или владельцем стандарта без реального upstream-вклада.

- использовать 169 тыс. telegram-подписчиков как эквивалент enterprise adoption. аудитория становится венчурной метрикой только после доказанной конверсии: qualified security leads, installs, paid audits, ciso participation или design-partner pipeline. specialist creator investors также требуют монетизации, а не follower count ([inc., 25.02.2025](https://www.inc.com/annabel-burba/vc-firm-slow-ventures-is-investing-60-million-in-content-creators-heres-what-its-looking-for/91152178)).

честный потолок чека

- сейчас, только с активами из вводной: $3–5 млн выглядит доказуемым диапазоном. ближайший очищенный аналог — codeintegrity с $5 млн после публичного exploit-сигнала, accelerator backing и регулируемых пилотов ([codeintegrity, 27.05.2026](https://www.codeintegrity.ai/blog/codeintegrity-seed-round), [geekwire, 27.05.2026](https://www.geekwire.com/2026/codeintegrity-raises-4-8m-to-put-permanent-guardrails-on-unpredictable-ai-agents/)).

- $6–8 млн — stretch, но не фантазия, если до открытия раунда есть: два-три именуемых design-partner’а; независимое воспроизведение ключевых дыр; один recognized cyber/protocol anchor; и production-метрика вроде числа защищенных tool calls, agents или предотвращенных атак.

- выше $8 млн при нулевой выручке потребует хотя бы одного актива, которого сейчас нет: repeat-founder pedigree, автор протокола в основной команде, несколько referenceable enterprise deployments или стратегический инвестор-клиент. runlayer получила $11 млн только при сочетании этих факторов ([techcrunch, 17.11.2025](https://techcrunch.com/2025/11/17/mcp-ai-agent-security-startup-runlayer-launches-with-8-unicorns-11m-from-khoslas-keith-rabois-and-felicis/)).

- рыночный фон допускает выбросы, но не делает их нормой: в 2025 году капитал концентрировался в меньшем числе крупных ai-раундов, тогда как количество сделок на carta упало до шестилетнего минимума ([carta, 18.02.2026](https://carta.com/data/state-of-private-markets-q4-2025/)). медианный американский seed в 2024 году составлял $2,5 млн, поэтому запрос $5–8 млн должен объяснять не только компанию, но и премию к обычному seed ([carta, 15.04.2025](https://carta.com/uk/en/data/top-seven-states-seed-funding-2024/)).

чему нельзя верить

- «восемь unicorn-клиентов» нельзя автоматически читать как восемь платящих production deployments: у runlayer это заявление компании, а techcrunch отдельно отметил, что david soria parra не ответил на запрос подтверждения своей роли ([techcrunch, 17.11.2025](https://techcrunch.com/2025/11/17/mcp-ai-agent-security-startup-runlayer-launches-with-8-unicorns-11m-from-khoslas-keith-rabois-and-felicis/)).

- «платящие клиенты» без names, acv, retention и expansion не равны product-market fit. инвесторы стали сильнее смотреть на net revenue retention, поскольку ранняя ai-выручка часто приходит из тестовых бюджетов и может не пережить пилот ([the information, доступ 24.08.2026](https://www.theinformation.com/articles/why-net-retention-rates-are-all-the-rage-for-ai-investors)).

- аудитория клиента не является аудиторией продукта. 34 млн подписчиков siriusxm или 15 млн клиентов sonos нельзя приписывать sierra; допустима только доля обращений, реально прошедших через агента.

- headline funding может быть cumulative: sierra $110 млн = $25 млн ранее + $85 млн свежих; reflection $130 млн = два раунда; «total raised» и «new money» должны стоять раздельно.

- valuation частной компании часто основана на неназванных источниках. ssi не подтвердила $5 млрд, а источники reflection расходятся между $545 млн и $555 млн ([reuters — ssi, 04.09.2024](https://www.investing.com/news/stock-market-news/exclusiveopenai-cofounder-sutskevers-new-safetyfocused-ai-startup-ssi-raises-1-billion-3600613), [reuters — reflection, 09.10.2025](https://www.investing.com/news/stock-market-news/nvidiabacked-reflection-ai-raises-2-billion-in-funding-boosts-valuation-to-8-billion-4279981)).

- safe valuation cap — не текущая оценка бизнеса. carta прямо предупреждает, что cap является потолком будущей конвертации, а не valuation ([carta, 26.02.2026](https://carta.com/data/newsletter-the-latest-safe-val-cap-trends/)).

- commitment не равен funded cash. по gonka публично подтвержден текст обязательства на $50 млн и отдельное упоминание покупки gnk на $12 млн, но не полный cash close.

- gpu count, nodes, tvl, token holders и volume не доказывают конечный спрос; volume особенно опасен без проверки beneficial ownership и wash trading controls ([doj, 09.10.2024](https://www.justice.gov/usao-ma/pr/eighteen-individuals-and-entities-charged-international-operation-targeting-widespread)).

- github stars, telegram followers, waitlist и product-hunt rank являются вниманием, а не удержанием, зависимостью или бюджетом.

проверенные, но не включенные в восемь строк

- neuraltrust не соответствует условию «слабый adoption»: к seed $20 млн компания называла aireuropa, abanca, iberia и banc sabadell, заявляла миллионы проверяемых agent interactions и удвоение годового arr 2025 уже в первом квартале 2026 года ([neuraltrust, 17.06.2026](https://neuraltrust.ai/news/neuraltrust-raises-20m)).

- onyx, workplace deep-research agent, поднял $10 млн, но уже называл десятки enterprise-клиентов, включая netflix, thales и bitwarden; это не zero-adoption case ([globe newswire, 12.03.2025](https://www.globenewswire.com/news-release/2025/03/12/3041638/0/en/onyx-raises-10m-to-help-teams-find-knowledge-faster-with-open-source-ai-agents.html)).

- onyx security объявил $40 млн одновременно с выходом из stealth, однако доступная основная информация исходит из пресс-релиза компании, а независимо проверяемые данные adoption на момент раунда не найдены ([onyx security, 12.03.2026](https://www.onyx.security/press-release/onyx-security-launches-with-40m-in-funding-to-build-the-secure-ai-control-plane-for-the-agentic-era)).

- sycamore получил $65 млн seed, но механически дублирует pedigree-корзину: former atlassian cto, coatue/lightspeed и длинный список руководителей intel, databricks, openai, okta и palo alto networks; заявленная работа с fortune 500 не сопровождается именами или экономикой ([sycamore, 30.03.2026](https://sycamore.so/press-releases/sycamore-raises-65m-seed/)).

список непроверенного

- точные классы акций, liquidation preferences и side letters у ssi, thinking machines, cognition, runlayer и codeintegrity публично не раскрыты.

- фактическая выручка на дату раунда у всех восьми команд, кроме отдельных более поздних сообщений, не подтверждена аудированными данными.

- личности и contract value ранних платящих клиентов reflection не раскрыты.

- количество, названия и условия пилотов codeintegrity не раскрыты.

- независимое подтверждение всех заявленных runlayer клиентов и фактической роли каждого advisor’а отсутствует.

- факт полного получения gonka всех $50 млн, график перечисления и отношение суммы к покупке gnk на $12 млн не подтверждены.

- targeted search не нашел чистого audience-only раунда от $5 млн в 2024–2026 годах; это отсутствие найденного кейса, а не доказательство его невозможности. найденный специализированный creator-механизм ограничивается чеками $1–3 млн и требует доказанной конверсии/монетизации ([inc., 25.02.2025](https://www.inc.com/annabel-burba/vc-firm-slow-ventures-is-investing-60-million-in-content-creators-heres-what-its-looking-for/91152178)).

- заявленные во вводной 30 cve, 21 000 открытых инстансов, oss-контрибьюшены и telegram-аудитория 169 тыс. не перепроверялись — согласно условию задачи они использованы только как исходная рамка.

это исследовательская оценка механики упаковки, а не инвестиционная, юридическая или финансовая рекомендация.
