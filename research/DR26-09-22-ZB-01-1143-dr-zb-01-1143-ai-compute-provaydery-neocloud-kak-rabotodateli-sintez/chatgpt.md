# DR26-09-22-ZB-01-1143 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

A. Резюме решения — 22.09.2026, 12:32 Europe/London

Вердикт: целиться в AI-compute стоит, но исходная гипотеза верна только наполовину.

1. Дефицит реальный. NVIDIA 26 августа 2026 года описала outlook до FY2028 как supply-constrained, заявила, что её мощности полностью загружены у всех облачных партнёров, и ожидает роста neocloud-мощностей примерно с 3 GW в конце 2025-го до 8 GW в конце 2026-го. Gartner прогнозирует рост AI-optimized IaaS с $21.5 млрд в 2025-м до $42.3 млрд в 2026-м и $66.1 млрд в 2027-м.  
https://investor.nvidia.com/files/content_files/TRANSCRIPT_-NVIDIA-Corp-NVDA-US-Q2-2027-Earnings-Call-26-August-2026-5_00-PM-ET.pdf  
https://www.gartner.com/en/newsroom/press-releases/2026-08-10-gartner-forecasts-worldwide-artificial-intelligence-optimized-iaas-spending-to-grow-96-percent-in-2026

2. «Сверхприбыльны» — неправда для значительной части сектора. CoreWeave, Nebius и IREN быстро растут, но продолжают показывать крупные чистые убытки, капиталоёмкость и долговую нагрузку. Applied Digital получает 96% выручки от трёх клиентов. Это богатый оборотом и финансированием сектор, а не обязательно богатый свободными деньгами.  
https://www.sec.gov/Archives/edgar/data/1769628/000176962826000366/crwv-20260630.htm  
https://www.sec.gov/Archives/edgar/data/1513845/000110465926094568/tm2622968d1_ex99-1.htm  
https://iren.gcs-web.com/news-releases/news-release-details/iren-reports-fy26-results  
https://www.sec.gov/Archives/edgar/data/1144879/000114487926000048/apld-20260531.htm

3. «Им не нужны клиенты» — тоже неправда. Несколько крупнейших провайдеров действительно могут заранее продать почти всю доступную мощность. Но они одновременно нанимают Customer Engineers, FDE, Solutions Architects, Technical Solutions Managers и capacity/product/operations-лидеров. Их проблема сместилась с «найти любой спрос» на «отобрать прибыльные workload’ы, довести клиента до production, правильно распределить GPU, уменьшить зависимость от одного гиганта и не сорвать многомиллиардный контракт».  
https://careers.nebius.com/  
https://modal.com/company  
https://job-boards.greenhouse.io/togetherai?gh_src=General+Catalyst+job+board  
https://fireworks.ai/careers  
https://www.coreweave.com/careers

4. Окно для найма широко открыто сейчас и, с высокой вероятностью, останется сильным до конца 2027 года. На 2028 год уверенность средняя: мощности продолжают вводиться, но спрос также растёт. После 2028-го рассчитывать именно на дефицитную ренту опасно: Gartner ожидает, что стоимость inference для модели с триллионом параметров к 2030 году снизится более чем на 90% относительно 2025-го. Сам сектор останется большим, но ценность переместится от простой перепродажи GPU к электроэнергии, software orchestration, utilisation, reliability и customer delivery.  
https://www.gartner.com/en/newsroom/press-releases/2026-08-10-gartner-forecasts-worldwide-artificial-intelligence-optimized-iaas-spending-to-grow-96-percent-in-2026  
https://www.gartner.com/en/newsroom/press-releases/2026-03-25-gartner-predicts-that-by-2030-performing-inference-on-an-llm-with-1-trillion-parameters-will-cost-genai-providers-over-90-percent-less-than-in-2025

5. Для Антона лучшая дверь — не чистый ML FDE. Приоритетная связка:

- Technical Solutions Manager / Senior Solutions Engineer / Solutions Architect.
- FDE Systems или Forward Deployed AI Strategy, а не FDE Post-Training.
- Capacity & Infrastructure Lead / Compute Strategy & Operations.
- Product Manager по compute, platform или deployments.
- Следующая ступень после входа: Head of Solutions, Head of FDE, Field CTO или GM.

Самые точные совпадения по профилю сейчас: Nebius, Prime Intellect, Fluidstack, Lambda, CoreWeave, Modal, Baseten и Fireworks.

B. Карта рынка

B1. Гиперскейлеры

AWS, Microsoft Azure, Google Cloud и Oracle Cloud остаются крупнейшими универсальными продавцами AI-compute. Это более устойчивые работодатели, но для Антона менее прямая ставка: роли там сильнее сегментированы, а найм проходит через стандартные корпоративные лестницы.

AWS во втором квартале 2026 года получил $42.2 млрд выручки, рост составил 37%; Trainium2 был практически распродан, Trainium3 почти полностью подписан, а значительная часть Trainium4, который появится примерно через 18 месяцев, уже зарезервирована.  
https://ir.aboutamazon.com/news-release/news-release-details/2026/Amazon-com-Announces-Second-Quarter-Results/default.aspx?mode=light  
https://www.aboutamazon.com/news/company-news/amazon-ceo-andy-jassy-amazon-chips-business-q1-2026-earnings

Microsoft Cloud получил $214.4 млрд за FY2026; Azure вырос на 43%, и менеджмент продолжал говорить, что спрос превышает доступное предложение.  
https://www.sec.gov/Archives/edgar/data/789019/000119312526323660/msft-20260630.htm  
https://www.microsoft.com/en-us/investor/earnings/fy-2026-q4/press-release-webcast

Google Cloud имел около $513.9 млрд backlog на конец второго квартала 2026 года.  
https://www.sec.gov/Archives/edgar/data/1652044/000165204426000071/goog-20260630.htm

Oracle в FY2026 получил $34.0 млрд cloud revenue, рост 39%, одновременно неся огромные капитальные затраты на AI-инфраструктуру.  
https://investor.oracle.com/investor-news/news-details/2026/Oracle-Announces-Record-Q4-and-FY-2026-Results-Driven-by-Cloud-Infrastructure--Cloud-Applications/

B2. Neocloud и AI-factory слой

Крупные специализированные игроки: CoreWeave, Lambda, Crusoe, Nebius, Fluidstack, Nscale, IREN и Applied Digital. Gartner ожидает, что neocloud-провайдеры займут 20% AI-cloud рынка объёмом $267 млрд к 2030 году.  
https://www.gartner.com/en/newsroom/press-releases/2026-06-23-gartner-predicts-neocloud-providers-will-capture-20-percent-of-the-267-billion-dollar-ai-cloud-market-by-2030

Внутри группы есть три разных бизнеса:

- Полноценные облака: CoreWeave, Lambda, Nebius.
- Вертикально интегрированные power/data-center/cloud компании: Crusoe, IREN, Applied Digital, Fluidstack, Nscale.
- Компании с контрактной моделью, где один hyperscaler или model lab фактически финансирует конкретный кластер.

Для соискателя это принципиально: «GPU распроданы тысячам клиентов» и «пятилетний контракт с Microsoft оплатил один кластер» — разные риски.

B3. Inference, serving и developer platform

Modal, Baseten, Together AI, Fireworks AI, RunPod, fal и DeepInfra продают не только GPU-часы, но и software layer: deployment, autoscaling, inference optimisation, model APIs, training/post-training и developer experience.

Replicate больше не следует рассматривать как отдельный работодатель категории: компания присоединилась к Cloudflare. Anyscale также надо оценивать вместе с Nscale, поскольку Nscale объявила её приобретение в июле 2026 года.  
https://blog.cloudflare.com/why-replicate-joining-cloudflare/  
https://www.nscale.com/press-releases/nscale-acquires-anyscale

B4. Чиповые компании со своим облаком

Cerebras, Groq и SambaNova продают одновременно собственную архитектуру ускорителя и облачный доступ к ней. Это интересные работодатели для product, customer engineering и platform operations, но риск выше: работник ставит не только на рынок compute, но и на победу конкретной архитектуры.

Tenstorrent также относится к этому слою, но актуальные публичные данные о выручке, концентрации и подходящих Антону вакансиях в этом исследовании не найдены.

B5. Брокеры, marketplaces и distributed compute

Prime Intellect, Vast.ai и SFCompute агрегируют или перепродают мощности разных владельцев. Их преимущество — меньшая зависимость от одного дата-центра и возможность превращать фрагментированный supply в продукт.

SFCompute 10 сентября сообщила о продаже compute на $245 млн, включая два 36-месячных соглашения на B300-кластеры.  
https://sfcompute.com/news/tcv-q3-2026

Vast.ai сообщает более чем о 20 000 GPU, 14 000 платящих клиентов и 13-кратном росте выручки за рассматриваемый период. Это данные самой компании, не аудированная отчётность.  
https://vast.ai/article/ramp-brex-fastest-growing-vendor

B6. Европа и Азия

Европейский long tail включает OVHcloud, Scaleway, Gcore, DataCrunch, Sesterce, Civo и CUDO Compute; часть игроков больше похожа на sovereign cloud или региональный HPC, чем на американский venture-backed neocloud.

В Азии к GPU-first и sovereign AI поставщикам относятся GMI Cloud, GMO Internet, Sakura Internet, KT Cloud, Naver Cloud, NHN Cloud, Ubitus, GreenNode/FPT и Nxera. Каталог Deploy перечисляет 50 GPU-first поставщиков в 14 азиатских странах; это карта для поиска, а не финансовый аудит каждого игрока.  
https://registry.deploy.report/asian-neoclouds

GMI Cloud имеет команды и инфраструктуру в США и Азии и продолжает нанимать, но публичной текущей выручки и концентрации клиентов я не нашёл.  
https://www.gmicloud.ai/en/company/about  
https://www.gmicloud.ai/en/company/career

C. Карта значимых компаний как работодателей

C1. CoreWeave — публичная, Nasdaq: CRWV

Финансы: выручка 2025 года — $5.1 млрд против $1.9 млрд в 2024-м и $229 млн в 2023-м. Чистый убыток за 2025 год — около $1.2 млрд. Во втором квартале 2026 года выручка выросла примерно на 112% год к году, до $2.58 млрд; чистый убыток составил $626 млн. Будущие выплаты principal по долгу на 30 июня 2026 года составляли $35.6 млрд, а чистые процентные расходы за первое полугодие — $1.18 млрд.  
https://www.sec.gov/Archives/edgar/data/1769628/000176962826000104/crwv-20251231.htm  
https://www.sec.gov/Archives/edgar/data/1769628/000176962826000366/crwv-20260630.htm

Концентрация: Microsoft дал 67% выручки 2025 года. OpenAI имеет commitment до $11.9 млрд до октября 2030 года; крупными контрагентами также являются Meta и Microsoft. Backlog на 30 июня 2026 года — $104 млрд.  
https://www.sec.gov/Archives/edgar/data/1769628/000176962826000104/crwv-20251231.htm  
https://investors.coreweave.com/news/news-details/2026/CoreWeave-Reports-Strong-Second-Quarter-2026-Results/default.aspx

Размер и найм: 2 189 сотрудников на конец 2025 года, включая 222 человека за пределами США. Компания активно нанимает в США и Европе. Точное совпадение для Антона — Technical Solutions Manager или Senior Manager TSM: управление стратегическим клиентом, end-to-end delivery, риски, KPI и влияние на продукт.  
https://www.sec.gov/Archives/edgar/data/1769628/000176962826000104/crwv-20251231.htm  
https://www.coreweave.com/careers  
https://www.builtinnyc.com/job/technical-solutions-manager/4519748

Риск работодателя: высокий financial leverage плюс концентрация на Microsoft. Это сильная строка резюме, но не «безрисковая золотая жила».

C2. Nebius — публичная, Nasdaq: NBIS

Финансы: выручка второго квартала 2026 года — $582.3 млн, рост 454%; первого полугодия — $981.3 млн, рост 529%. Скорректированная EBITDA второго квартала — $236.2 млн, но убыток continuing operations — $190.4 млн. Non-current debt вырос с $4.1 млрд на конец 2025 года до $8.5 млрд.  
https://www.sec.gov/Archives/edgar/data/1513845/000110465926094568/tm2622968d1_ex99-1.htm

Спрос: четыре сделки второго квартала имели средний TCV свыше $1 млрд; Nebius заявил, что мог бы продать всю мощность 2027 года уже сейчас, но сохраняет часть capacity. Примерно 70% новых сделок имели предоплату, покрывающую 50–60% capex. Это сильнейшее найденное подтверждение настоящей клиентской очереди, хотя утверждение исходит от самой компании.  
https://www.sec.gov/Archives/edgar/data/1513845/000110465926094568/tm2622968d1_ex99-2.htm

Концентрация: контракты с Microsoft и Meta огромны. Microsoft — до $17.4 млрд до 2031 года; Meta — до $27 млрд, включая $12 млрд выделенной мощности и потенциальный $15-миллиардный backstop.  
https://www.sec.gov/Archives/edgar/data/1513845/000110465926052948/nbis-20251231x20f.htm  
https://www.sec.gov/Archives/edgar/data/1513845/000110465926027886/tm268879d1_6k.htm

Найм: на дату среза карьерная страница показывала 376 вакансий. Среди них Field CTO, Director of Solutions Architecture, Head of Forward Deployed Engineering, Customer Engineer, FDE и Senior Applied AI Solutions Engineer в США и Европе. Это самый богатый набор ролей под переход COO/CTO → field/platform leadership.  
https://careers.nebius.com/

C3. IREN — публичная, Nasdaq: IREN

Финансы: FY2026 AI Cloud revenue — $128.8 млн против $16.4 млн годом ранее; общая выручка — $707 млн. Чистый убыток — $702.6 млн, значительная часть связана с impairment при развороте от bitcoin mining к AI. На конец года компания имела $5.9 млрд cash и сообщала примерно о $14 млрд финансирования и предоплат.  
https://iren.gcs-web.com/news-releases/news-release-details/iren-reports-fy26-results

Спрос: мощность 2026 года в основном распродана; в июле компания заявила, что 85% цели свыше $4 млрд year-end ARR уже законтрактовано. Названы Microsoft, NVIDIA, Perplexity, Figure, Together, Fluidstack, Fireworks, fal и Hume; средневзвешенный срок контрактов — около четырёх лет. Процент выручки крупнейшего клиента не раскрыт.  
https://irisenergy.gcs-web.com/news-releases/news-release-details/iren-signs-28bn-new-customer-contracts-leading-ai-developers

Риск: настоящий спрос сочетается с быстрым корпоративным pivot, impairment и крупным GPU-финансированием. Для Антона интереснее platform operations или GM AI Cloud, чем физическая эксплуатация дата-центров.

C4. Applied Digital — публичная, Nasdaq: APLD

FY2026 revenue — $611.3 млн против $228.6 млн, рост около 167%; operating loss — $236.5 млн. Три крупнейших клиента дали 59%, 25% и 12% выручки, то есть вместе 96%. HPC-бизнес фактически является набором примерно 15-летних арендных контрактов, а не диверсифицированным developer cloud.  
https://www.sec.gov/Archives/edgar/data/1144879/000114487926000048/apld-20260531.htm

Найм есть, но большинство ролей ближе к construction, facilities и data-center operations. Для профиля Антона это ниже приоритетом, если только не откроется GM/VP Operations или customer programme role.  
https://www.applieddigital.com/careers

C5. Lambda — частная, pre-IPO-кандидат

Последний подтверждённый компанией equity-раунд — $1.5 млрд в ноябре 2025 года; также заключено многомиллиардное соглашение с Microsoft. В августе 2026 года Lambda разместила $926 млн GPU-backed senior secured debt под dedicated investment-grade offtaker.  
https://lambda.ai/investors  
https://lambda.ai/blog/lambda-prices-926-million-senior-secured-term-loan-b-facility

Axios сообщал о переговорах по привлечению до $3 млрд при оценке около $12 млрд, но на дату среза это переговоры, а не подтверждённо закрытый раунд.  
https://www.axios.com/newsletters/axios-pro-rata-60ba74b4-004e-4f57-b2e5-496aa335b1f7

Компания нанимает в San Francisco, San Jose и Bellevue в гибридном режиме. В Senior Solution Engineer требуется 8+ лет cloud infrastructure, 4+ года customer-facing и 3+ года hands-on AI/ML workloads; обучение и fine-tuning указаны скорее как усилитель, тогда как GPU, Kubernetes, serving и работа с клиентом являются ядром.  
https://lambda.ai/careers  
https://jobs.ashbyhq.com/Lambda/061cf79b-0e95-4082-8805-5822499441f1

Для Антона это подтверждает, что его интервью было по правильной двери. Недостаток — не отсутствие ML research, а необходимость доказать производственную работу с NVIDIA stack, vLLM/TensorRT-LLM и Kubernetes.

C6. Crusoe — частная

17 сентября 2026 года Crusoe объявила initial close раунда Series F на $3.9 млрд при post-money оценке $30.9 млрд. Компания сообщает более чем о $140 млрд TCV, свыше 6 GW contracted capacity, 1 GW operational, более чем 20-кратном росте cloud bookings и более $100 млн contracted ARR для managed inference.  
https://www.crusoe.ai/resources/newsroom/crusoe-announces-series-f-funding

Штат превышает 1 800 человек в пяти странах; появились офисы в Bellevue и New York, найм идёт по всем рабочим географиям. Аудированную выручку и долю крупнейшего клиента я не нашёл.  
https://www.crusoe.ai/resources/newsroom/crusoe-announces-series-f-funding  
https://www.crusoe.ai/about/careers

Риск: $140 млрд TCV не равны признанной выручке. Сильные стороны Антона здесь — cross-functional cloud delivery, product operations и customer programme leadership.

C7. Nscale — частная, подала S-1

В марте 2026 года Nscale подняла $2 млрд Series C при оценке $14.6 млрд. В феврале она подписала $1.4 млрд GPU-backed debt для кластеров в Великобритании, Норвегии, Португалии и Исландии.  
https://www.nscale.com/press-releases/nscale-series-c  
https://www.nscale.com/press-releases/nscale-signs-1-4bn-delayed-draw-term-loan

30 июля Nscale объявила покупку Anyscale; около 200 сотрудников Anyscale должны перейти в объединённую компанию. 18 сентября была подана регистрационная документация для IPO. В 2025 году Nscale показывала около $33 млн выручки против $19.1 млн годом ранее; долгосрочные take-or-pay contracts составляли основную часть TCV.  
https://www.nscale.com/press-releases/nscale-acquires-anyscale  
https://www.sec.gov/Archives/edgar/data/2110365/000119312526395475/ck0002110365-20260918.htm

Риск: одновременно масштабирование инфраструктуры, долг, IPO и интеграция Anyscale. Это создаёт и возможности для integration/operations leader, и высокий организационный риск.

C8. Fluidstack — частная

Fluidstack сообщила о $830 млн Series A при оценке $7.5 млрд. Публичной выручки, customer concentration и достоверного общего headcount я не нашёл.  
https://fluidstack.io/news/series-a-announcement

На дату среза карьерная страница содержала 237 вакансий. Лучшие совпадения: Strategy & Operations Generalist, Decision Engineering Team Lead, Product Manager Compute Operations, Deployments Program Lead и Technical Program Manager. Это один из лучших прямых вариантов для COO, который умеет кодировать.  
https://fluidstack.io/jobs

C9. Modal — частная

21 мая 2026 года Modal подняла $355 млн Series C при post-money $4.65 млрд. Компания заявила более $300 млн annualized revenue и пятикратный рост с сентября; штат — более 120 человек в New York, San Francisco и Stockholm, также открыт London.  
https://modal.com/blog/modal-series-c  
https://modal.com/company

Вакансии: Customer Engineer, FDE ML, FDE Systems, Solutions Architect, Compute Strategy & Operations Lead, Business Operations Manager и Developer Relations. Это редкий работодатель, где одновременно существуют техническая и операционная двери.  
https://modal.com/company

C10. Baseten — частная

5 февраля 2026 года Baseten подняла $300 млн Series E при оценке $5 млрд. Компания сообщила о 100-кратном росте inference volume за предыдущий год и назвала клиентами Abridge, Clay, Cursor, OpenEvidence, Mercor и Notion. Процент концентрации и текущую выручку не раскрыла.  
https://www.baseten.co/blog/announcing-baseten-s-300m-series-e/

Capacity & Infrastructure Lead с Python/SQL, economics, utilisation и cross-functional work — очень точная роль для Антона. FDE Training значительно сложнее: там требуется глубокая практика training stack.  
https://jobs.ashbyhq.com/baseten/cfb2c11e-20f9-4039-a130-21fc9dc4cb34/  
https://jobs.ashbyhq.com/baseten/11ab2593-6648-4943-ab4a-284fe7e89720

C11. Together AI — частная

1 июля 2026 года Together объявила Series C на $800 млн. Компания называет тысячи клиентов, включая Cognition, Decagon, ElevenLabs, Cursor и Suno, и 500 MW независимо капитализированных commitments; оценка и точная выручка в релизе не раскрыты.  
https://www.together.ai/blog/announcing-our-series-c

На дату среза было 77 вакансий, включая FDE, Solutions Architect, GPU Cluster Customer Success, Infrastructure Strategy & Operations, international cloud sourcing и data-center delivery.  
https://job-boards.greenhouse.io/togetherai?gh_src=General+Catalyst+job+board

Для Антона правильнее Solutions Architect или Customer Success Engineering. Их FDE Inference & Post-Training требует экспертного знания vLLM, TensorRT-LLM/SGLang, KV cache, quantisation и LoRA/SFT/DPO/RLHF; это не entry point без дополнительного доказательства.  
https://job-boards.greenhouse.io/togetherai/jobs/5131941007?gh_src=5a6e93ae7us  
https://job-boards.greenhouse.io/togetherai/jobs/4627491007?gh_src=Long+Journey+Ventures+job+board

C12. Fireworks AI — частная

15 июля 2026 года Fireworks подняла $1.505 млрд Series D при оценке $17.5 млрд и сообщила более чем о $1 млрд ARR и 40 трлн обрабатываемых токенов в сутки. Клиенты включают Cursor и Harvey; аудированная концентрация не раскрыта.  
https://fireworks.ai/blog/series-d-announcement

Карьерная страница показывала около 80 ролей: AI Deployment Strategist, AI Field Engineer, Head of GTM Engineering, Strategic Projects Lead, platform/training product managers и Developer Advocate.  
https://fireworks.ai/careers

AI Field Engineer требует production coding, Python, Kubernetes и понимания inference, serving и fine-tuning. Это возможно для Антона после целевого hands-on проекта, но не только на основании COO-титула.  
https://jobs.ashbyhq.com/fireworks/69739e4f-1ca5-45db-8a70-4309d961c0b7

C13. RunPod — частная

24 июня 2026 года RunPod поднял $100 млн при оценке $1 млрд. Компания заявляет более миллиона разработчиков, свыше 20 млрд inference requests, 90% успешных первых deployment’ов и 85% возвращающихся после deployment разработчиков. Это более диверсифицированная self-serve модель, чем whale-contract neocloud.  
https://www.prnewswire.com/news-releases/runpod-raises-100m-led-by-summit-partners-to-accelerate-the-ai-developer-cloud-302808689.html

Компания remote-first, нанимает в США, Канаде, Европе, Индии и APAC. Однако текущая FDE-вакансия требует глубокой практики training/inference/deployment и прямо указывает отсутствие visa sponsorship.  
https://www.runpod.io/careers  
https://jobs.ashbyhq.com/runpod/24b589d4-1868-4e6e-8e79-9a81c50db282

C14. Prime Intellect — частная

В июле 2026 года Prime Intellect подняла $130 млн Series A; общий объём привлечённого капитала превысил $150 млн, а annualized revenue — $100 млн. Оценка и концентрация не раскрыты.  
https://www.intelcapital.com/prime-intellect-raises-130m-series-a-to-build-the-open-superintelligence-stack/

На карьерной странице около 30 позиций: Head of Compute, Solutions Architect AI Infrastructure, Technical Account Manager, Product Strategy & Revenue Lead и Forward Deployed AI Strategy Lead.  
https://www.primeintellect.ai/careers

FDE AI Strategy Lead —, возможно, лучшее единичное совпадение во всём исследовании. Роль ведёт клиента от discovery и POC до revenue и product strategy; компания допускает путь через early-stage operator, PM или technical GTM, а не только через ML research, и указывает visa sponsorship и relocation.  
https://jobs.ashbyhq.com/PrimeIntellect/a259487d-9599-42e6-ad42-b97a9772f48b

C15. Vast.ai — частная

Vast.ai показывает marketplace-модель с более чем 20 000 GPU и 14 000 платящих клиентов. Публичной аудированной выручки и последней оценки я не нашёл.  
https://vast.ai/article/ramp-brex-fastest-growing-vendor

На странице вакансий были Head of Sales, Technical Product Manager Infrastructure, Developer Relations и systems roles. Диапазоны: Technical PM $170–240 тыс., DevRel $160–200 тыс., systems engineering до $320 тыс.; работа преимущественно onsite в SF или LA.  
https://vast.ai/jobs

C16. Cerebras — публичная после IPO в мае 2026 года

Во втором квартале 2026 года выручка Cerebras составила $180.1 млн, рост 74%; cloud revenue выросла на 281%. При IPO компания получила около $6.2 млрд net proceeds.  
https://www.sec.gov/Archives/edgar/data/2021728/000162828026056357/cbrs-20260630.htm

Три крупнейших клиента дали 34%, 32% и 10% квартальной выручки. Компания имеет большое соглашение с OpenAI на 750 MW committed capacity и ещё 1.25 GW optional. Это перспективный, но высококонцентрированный работодатель.  
https://www.sec.gov/Archives/edgar/data/2021728/000162828026056357/cbrs-20260630.htm

C17. Groq и SambaNova

Groq 22 июня 2026 года привлёк $650 млн для масштабирования inference cloud. Но после сделки по лицензированию технологий NVIDIA за $20 млрд и ухода части senior team работодатель находится в периоде стратегической перестройки.  
https://groq.com/newsroom/groq-raises-usd650m-to-scale-its-ai-inference-cloud-business  
https://www.axios.com/2026/05/28/groq-650-million-nvidia

SambaNova 8 июля объявила first close $1 млрд Series F при оценке $11 млрд и продолжает нанимать. Финансовую концентрацию и текущую выручку компания не раскрыла.  
https://sambanova.ai/press/sambanova-completes-first-close-of-1b-financing-at-11b-valuation?hs_amp=true  
https://sambanova.ai/company/careers

C18. fal, DeepInfra, Replicate

fal в декабре 2025 года подняла $140 млн Series D и сообщала о команде примерно из 70 человек. Надёжную актуальную выручку и подходящую Антону открытую руководящую роль я не нашёл.  
https://blog.fal.ai/our-series-d-scaling-fal/

DeepInfra 4 мая 2026 года подняла $107 млн Series B, сообщив о трёхкратном росте выручки с начала года и почти 5 трлн токенов в неделю. Точную роль-вход для Антона и customer concentration не нашёл.  
https://www.globenewswire.com/news-release/2026/05/04/3286977/0/en/DeepInfra-Closes-107M-Series-B-to-Power-Production-Scale-AI-Inference.html

Replicate как отдельную цель исключаю из shortlist из-за присоединения к Cloudflare.  
https://blog.cloudflare.com/why-replicate-joining-cloudflare/

D. Где настоящая очередь, а где один гигант

Настоящий избыток диверсифицированного спроса:

- RunPod: более миллиона разработчиков, self-serve, без minimum commitment. Это наиболее убедительная массовая клиентская база.  
https://www.prnewswire.com/news-releases/runpod-raises-100m-led-by-summit-partners-to-accelerate-the-ai-developer-cloud-302808689.html

- Vast.ai: более 14 000 платящих клиентов и marketplace supply. Данные не аудированы, но модель структурно меньше зависит от одного whale.  
https://vast.ai/article/ramp-brex-fastest-growing-vendor

- Modal: более $300 млн annualized revenue и разные production workloads, от inference до agents и biotech. Процент крупнейшего клиента не раскрыт.  
https://modal.com/blog/modal-series-c

- Baseten, Together и Fireworks показывают много известных AI-native клиентов, но наличие логотипов не доказывает низкую концентрацию; точные проценты не опубликованы.  
https://www.baseten.co/blog/announcing-baseten-s-300m-series-e/  
https://www.together.ai/blog/announcing-our-series-c  
https://fireworks.ai/blog/series-d-announcement

Спрос есть, но экономика завязана на нескольких гигантов:

- CoreWeave: Microsoft — 67% выручки 2025 года.  
https://www.sec.gov/Archives/edgar/data/1769628/000176962826000104/crwv-20251231.htm

- Nebius: огромные Microsoft и Meta commitments; публичной разбивки выручки за квартал между ними нет.  
https://www.sec.gov/Archives/edgar/data/1513845/000110465926052948/nbis-20251231x20f.htm  
https://www.sec.gov/Archives/edgar/data/1513845/000110465926027886/tm268879d1_6k.htm

- Applied Digital: 96% выручки от трёх клиентов.  
https://www.sec.gov/Archives/edgar/data/1144879/000114487926000048/apld-20260531.htm

- Cerebras: 76% квартальной выручки от трёх клиентов.  
https://www.sec.gov/Archives/edgar/data/2021728/000162828026056357/cbrs-20260630.htm

- Lambda: GPU-backed debt связан с dedicated investment-grade offtaker, а Microsoft имеет многомиллиардное соглашение. Точный процент концентрации не опубликован.  
https://lambda.ai/blog/lambda-prices-926-million-senior-secured-term-loan-b-facility  
https://lambda.ai/investors

Вывод: «очередь клиентов» подтверждается, но чаще это очередь за конкретными будущими кластерами и power capacity. Работодатель всё равно нуждается в людях, которые превратят контракт и железо в запущенный production workload.

E. Карта ролей

E1. FDE — главная точка входа?

Частично подтверждено.

FDE — повторяющаяся и заметная дверь у Modal, Baseten, Nebius, RunPod, Together, Fireworks и Prime Intellect. Но в общем количестве вакансий сектора FDE не доминирует: у вертикальных neocloud намного больше infrastructure engineering, data-center delivery, finance, construction, operations и capacity roles.  
https://modal.com/company  
https://careers.nebius.com/  
https://fireworks.ai/careers  
https://www.primeintellect.ai/careers  
https://job-boards.greenhouse.io/togetherai?gh_src=General+Catalyst+job+board

Более точный тезис: доминирует не буквальное название FDE, а семейство customer-to-production ролей:

- Forward Deployed Engineer.
- Customer Engineer.
- AI Field Engineer.
- Deployment Strategist.
- Solutions Architect / Solution Engineer.
- Technical Solutions Manager.
- Technical Account Manager.

E2. Что требуют и каков шанс Антона

Проценты ниже — моя оценка соответствия публичным JD, а не статистика работодателей.

1. Technical Solutions Manager / Senior Solution Engineer

Требования: customer discovery, архитектура, delivery сложных программ, управление риском, cloud/GPU/Kubernetes, умение влиять на product и engineering. Training models обычно не является ядром.

Шанс Антона после точечного обновления GPU/serving знаний: 65–80%. Это его лучшая текущая дверь.  
https://www.builtinnyc.com/job/technical-solutions-manager/4519748  
https://jobs.ashbyhq.com/Lambda/061cf79b-0e95-4082-8805-5822499441f1

2. FDE Systems / Customer Engineer

Требования: Python/C++, distributed systems, deployment, debugging, production ownership и работа рядом с клиентом. Глубокая ML-наука не обязательна, но нужна свободная ориентация в inference stack.

Шанс: 55–70%.  
https://modal.com/company  
https://careers.nebius.com/

3. FDE ML / Post-Training

Требования: PyTorch, vLLM/SGLang/TensorRT-LLM, distributed training, fine-tuning, LoRA/SFT/DPO/RLHF, profiling и model optimisation.

Шанс сейчас: 20–35%. После собственного production benchmark и нескольких недель плотной практики — выше, но исследовательский пробел одним резюме не закрыть.  
https://job-boards.greenhouse.io/togetherai/jobs/5131941007?gh_src=5a6e93ae7us  
https://jobs.ashbyhq.com/modal/9fadb51f-ce11-41b1-84d5-470e66cc8ee9/

4. Applied AI / AI Field Engineer

Требования: production code, POC→deployment, API/Kubernetes/cloud, работа с latency, throughput и reliability. Обычно требуется working knowledge ML, но не публикации по ML.

Шанс: 40–60%, если показать живой deployment.  
https://jobs.ashbyhq.com/fireworks/69739e4f-1ca5-45db-8a70-4309d961c0b7

5. Capacity & Infrastructure Lead / Compute Strategy & Operations

Требования: Python/SQL, cloud bills, unit economics, procurement, utilisation, forecasting, cross-functional execution.

Шанс: 70–85%. Это лучшее соединение его COO, CTO и программирования.  
https://jobs.ashbyhq.com/baseten/cfb2c11e-20f9-4039-a130-21fc9dc4cb34/  
https://modal.com/company  
https://fluidstack.io/jobs

6. Product Manager Compute / Platform / Deployments

Требования: законченный продуктовый цикл, техническая глубина, клиентская discovery, приоритизация reliability/cost/performance и способность говорить с infrastructure engineers.

Шанс: 60–75%, если в резюме показать не титул CTO, а конкретные метрики «идея → продукт → выручка → эксплуатация».  
https://vast.ai/jobs  
https://fluidstack.io/jobs  
https://fireworks.ai/careers

7. Head of FDE / Director Solutions / Field CTO

Требования: построить функцию, нанять команду, вести крупнейших клиентов, создать повторяемые playbooks и одновременно оставаться технически убедительным.

Шанс: 35–55%. У Антона подходит управленческий цикл, но работодателю потребуется доказательство именно GPU/AI-infrastructure credibility. Nebius сейчас предлагает наиболее прямые варианты.  
https://careers.nebius.com/  
https://www.builtinnyc.com/job/manager-technical-solutions-manager/7406052

8. VP Operations / GM / Chief of Staff

Требования сильно зависят от стадии. Такие роли встречаются реже, часто заполняются через инвесторов и сеть, а не массовую карьерную страницу.

Шанс на холодную заявку: 25–45%. Через CEO/founder/investor outreach у Prime Intellect, Fluidstack, Nscale или небольшого marketplace шанс выше.

9. Developer Relations

Требования: технический контент, публичные demos, community, конференции, хороший код.

Шанс: 40–60%, но только при наличии публичного technical evidence. Сам по себе COO-профиль не заменяет developer audience.  
https://modal.com/company  
https://vast.ai/jobs

F. Компенсации

Диапазоны нельзя сравнивать механически: где-то указан base, где-то OTE, а equity почти всегда отдельно.

- Lambda Senior Solution Engineer: San Francisco/San Jose $251–355 тыс.; Bellevue $226–302 тыс.  
https://jobs.ashbyhq.com/Lambda/061cf79b-0e95-4082-8805-5822499441f1

- Together FDE Inference & Post-Training: $270–300 тыс. OTE плюс equity.  
https://job-boards.greenhouse.io/togetherai/jobs/5131941007?gh_src=5a6e93ae7us

- Together Solutions Architect: $250–300 тыс. OTE плюс equity.  
https://job-boards.greenhouse.io/togetherai/jobs/4627491007?gh_src=Long+Journey+Ventures+job+board

- Modal FDE ML: $180–250 тыс. base.  
https://jobs.ashbyhq.com/modal/9fadb51f-ce11-41b1-84d5-470e66cc8ee9/

- Modal Business Operations Manager: $140–190 тыс. base.  
https://jobs.ashbyhq.com/Modal/225b43d8-ef14-4082-b16e-005c74b42957/

- Modal Regional Director: около $400 тыс.; это enterprise sales leadership, а не COO-роль.  
https://jobs.ashbyhq.com/modal/0d4c15af-afa7-4430-b3db-dd8258950aec

- CoreWeave Technical Solutions Manager: опубликованный base $153–204 тыс.; встречающийся агрегированный диапазон $165–220 тыс. включает различия по локациям.  
https://www.builtinnyc.com/job/technical-solutions-manager/4519748

- CoreWeave Manager/Senior Manager TSM: $165–242 тыс. base; на той же странице отображалась более свежая senior manager позиция $207–275 тыс.  
https://www.builtinnyc.com/job/manager-technical-solutions-manager/7406052

- Nebius FDE Ecosystem: $179.5–224.3 тыс. base по зеркалу вакансии.  
https://builtin.com/job/forward-deployed-engineer-ecosystem/9258985

- Baseten Capacity & Infrastructure Lead: $220–260 тыс. base.  
https://jobs.ashbyhq.com/baseten/cfb2c11e-20f9-4039-a130-21fc9dc4cb34/

- RunPod FDE: $100–180 тыс.  
https://jobs.ashbyhq.com/runpod/24b589d4-1868-4e6e-8e79-9a81c50db282

- Vast.ai Technical Product Manager Infrastructure: $170–240 тыс.; DevRel $160–200 тыс.; senior systems roles до $320 тыс.  
https://vast.ai/jobs

Реалистичный target для Антона в Bay Area: $220–300 тыс. cash/OTE плюс equity для senior solutions, capacity/product или field role. $300 тыс.+ достижимо в сильном FDE/SA либо leadership, но там техническая проверка будет жёстче.

G. Виза, релокация и география

Активная O-1 — преимущество, но не свободно переносимое разрешение на работу. При смене работодателя новый работодатель или агент должен подать новую Form I-129; для O-категории нельзя предполагать H-1B-style portability без одобрения. Это надо проверить с иммиграционным юристом до формулировки в оффере.  
https://www.uscis.gov/sites/default/files/document/forms/i-129oinstr-feerule.pdf  
https://www.uscis.gov/sites/default/files/document/foia/Nonimmigrant_Services.pdf

Практические выводы:

- Prime Intellect прямо указывает visa sponsorship и relocation — лучший прозрачный вариант.  
https://jobs.ashbyhq.com/PrimeIntellect/a259487d-9599-42e6-ad42-b97a9772f48b

- RunPod FDE прямо говорит, что sponsorship нет.  
https://jobs.ashbyhq.com/runpod/24b589d4-1868-4e6e-8e79-9a81c50db282

- Together предлагает Bay Area relocation support, но blanket sponsorship policy я не нашёл. В форме есть вопрос о sponsorship.  
https://www.together.ai/careers  
https://job-boards.greenhouse.io/togetherai/jobs/4627491007?gh_src=Long+Journey+Ventures+job+board

- Nebius имеет глобальные команды и отдельные immigration-функции, но гарантии sponsorship для каждой американской позиции нет. ЕС-паспорт усиливает варианты Amsterdam/Finland/France/Germany, если американская петиция задержится.  
https://careers.nebius.com/

- Modal имеет офисы в SF, New York, Stockholm и London; часть customer/FDE ролей доступна сразу в нескольких офисах.  
https://modal.com/company

- Lambda работает гибридно в SF/San Jose/Bellevue; для Антона Palo Alto — заметное операционное преимущество.  
https://lambda.ai/careers

В резюме и первом письме формулировка должна быть точной: “Based in Palo Alto; active O-1 status; new-employer petition structure understood; EU citizen; relocation-ready.” Не писать “no sponsorship required”, пока это не подтвердил адвокат.

H. Ранжированный shortlist под Антона

1. Nebius

Роль-вход: Field CTO; Director Solutions Architecture; Head of FDE Ecosystem; Senior Applied AI Solutions Engineer.

Почему №1: единственная крупная компания с текущей лестницей от customer engineer до Head of FDE и Field CTO одновременно; глобальная география; очень быстрый рост.

Риск: Microsoft/Meta concentration, удвоившийся долг и delivery risk многомиллиардных контрактов.  
https://careers.nebius.com/  
https://www.sec.gov/Archives/edgar/data/1513845/000110465926094568/tm2622968d1_ex99-1.htm

2. Prime Intellect

Роль-вход: Forward Deployed AI Strategy Lead; Product Strategy & Revenue Lead; Solutions Architect AI Infrastructure.

Почему: роль прямо соединяет discovery, код, POC, product strategy и revenue; ML research — лишь один из возможных путей, а не обязательный фильтр. Есть sponsorship и relocation.

Риск: ранняя стадия и меньший масштаб организации.  
https://www.primeintellect.ai/careers  
https://jobs.ashbyhq.com/PrimeIntellect/a259487d-9599-42e6-ad42-b97a9772f48b

3. Fluidstack

Роль-вход: Decision Engineering Team Lead; Product Manager Compute Operations; Strategy & Operations; Deployments Program Lead.

Почему: самое чистое совпадение с «COO, который пишет код и понимает продуктовый цикл».

Риск: публичная финансовая прозрачность слабая, культура подчёркнуто интенсивная.  
https://fluidstack.io/jobs  
https://fluidstack.io/news/series-a-announcement

4. Lambda

Роль-вход: Senior Solution Engineer; adjacent Principal Product/Customer Infrastructure role.

Почему: Антон уже прошёл интервью по правильной функции; локально рядом; компания ценит то, что человек построил и какой impact дал, а не только pedigree.

Риск: ML-infrastructure bar выше его текущего доказанного профиля; частная финансовая отчётность непрозрачна, присутствует GPU-backed debt.  
https://lambda.ai/careers  
https://jobs.ashbyhq.com/Lambda/061cf79b-0e95-4082-8805-5822499441f1  
https://lambda.ai/blog/lambda-prices-926-million-senior-secured-term-loan-b-facility

5. Fireworks AI

Роль-вход: AI Deployment Strategist; AI Field Engineer Enterprise; Strategic Projects Lead; Product Manager Platform.

Почему: более $1 млрд ARR, широкий field/product найм и близость San Mateo/SF.

Риск: для AI Field Engineer нужна доказанная практика production inference.  
https://fireworks.ai/blog/series-d-announcement  
https://fireworks.ai/careers

6. CoreWeave

Роль-вход: Technical Solutions Manager; Senior Manager TSM; Solution Specialist Infrastructure; Chief of Staff к технической функции.

Почему: роль TSM почти дословно описывает customer delivery, operations, risk и product feedback из опыта Антона.

Риск: $35.6 млрд будущего principal, крупные процентные расходы и 67% выручки от Microsoft.  
https://www.builtinnyc.com/job/technical-solutions-manager/4519748  
https://www.sec.gov/Archives/edgar/data/1769628/000176962826000366/crwv-20260630.htm

7. Modal

Роль-вход: FDE Systems; Customer Engineer; Solutions Architect; Compute Strategy & Operations Lead.

Почему: быстрый рост, компактная команда, открыты сразу инженерная и операционная двери.

Риск: высокая техническая планка, большинство ключевых ролей привязано к офису.  
https://modal.com/blog/modal-series-c  
https://modal.com/company

8. Baseten

Роль-вход: Capacity & Infrastructure Lead; затем FDE или platform product.

Почему: Capacity Lead идеально соединяет Python/SQL, unit economics, utilisation и cross-functional execution.

Риск: FDE Training требует глубокой ML-практики и не должен быть первой заявкой.  
https://www.baseten.co/blog/announcing-baseten-s-300m-series-e/  
https://jobs.ashbyhq.com/baseten/cfb2c11e-20f9-4039-a130-21fc9dc4cb34/

9. Together AI

Роль-вход: Solutions Architect; Customer Success Engineer GPU Cluster; Infrastructure Strategy & Operations.

Почему: 77 вакансий, высокий cash compensation, полная цепочка training→inference.

Риск: FDE Post-Training слишком специализирован для текущего профиля Антона.  
https://job-boards.greenhouse.io/togetherai?gh_src=General+Catalyst+job+board  
https://job-boards.greenhouse.io/togetherai/jobs/5131941007?gh_src=5a6e93ae7us

10. RunPod

Роль-вход: FDE, product или DevRel, если соответствующая вакансия остаётся открытой.

Почему: диверсифицированная миллионная developer base и remote-first организация.

Риск: текущая FDE требует deep ML deployment и не предоставляет sponsorship.  
https://www.runpod.io/careers  
https://jobs.ashbyhq.com/runpod/24b589d4-1868-4e6e-8e79-9a81c50db282

11. Crusoe

Роль-вход: Cloud Customer Strategy; Product Operations; Strategic Program/Delivery.

Почему: крупнейший свежий private round, 1 800+ сотрудников и вертикальная инфраструктура от энергии до inference.

Риск: headline TCV огромен, но текущая признанная выручка и customer concentration не опубликованы.  
https://www.crusoe.ai/resources/newsroom/crusoe-announces-series-f-funding  
https://www.crusoe.ai/about/careers

12. Nscale / Anyscale

Роль-вход: Customer Engineering; Platform Product; Integration/Operations leader.

Почему: объединение физического AI cloud с Ray/Anyscale software stack создаёт потребность в людях, способных соединить продукт, операции и клиентов.

Риск: IPO, интеграция приобретения и GPU-backed debt происходят одновременно.  
https://www.nscale.com/press-releases/nscale-acquires-anyscale  
https://www.nscale.com/press-releases/nscale-signs-1-4bn-delayed-draw-term-loan

13. Vast.ai

Роль-вход: Technical Product Manager Infrastructure; Head of Sales; Developer Relations.

Почему: marketplace с диверсифицированной базой и прямые Bay Area вакансии.

Риск: меньше масштаб и прозрачность, чем у публичных neocloud.  
https://vast.ai/jobs  
https://vast.ai/article/ramp-brex-fastest-growing-vendor

14. Cerebras

Роль-вход: Cloud Customer Engineering; Product/Platform Operations.

Почему: быстро растущая cloud revenue и публичная финансовая отчётность.

Риск: 76% квартальной выручки от трёх клиентов плюс ставка на одну альтернативную архитектуру.  
https://www.sec.gov/Archives/edgar/data/2021728/000162828026056357/cbrs-20260630.htm

15. IREN

Роль-вход: AI Cloud Platform Operations; Customer Program Lead; будущий GM направления.

Почему: capacity 2026 года в основном распродана, контракты примерно на четыре года и быстрое строительство AI-бизнеса.

Риск: крупный pivot, impairment, высокая потребность в финансировании и пока небольшой исторический AI-cloud revenue относительно будущих commitments.  
https://iren.gcs-web.com/news-releases/news-release-details/iren-reports-fy26-results  
https://irisenergy.gcs-web.com/news-releases/news-release-details/iren-signs-28bn-new-customer-contracts-leading-ai-developers

I. Кто опасен как работодатель

Красная зона:

- CoreWeave: высокий долг и концентрация Microsoft.
- Applied Digital: 96% выручки от трёх клиентов.
- Nscale: долг плюс IPO плюс интеграция Anyscale.
- Groq: стратегическая перестройка после сделки с NVIDIA.
- Cerebras: высокая концентрация и platform/chip risk.

Жёлтая зона:

- Nebius: спрос доказан, но масштаб исполнения и долговая нагрузка растут быстрее истории компании.
- IREN: спрос доказан, но это быстрый переход из bitcoin-инфраструктуры.
- Lambda: сильная позиция, но private-company opacity и GPU-backed leverage.
- Crusoe и Fluidstack: большие commitments и финансирование, но недостаточно публичных данных о выручке и концентрации.

Относительно более диверсифицированные по модели:

- RunPod и Vast.ai — массовый marketplace/self-serve.
- Modal — разные виды workloads и клиентов.
- Baseten, Fireworks и Together — много client logos, но отсутствие раскрытой концентрации не позволяет присвоить им низкий риск уверенно.

J. Что изменилось за последние три месяца

- 22 июня: Groq привлёк $650 млн.  
https://groq.com/newsroom/groq-raises-usd650m-to-scale-its-ai-inference-cloud-business

- 24 июня: RunPod привлёк $100 млн при оценке $1 млрд и заявил более миллиона разработчиков.  
https://www.prnewswire.com/news-releases/runpod-raises-100m-led-by-summit-partners-to-accelerate-the-ai-developer-cloud-302808689.html

- 1 июля: Together AI объявила $800 млн Series C.  
https://www.together.ai/blog/announcing-our-series-c

- 8 июля: SambaNova объявила first close $1 млрд при оценке $11 млрд.  
https://sambanova.ai/press/sambanova-completes-first-close-of-1b-financing-at-11b-valuation?hs_amp=true

- 15 июля: Fireworks подняла $1.505 млрд при оценке $17.5 млрд и сообщила более $1 млрд ARR.  
https://fireworks.ai/blog/series-d-announcement

- Июль: Prime Intellect подняла $130 млн и сообщила более $100 млн annualized revenue.  
https://www.intelcapital.com/prime-intellect-raises-130m-series-a-to-build-the-open-superintelligence-stack/

- 20 июля: Fluidstack раскрыла $830 млн Series A при оценке $7.5 млрд.  
https://fluidstack.io/news/series-a-announcement

- 30 июля: Nscale объявила приобретение Anyscale.  
https://www.nscale.com/press-releases/nscale-acquires-anyscale

- Август: CoreWeave показала квартальный рост выручки около 112% и backlog $104 млрд.  
https://investors.coreweave.com/news/news-details/2026/CoreWeave-Reports-Strong-Second-Quarter-2026-Results/default.aspx

- Август: Nebius показала рост квартальной выручки 454% и одновременно рост долга.  
https://www.sec.gov/Archives/edgar/data/1513845/000110465926094568/tm2622968d1_ex99-1.htm

- 12 августа: Lambda разместила $926 млн GPU-backed debt.  
https://lambda.ai/blog/lambda-prices-926-million-senior-secured-term-loan-b-facility

- 26 августа: NVIDIA сохранила supply-constrained outlook до FY2028.  
https://investor.nvidia.com/files/content_files/TRANSCRIPT_-NVIDIA-Corp-NVDA-US-Q2-2027-Earnings-Call-26-August-2026-5_00-PM-ET.pdf

- 10 сентября: SFCompute сообщил о $245 млн проданного compute.  
https://sfcompute.com/news/tcv-q3-2026

- 17 сентября: Crusoe объявила $3.9 млрд Series F при оценке $30.9 млрд и более $140 млрд TCV.  
https://www.crusoe.ai/resources/newsroom/crusoe-announces-series-f-funding

- 18 сентября: Nscale подала IPO-регистрацию.  
https://www.sec.gov/Archives/edgar/data/2110365/000119312526395475/ck0002110365-20260918.htm

Итого за три месяца: окно не закрывается. Капитал и найм ускорились, но одновременно резко выросли leverage, концентрация и execution risk.

K. Что делать на этой неделе

1. Сделать две версии позиционирования.

Версия A: “Customer-to-production technical leader” для Solutions/FDE Systems/Field CTO.

Версия B: “Compute economics and operations leader who codes” для Capacity, Platform Product и Operations.

В обеих убрать абстрактное «COO/CTO». На первой странице показать три измеримых цепочки: проблема → что лично построил → production → выручка/экономия/надёжность.

2. За 1–2 дня собрать один технический proof.

Развернуть open model через vLLM или SGLang, измерить latency, throughput, utilisation и стоимость на двух GPU-конфигурациях, добавить failure/recovery сценарий. Именно эти понятия повторяются в вакансиях Together, Lambda, Fireworks и Modal.  
https://job-boards.greenhouse.io/togetherai/jobs/5131941007?gh_src=5a6e93ae7us  
https://jobs.ashbyhq.com/Lambda/061cf79b-0e95-4082-8805-5822499441f1  
https://jobs.ashbyhq.com/fireworks/69739e4f-1ca5-45db-8a70-4309d961c0b7  
https://modal.com/company

3. Податься сначала в восемь наиболее точных дверей.

- Prime Intellect — Forward Deployed AI Strategy Lead.
- Fluidstack — Decision Engineering Team Lead или Compute Operations PM.
- Nebius — Field CTO / Director Solutions / Head FDE.
- Baseten — Capacity & Infrastructure Lead.
- CoreWeave — Technical Solutions Manager.
- Modal — FDE Systems или Compute Strategy & Operations.
- Fireworks — Deployment Strategist / Strategic Projects.
- Lambda — повторный заход через интервьюера на Senior Solution Engineer или соседнюю customer/product роль.

4. Не ограничиваться формой заявки.

Писать руководителю функции с одним конкретным тезисом: «Я умею брать технически сырой продукт, превращать его в повторяемый production delivery и отвечать за экономику, клиента и код». Приложить один релевантный кейс и benchmark, не общий рассказ о карьере.

Для Lambda лучше запросить у уже знакомого интервьюера калибровку: какой именно пробел помешал и какая соседняя роль закрывает его сильными сторонами. Карьерная страница прямо говорит, что компания оценивает созданные вещи, impact, техническую глубину и способность работать в хаосе.  
https://lambda.ai/careers

5. До обсуждения оффера подготовить иммиграционную формулировку.

Получить у юриста одностраничный ответ: кто подаёт новую O-1 petition, можно ли использовать agent structure и когда разрешён start date. Это снимет неопределённость у рекрутера, не создавая ложного заявления “no sponsorship required”.  
https://www.uscis.gov/sites/default/files/document/forms/i-129oinstr-feerule.pdf

L. Финальный вердикт

Окно открыто. Практический горизонт дефицитного бума — минимум следующие 12–24 месяца; до конца 2027 года уверенность высокая, на 2028 год средняя. После этого AI-compute останется большим рынком, но работодатели, живущие только на дефиците GPU, станут опаснее.

Антону не надо притворяться ML researcher. Его сильная позиция: технический оператор, который умеет сам писать код, разговаривать с клиентом, принимать продуктовые решения и доводить инфраструктуру от хаоса до выручки.

Главная ошибка — подаваться только на VP/COO или только на глубоко ML-ориентированный FDE. Самый рациональный мост: Senior Solutions / Technical Solutions Manager / FDE Systems / Compute Strategy & Operations, затем быстрый переход в Head of Solutions, Head of FDE или Field CTO.

🧒 Простыми словами: GPU сейчас как хорошие квартиры в городе — желающих больше, чем свободных мест. Но владельцы этих «квартир» часто купили их в долг и сдали одному очень большому арендатору. Антону лучше идти туда не как учёному, который изобретает новый мозг, а как человеку, который умеет заселить клиента, починить дом, посчитать экономику и превратить всё это в работающий бизнес.

Конец среза: 22.09.2026, 12:32 Europe/London.
