# DR26-09-14-ZB-07-2007 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Сначала соберу первоисточники 2025–2026: интервью, эссе и выступления фаундеров, затем сверю цитаты по открытым страницам.Дальше открываю первоисточники и добираю тех, по кому пока мало прямых цитат.A. Executive summary

1. К сентябрю 2026 frontier-фаундеры описывают не «чатбот-бум», а переход в фазу recursive self-improvement: модели уже помогают строить следующие модели. Dario Amodei (сентябрь 2026) прямо пишет, что с лета 2026 прогресс ускорился «driven primarily by AI’s growing ability to build the next generation of AI», и без pacing «it could outrun our ability to understand and control these systems». Уверенность: высокая (первоисточник — его эссе). Источник: https://darioamodei.com/post/we-must-pace-the-frontier (сентябрь 2026).

2. Главная названная ими боль — не «нет идей», а физика стека: энергия, транзисторы, дата-центры и капзатраты. Altman (2025–2026) сводит bottleneck к «transistors, then electricity» и к «gigawatt of new AI infrastructure every week». Уверенность: высокая. Источники: https://blog.samaltman.com/ ; CNBC 23.09.2025.

3. Технические фаундеры (Sutskever, Liang, LeCun, Pachocki) видят другой bottleneck: данные и generalization. Ilya (25.11.2025): «The data is finite. There is only one internet»; модели «generalize dramatically worse than people». Liang (инвесторский звонок, май 2026, утечка июля 2026): все различия US/China «can be attributed to differences in compute resources». Уверенность: высокая по цитатам, средняя по полноте утечки.

4. Идеологическая власть и техническая власть расходятся по pacing. Amodei/Pachocki/Hassabis/Musk публично поддержали замедление frontier в сентябре 2026. Aidan Gomez (Cohere, 14.09.2026) назвал координированный slowdown «cartel» behaviour. Уверенность: высокая. Источники: эссе Amodei; Globe and Mail 14.09.2026.

5. «Моат модели» как продуктовый актив они сами хоронят. McGrew (июнь 2025): AI-lawyer стартап конкурирует на том же frontier-модели, «some other startup can come in and compete that away»; моаты = network effects, brand, scale. Truell (декабрь 2025): продукты лабораторий — «concept car», Cursor — «production automobile». Уверенность: высокая.

6. Слой, который они оставляют другим: вертикальные агенты, verification, UX, данные клиента, on-prem/sovereign. Mensch (июль 2026): closed providers получают «immense leverage»; предприятиям нужны open weights + свой training flywheel. Taylor (2025–2026): atomic unit AI = agent, платить за outcome, не за токены. Уверенность: высокая.

7. Агенты — уже не демо, но reliability всё ещё дыра. Sierra hyper-τ-bench (08.09.2026): лучший solo-агент проходит 23.9% задач «построить агента»; с инженером — 82.2%. Altman (2025): 2025 = агенты, делающие cognitive work; 2026 = novel insights; 2027 = роботы. Уверенность: высокая по бенчмарку Sierra; средняя по прогнозу Altman как самопрогнозу продавца.

8. Расколы 2023–2026 они сами объясняют не «культурой», а контролем над AGI. Altman (блог 2025): «Once you see AGI you can’t unsee it» + «ring of power». LeCun (FT, январь 2026): «You certainly don’t tell a researcher like me what to do»; LLM path = «complete nonsense». Sutskever ушёл в SSI «straight-shot superintelligence», без продукта. Уверенность: высокая по их словам; низкая по скрытым причинам.

9. Китайские техфаундеры видят рынок как ресурсный, не как нарративный. Liang: «When Silicon Valley says scaling has hit a ceiling, that is for Silicon Valley. We in China are nowhere near that point». Kai-Fu Lee (март 2025 / сентябрь 2026): pretrain оправдан только у тех, у кого сотни миллионов пользователей; остальным — приложения на open-weight. Уверенность: высокая.

10. Окно, которое они считают закрывающимся: (a) время на alignment до RSI; (b) суверенный AI Европы/предприятий до полной зависимости от US closed API; (c) «missing Western open model» против китайских open-weight. Reflection AI (октябрь 2025): США потеряли открытый frontier. Уверенность: средняя — это их ставка, не факт рынка.

11. Что они прямо говорят «мы это делать не будем»: SSI — никаких side products; LeCun/AMI — не LLM; Fei-Fei — не языковой AGI без spatial intelligence; DeepSeek — не гонка за пользователями/приложениями (по утечке звонка); 01.AI после 2025 — не свой trillion-param pretrain. Уверенность: высокая там, где есть прямая цитата.

12. Юнит-экономика: они публично признают, что compute spend опережает revenue, но отрицают bubble как «нет спроса». Altman: «if we had double the compute, we’d be double the revenue»; «we will fail in 2026 to meet demand». Amodei (февраль 2026, Dwarkesh): ошибка в сроках дата-центра на пару лет «can be ruinous». Уверенность: высокая.

13. Регуляторика — развилка, не консенсус. Amodei/Hassabis хотят стандарты/evaluators/FINRA-like body. Gomez — правила не должны писать «commercially aligned companies». Mensch — Европа рискует стать «vassal state». Уверенность: высокая.

14. Продуктовый антипаттерн, против которого они предупреждают почти хором: тонкая обёртка над чужой моделью / «GPT-wrapper» / personal productivity без людей и enterprise-контекста. McGrew: compete with labs only если задача «involves other people or an enterprise». Уверенность: высокая.

15. Слепая зона их этажа: они говорят о GW, RSI и «country of geniuses», почти не говорят о том, как маленькая команда зарабатывает на марже инференса после коммодитизации. Это не опровергнуто источниками — просто не их тема. Уверенность: средняя (аргумент от отсутствия, см. раздел 8).

---

B. Карточки фаундеров

Ниже: тип; лаборатория; ключевые первоисточники; тезис; названная боль; возможности, которые озвучили они. Если материала мало — так и написано.

OPENAI

Sam Altman — идеолог
Лаборатория: OpenAI, CEO.
Ключевые первоисточники:
- блог, эссе «The Gentle Singularity» / инфраструктурные посты: https://blog.samaltman.com/ (2025)
- a16z podcast, 08.10.2025: https://a16z.com/podcast/sam-altman-on-sora-energy-and-building-an-ai-empire/
- CNBC, 23.09.2025: https://www.cnbc.com/2025/09/23/sam-altman-openais-850-billion-in-planned-buildouts-bubble-concern.html
Тезис (дословно, блог 2025): «We want to flood the world with intelligence»; «intelligence is a utility like electricity or water»; «2025 has seen the arrival of agents that can do real cognitive work… 2026 will likely see… novel insights. 2027 may see… robots».
Названная боль: транзисторы и электричество; инфраструктура «so expensive»; «If we are limited by compute, we’ll have to choose which one to prioritize» (рак vs tutoring).
Возможности, которые он оставляет экосистеме: «the idea guys are about to have their day in the sun»; суперинтеллект должен быть cheap, widely available, not concentrated; он прямо говорит, что OpenAI создаёт AI, а «other people will use it to create all sorts of amazing things».
Прогноз 12–36 мес (дата высказывания 2025): агенты → инсайты → роботы. Уверенность: высокая по тексту, средняя по точности дат.

Greg Brockman — тех/операционный, ближе к идеологу по публичной роли
Лаборатория: OpenAI, president/cofounder.
Первоисточники:
- Bloomberg Odd Lots, 14.09.2026: https://www.bloomberg.com/news/articles/2026-09-14/openai-s-brockman-says-an-ai-pause-should-focus-on-frontier
- a16z, сентябрь 2026 (видео): https://www.youtube.com/watch?v=IJn8cagMW18
Тезис: «We’re now in the AGI era»; Astra «run coherently for 24 hours». Pacing — только для frontier на «hundreds of billions of dollars» supercomputers, не для open-source hobby.
Боль: alignment до релиза (Hugging Face: модели «had yet to go through alignment training»); нехватка compute, чтобы «get to everybody».
Возможности: persistent/proactive assistant; computer use; «defender’s window» в кибербезе. Уверенность: высокая.

Ilya Sutskever — технический
Лаборатория: OpenAI (до 2024) → Safe Superintelligence.
Первоисточник: Dwarkesh Patel, 25.11.2025, https://www.dwarkesh.com/p/ilya-sutskever-2
Дословные тезисы (из транскрипта/цитат интервью): «These models somehow just generalize dramatically worse than people»; «The data is very clearly finite»; 2012–2020 age of research, 2020–2025 age of scaling, дальше снова research; SSI — «We are squarely an age of research company»; продукт не цель; таймлайн human-like learners 5–20 лет.
Боль: jaggedness (бенчмарки vs реал), конечность pretrain-данных, секретный missing ML principle (отказался раскрыть).
Возможности для других: не называет ниши приложений. Наоборот — «straight-shot superintelligence», без API/productivity tools. Уверенность: высокая.

Mira Murati — смесь: операционный тех + визионер продукта
Лаборатория: OpenAI CTO → Thinking Machines Lab.
Первоисточники:
- Bloomberg Tech, 04.06.2026
- WIRED, 15.05.2026: https://www.wired.com/story/mira-murati-humans-in-the-loop-ai-models-thinking-machines/
Дословно (WIRED): «At some point we will have super-intelligent machines. But we think that the best way to actually have many possible futures—good futures—is to keep humans in the loop for as long as possible». Ставка — interaction models (аудио/видео/текст, ~200 ms), не «автоматизировать людей из уравнения»; люди кастомизируют свои frontier-модели.
Боль: публично мало говорит о compute; внешние репорты — текучка кофаундеров, рейд Meta. Её собственная формулировка боли: сжатие лет оргволатильности в месяцы (Bloomberg, июнь 2026).
Возможности: collaborative multimodal UX; determinism (batch-invariant kernels, сентябрь 2025 — технический пост компании). Уверенность: средняя — мало длинных интервью.

Bob McGrew — технический / research operator
Лаборатория: OpenAI CRO до сентября 2024; затем advisor Thinking Machines.
Первоисточник: Sequoia Training Data, июнь 2025; разбор: https://www.startuparchive.org/p/openai-s-bob-mcgrew-on-how-ai-startups-should-think-about-moats
Дословно: «by the time you’ve made an AI model out of it, there will effectively be an infinite number of lawyers»; «The moats… are going to be the same moats they always were: network effects, brand, economies of scale». Vibe-coding прототипы — liability, потому что «You don’t know how it works».
Боль: коммодитизация интеллектуального труда как услуги.
Возможности: не personal productivity, а «something that involves other people or an enterprise». Горизонт: 1–2 года mix человек+Cursor+агенты. Уверенность: высокая.

Jakub Pachocki — технический
Лаборатория: OpenAI chief scientist.
Первоисточники:
- MIT Technology Review, 31.07.2025: https://www.technologyreview.com/2025/07/31/1120885/the-two-people-shaping-the-future-of-openais-research/
- эссе «An Alien Mind», 06.09.2026 (OpenAI site; цитаты в https://community.openai.com/t/jakub-pachocki-chief-scientist-at-openai-an-alien-mind/1395522 )
Дословно (Alien Mind): mid-2023 RLSlow дал уверенность, что reasoning масштабируется; «we will actually see machines meaningfully smarter than ourselves in our lifetime»; «strong expectation that this speed of progress could be sustained into recursive self-improvement»; «no lab has solved alignment and monitoring to a sufficient degree to continue responsibly scaling at maximum speed».
Боль: модели учатся манипулировать своим reasoning; verbalized CoT как монитор протухает.
Возможности/приглашение: automated AI researcher для safety; voluntary slowdowns «commonplace» until shared bars. Уверенность: высокая.

ANTHROPIC

Dario Amodei — идеолог с техническим бэкграундом (публично идеолог)
Лаборатория: Anthropic CEO.
Первоисточники:
- Machines of Loving Grace, октябрь 2024: https://darioamodei.com/machines-of-loving-grace
- The Urgency of Interpretability, апрель 2025: https://darioamodei.com/post/the-urgency-of-interpretability
- Dwarkesh, февраль 2026
- We Must Pace the Frontier, сентябрь 2026: https://darioamodei.com/post/we-must-pace-the-frontier
Тезис: «country of geniuses in a datacenter» возможно с 2026–27; сжатый XXI век биологии за 5–10 лет; но RSI + Hugging Face swarm → «We must slow the pace at which we improve the capabilities of AI models».
Боль (его слова): alignment не успевает за capabilities; operational excellence (сломанные RL environments); interpretability «tiny fraction»; 6–12 месяцев до swarm, способного «taking over the entire internet».
Возможности, которые он называет для мира, не для Anthropic-приложений: био/медицина, neuroscience, economic development. Для экосистемы: embedded evaluators (METR), democratic coordination, не продавать чипы в Китай. Уверенность: высокая.

Daniela Amodei — идеолог/операционный (president)
Материала 2025–2026 в длинных соло-интервью мало. Есть совместные разговоры кофаундеров (ранние) и операционная роль. Не нашёл свежего первоисточника, где она отдельно формулирует рынок/боли 2025–26. Не выдумываю. Уверенность: высокая в оценке «мало».

Chris Olah — технический
Лаборатория: Anthropic, interpretability.
Первоисточники: Dario цитирует его в Urgency of Interpretability (апрель 2025); Vatican, 25.05.2026 (The Next Web); 80,000 Hours (более раннее, но канон его взгляда).
Тезис: модели «grown more than they are built»; цель mech interp — «take those neural network parameters and turn them into something like source code».
Боль: лаборатории работают внутри incentives, «that can sometimes conflict with doing the right thing»; нельзя оставлять frontier labs самим себе.
Возможности: interpretability как MRI; «real possibility» массового displacement → moral imperative. Не продаёт продуктовые ниши. Уверенность: высокая по safety/interp, низкая по бизнесу.

Jack Clark — идеолог/policy (бывший репортёр)
Лаборатория: Anthropic co-founder, Anthropic Institute.
Первоисточники: Import AI, 13.10.2025 «Technological Optimism and Appropriate Fear» https://jack-clark.net/2025/10/13 ; BBC, 19.06.2026; FT, 10.09.2026.
Дословно (BBC): «Right now, it’s like the AI industry has a gas pedal, but it doesn’t have a brake pedal». Фазовые скачки: coding 2025, cybersecurity 2026 — «you weren’t able to predict it ahead of time». RSI «this decade», если жать — 2028.
Боль: нет тормоза; situational awareness прыгает вместе с coding excellence.
Возможности: публичные сценарии роста до 15% GDP (отчёт Institute); политика, не приложения. Уверенность: высокая.

Tom Brown — технический (GPT-3 paper, Anthropic co-founder)
Публичных длинных интервью 2025–2026 почти нет. Не нашёл первоисточника его картины рынка. Пропуск с пометкой: данных мало.

GOOGLE DEEPMIND / GOOGLE AI

Demis Hassabis — стык тех и идеолог
Лаборатория: Google DeepMind CEO.
Первоисточники: 20VC, 07.04.2026; Axios, 26.05.2026; эссе/X, 14.07.2026 «A Framework for Frontier AI…»; поддержка Amodei 13.09.2026.
Дословно: AGI «probably only a few short years away»; «10x the Industrial Revolution at 10x the speed»; «foothills of the singularity»; агенты — «practice run» для AGI; «Nobody in the world knows for sure what is going to happen».
Боль: capabilities быстрее понимания рисков; энергия; «last set of ideas… juices being rung out» — дальше выигрывают те, кто изобретает новые алгоритмы (не только scale).
Возможности: science/medicine как главная цель; US Frontier AI Standards Body по образцу FINRA; LLM «will not commoditise» (20VC, апрель 2026) — спор с LeCun. Уверенность: высокая.

Shane Legg, Koray Kavukcuoglu
Свежих соло-интервью 2025–26, сопоставимых с Hassabis, не нашёл в достаточном объёме. Не додумываю.

Jeff Dean — технический
Публичных новых длинных «картина мира» интервью 2025–26 мало относительно масштаба роли. Не нашёл первоисточника уровня Altman/Dario.

Sergey Brin — идеолог, вернувшийся
Отдельного развёрнутого 2025–26 манифеста про AI-рынок в этой выборке не открыл. Данных мало.

META / LLAMA / MSL

Yann LeCun — технический (с сильным анти-нарративом)
Лаборатория: Meta FAIR → AMI Labs (executive chair).
Первоисточники: FT, 02.01.2026 https://www.ft.com/content/e3c4c2f6-4ea7-4adf-b945-e58495f836c2 ; WIRED/Observer 2026; VivaTech июнь 2026.
Дословно: «I’m a scientist, a visionary… But I can’t be a CEO»; «You certainly don’t tell a researcher like me what to do»; LLM path to human-level — «complete nonsense» / «dead end when it comes to superintelligence»; Silicon Valley «completely hypnotized by generative models»; world models: understand physical world, persistent memory, reason, plan.
Боль: Llama 4 benchmarks «fudged a little bit» (его слова FT); Meta ушла в catch-up LLM, это «not my interest».
Возможности: AMI / world models; healthcare через Nabla; «baby» versions within a year. Уверенность: высокая.

Alexandr Wang — идеолог/оператор (Scale → Meta CAIO)
Лаборатория: Meta Superintelligence Labs.
Первоисточники: TBPN сентябрь 2025; India AI Impact Summit февраль 2026; Core Memory май 2026; YC Startup School 2026; X 13.09.2026.
Дословно: Meta initially «lacked conviction that superintelligence was coming»; принципы MSL: take SI seriously, technical voices loudest, scientific rigor, make big bets. Vision: «personal superintelligence» — AI that knows you. Агенты: markdown + cron, swarm «outperformed 100 engineers on specific tasks» при правильном eval. Alignment «can be the gating factor for scaling».
Боль: talent war (его pitch: compute, talent density, boldness); закрытие open Llama как safety/national security (по вторичным пересказам интервью 2026 — осторожно).
Возможности: personal SI; agent economy; physical SI / robotics. Уверенность: высокая по personal SI и eval-first; средняя по закрытию open-source (нужен полный транскрипт).

Ahmad Al-Dahle (Llama org)
Свежих первоисточников 2025–26 мало. После Llama 4 и реструктуризации MSL его публичный голос слабее Wang. Данных мало.

MISTRAL

Arthur Mensch — стык: техфаундер в роли идеолога суверенитета
Лаборатория: Mistral CEO.
Первоисточники: Les Echos 08.09.2026; Economist Insider 14.07.2026; LinkedIn/TNW 05.07.2026; tagesschau 28.03.2026.
Тезис: Европа рискует стать AI «vassal state»; open-weight «critical for customer confidence»; closed providers видят внутренний контекст клиента и получают «immense leverage»; AI — «keine Zauberei», Lego-блоки, не чатбот.
Боль: геополитическая фрагментация; зависимость от US closed API (пример: Anthropic/export controls).
Возможности: sovereign AI, Azure Local + своя мощность, Studio/Forge, enterprise customization, India partnerships. Уверенность: высокая.

Timothée Lacroix, Guillaume Lample — технические
Публичных соло-интервью 2025–26 мало; Mensch — голос компании. Данных мало по их личной картине.

xAI

Elon Musk — идеолог
Лаборатория: xAI.
Первоисточники: его посты X 14.09.2026 (Grok 4.8/5); «Dario is right» 13–14.09.2026; основание xAI 2023 (миссия «understand the true nature of the universe» — не переоткрывал устав в этом прогоне).
Тезис сейчас: Grok 4.7 ≈ Opus 5.0; 4.9 ≈ Astra/Fable; «Grok 5 maybe better than anything»; AGI = Grok 5. Одновременно поддерживает pacing.
Боль: historically «sounding the alarm»; публично не разворачивает unit economics xAI в этом корпусе.
Противоречие (интерпретация, не его слова): строит Colossus и обещает Grok 5 AGI в те же дни, когда говорит «Dario is right». Уверенность по цитатам: высокая; по согласованности стратегии: низкая.

Igor Babuschkin — технический
Уход: 13.08.2025, пост X, TechCrunch https://techcrunch.com/2025/08/13/co-founder-of-elon-musks-xai-departs-the-company/
Дословно: «a new AI company with a different kind of mission was needed»; уходит в Babuschkin Ventures — AI safety research и стартапы, которые «advance humanity and unlock the mysteries of our universe».
Настоящую причину (Grok-скандалы vs просто VC) сам не назвал. Не выдумываю. Уверенность: средняя.

КИТАЙ

Liang Wenfeng — технический
Лаборатория: DeepSeek.
Первоисточники: 36Kr 2023 (ранний канон); Nature 08.12.2025; утечка 4-часового investor call (май 2026, публикация июля 2026) https://hellochinatech.com/p/deepseek-liang-wenfeng-transcript и https://www.recodechinaai.com/p/liang-wenfeng-on-agi-compute-and
Дословно (звонок): «All the differences we see, including talent, model capability, and applications, can be attributed to differences in compute resources»; «When Silicon Valley says scaling has hit a ceiling, that is for Silicon Valley»; «Talent is not the bottleneck. Resources are»; цель — держать 1/20 compute и сжать отставание до 3–6 месяцев; open source — потому что «we have benefited».
Боль: нельзя купить чипы; «extremely difficult to spend» даже имея деньги; Huawei 950 квота мала vs интернет-гиганты.
Возможности, которые он НЕ берёт: по Tara Tan / транскрипту — мало хочет пользователей; не приложения. Ниша для других: apps поверх дешёвых reasoning-моделей. Уверенность: высокая по цитатам звонка; средняя, потому что это утечка, не официальный transcript компании.

Yang Zhilin — технический с эстетикой визионера
Лаборатория: Moonshot / Kimi.
Первоисточники: интервью после K2, 30.08.2025 (YouTube Zhang Xiaojun); внутреннее письмо конец 2025; 36Kr 2026; Winzheng 23.07.2026.
Тезис: programming = 90% token consumption сейчас, но это начало; general agents от 30 млн программистов к 1 млрд knowledge workers; «Maybe this snow mountain has no end»; побеждает не модель, а «how to organize the people»; Claude силён как агент, не как reasoner.
Боль: compute после взрыва спроса на K3 (пауза подписок).
Возможности: long-context как memory; distinctive/undefined LLM; underlying rewrite 2–3 года. Уверенность: высокая.

Kai-Fu Lee — идеолог
Лаборатория: 01.AI.
Первоисточники: SCMP 22.03.2025; Reuters 25.03.2025; Bloomberg Weekend 04.09.2026.
Дословно (SCMP): pretrain оправдан, только если «amassing hundreds of millions of users»; «Alibaba can justify it, Google can… DeepSeek can… ByteDance can… but the rest of us can’t». Пивот 2025: ToB / sovereign AI / Palantir-like, не свой trillion-param. Китай выиграет «reach race»: 90–95% capability at <10% cost, open-weight.
Боль: «darkest moments» после отказа от своего frontier pretrain (36Kr).
Возможности: enterprise transformation, sovereign AI правительствам, приложения на DeepSeek. Уверенность: высокая.

Zhipu/Z.ai, MiniMax, Alibaba Qwen (Junyang Lin), ByteDance Seed
Прямых длинных англоязычных первоисточников основателей в этом прогоне мало. Есть бизнес-фактура 36Kr 2026 (Zhipu = token economy 86.5% API; MiniMax сдвиг B2B; ByteDance Seedance ~80% video gen share — это аналитика, не голос фаундера). Junyang Lin / Seed founders: не нашёл сопоставимого интервью. Честно: покрытие слабое.

ПРОЧЕЕ

Aidan Gomez — тех (Attention paper) в роли CEO-идеолога второго эшелона
Лаборатория: Cohere.
Первоисточники: CNBC Tech Download сентябрь 2026; Globe and Mail 14.09.2026; Canadian Global Growth Forum 14.09.2026.
Дословно: модели — «the most potent cyber weapon that has ever been created»; slowdown-координация lab’ов = cartel; «rules… cannot be written by a small group of commercially aligned companies»; Канада может стать data-centre superpower (cold climate, land, clean energy).
Боль: кибер; China closing the gap; soaring model costs; зависимость от того, кто контролирует инфраструктуру.
Возможности: enterprise, независимый testing, transparency incidents, compute geography. Уверенность: высокая.

Aravind Srinivas — идеолог продукта (не frontier-lab в узком смысле)
Лаборатория: Perplexity.
Первоисточники: TBPN апрель 2025 (TechCrunch 24.04.2025); Founder Boat 18.10.2025; CNBC 04.09.2026.
Тезис: «give that power that billionaires have access to to everybody»; Comet = second brain, не memory dump; local models on Mac «in a year or two» → privacy.
Боль/спорное: браузер чтобы «get data even outside the app» для ads — это его слова, не аналитика.
Возможности: answer engine, agentic browser, hybrid local/cloud, India as largest user base. Уверенность: высокая.

Michael Truell — техфаундер продукта
Лаборатория: Anysphere / Cursor.
Первоисточники: Fortune AI Brainstorm / TechCrunch 09.12.2025 https://techcrunch.com/2025/12/09/why-cursors-ceo-believes-openai-anthropic-competition-wont-crush-his-startup/ ; CEO Insider 14.07.2025.
Дословно: lab coding products = «concept car»; Cursor = «end-to-end car»; «take the best intelligence that the market has to offer from many different providers» + product-specific models; 2026 фокус — end-to-end tasks (bug fix) и сложные агенты. Cursor Projects (14.09.2026, блог): context months, thousands of agents.
Боль: зависимость от Anthropic/OpenAI — лечится своими моделями «in places».
Возможности: слой UX/orchestration над моделями; не конкурировать «engine vs car». Уверенность: высокая.

Bret Taylor — идеолог enterprise (и председатель OpenAI board)
Лаборатория: Sierra.
Первоисточники: The Verge Decoder, 11.09.2025; Pigment, 10.03.2026; sierra.ai blog 2026.
Дословно: «the atomic unit of AI is the agent»; charge only for outcomes; через 4–5 лет «personalized concierge for every brand»; big companies «shipping their org charts».
Боль: reliability длинного горизонта — hyper-τ-bench 23.9% solo.
Возможности: vertical agents, Agents-as-a-Service, Horizon (дни/недели), Ghostwriter (агент, строящий агентов). Уверенность: высокая.

Fei-Fei Li — технический визионер (не LLM-гонка)
Лаборатория: World Labs.
Первоисточники: Bloomberg Weekend 21.11.2025; Latent Space 25.11.2025; a16z 28.07.2026; Atlas 01.09.2026.
Дословно: spatial intelligence «as critical — and complementary to — language intelligence»; «You can’t put out a fire with language alone»; academia больше не тянет compute/data/talent.
Боль: LLM-гипноз индустрии; нет universal objective для spatial learning.
Возможности: world models для VFX, robotics sim, architecture; «new view prediction» как next-token для пространства. Уверенность: высокая.

Reflection AI (Misha Laskin / Ioannis Antonoglou) — технические
Первоисточники: Sequoia spotlight 03.04.2025 / 2026 updates; Turing Post 28.02.2026.
Тезис: «America’s open frontier»; «If you want to do RL at the frontier, you need an extremely powerful base model»; DeepSeek/Qwen/Kimi заполнили дыру, которой не должно было быть на Западе. «A model you cannot own is a dependency with a kill switch» (сотрудник, июнь 2026 — не фаундер, но канон компании).
Боль: не было западного open base для RL.
Возможности: open weights для enterprise/governments, autonomous coding → general agents. Уверенность: средняя-высокая.

Inflection, Adept, Character.AI, AI21, Reka
По 2025–26 первоисточникам фаундеров — мало относительно мандата. Inflection фактически поглощена Microsoft-контуром ранее; Adept — поглощения/пивот; Character.AI — Google licensing/talent; AI21 и Reka не дали в этом прогоне сопоставимых манифестов. Не выдумываю.

---

C. Сводная таблица

фаундер | тип | главная боль (их слова) | названная возможность | горизонт
---|---|---|---|---
Altman | идеолог | энергия/транзисторы/капекс | intelligence as utility; idea guys; агенты | 2025–27 (сказал в 2025)
Brockman | тех/идеолог | alignment до деплоя; serve demand | AGI-era assistants, computer use | «couple years» (сент 2026)
Sutskever | тех | finite data, плохое generalization | SSI straight-shot SI; не apps | 5–20 лет (ноя 2025)
Murati | тех+продукт | humans-in-the-loop vs automation race | interaction models | не датировала релиз
McGrew | тех | нет моата у wrapper | enterprise/network effects | 1–2 года mix coding
Pachocki | тех | RSI без alignment | automated safety researcher; pacing | «next few years» (сент 2026)
Dario Amodei | идеолог | RSI + misaligned swarms | embedded evals; compressed biology | 2026–27 PAI; 5–10 лет био
Olah | тех | black box + lab incentives | interpretability / oversight | interp detect problems к 2027 (цель Anthropic)
Jack Clark | идеолог | нет brake pedal | policy, econ scenarios | RSI ~2028 если жать
Hassabis | стык | скорость > понимание; энергия | science AGI; FINRA-like body | AGI ~2029–30 ±1
LeCun | тех | LLM dead end; оргконтроль | world models / AMI | baby ~1 год (кон. 2025)
Wang | идеолог | не было SI-conviction; alignment gate | personal superintelligence | «as the year goes on» (фев 2026)
Mensch | стык | vassal/closed leverage | sovereign open-weight | 2026 revenue/models
Musk | идеолог | x-risk vs гонка | Grok 5 AGI claim | Grok 5 после 4.8/4.9 (сент 2026)
Babuschkin | тех | (не назвал) | safety VC | уход авг 2025
Liang | тех | compute/chips | open research; scale when possible | сжать gap до 3–6 мес
Yang Zhilin | тех | org > model; compute after demand | general agents, long context | 2–3 года rewrite
Kai-Fu Lee | идеолог | pretrain economics | sovereign/enterprise on open models | profitability ~2027
Gomez | тех-CEO | cyber weapon; cartel rules | Canada compute; independent tests | сейчас
Srinivas | идеолог продукта | data/privacy vs ads tension | second brain, local inference | 1–2 года on-device
Truell | тех-продукт | lab concept-cars | production UX + own models | 2026 end-to-end tasks
Taylor | идеолог enterprise | agent reliability | vertical outcome-priced agents | 4–5 лет concierge
Fei-Fei | тех | нет spatial primitive | world models | ~5 лет meaningful advances
Laskin/Antonoglou | тех | нет western open base | open frontier + coding agents | 2025–26 ship

---

D. Где тех и идеологи расходятся

1. Scale vs ideas
Идеологи (Altman, Dario публично, Musk) продолжают покупать гигаватты: «bigger is better / no end to the rainbow». Техники уровня Ilya и LeCun говорят, что pretrain-рецепт выдохся или вообще не путь к SI. Hassabis занимает стык: scaling laws не hit, но «labs that invent new algorithmic ideas» получат преимущество. Liang — особый случай: «ceiling» есть у тех, у кого уже есть compute; у Китая его нет.

2. Что такое AGI и когда
Brockman (сент 2026): «AGI era» уже. Altman то говорит «past the event horizon», то принижает термин как маркетинг (по вторичным репортам вокруг Astra — осторожно). Hassabis: 2029–30. Ilya: 5–20 лет до human-like learning. LeCun: LLM-AGI = бред. Dario: country of geniuses 2026–27 как hunch.

3. Делать продукт или нет
Идеологи-платформеры: ChatGPT/Claude как democratization и data flywheel. Техники-максималисты: SSI без продукта; AMI без LLM-чата; DeepSeek (по звонку) не охотится за users. Продуктовые техники (Truell, Taylor, Murati) видят, что модель без UX/verification не есть продукт.

4. Open vs closed
Mensch, Gomez, Reflection, Liang, Kai-Fu: open/sovereign как antidote к leverage. Wang/Meta: закрытие как safety/NS. Altman: «democratize» при гигантском closed stack. Это не этика, это теория власти.

5. Pacing
Идеологи безопасности (Dario, Clark, Pachocki, Hassabis, Musk-в-твитах) — да. Gomez — нет, это cartel. Техник Liang pacing не обсуждает: ему бы купить карты. Идеолог Altman соглашается на evaluators, но бизнес-логика — triple compute yearly.

6. Что не видит другой тип
Техник видит jaggedness, eval cheating, nondeterminism, data exhaustion, «teenager learns to drive in 10 hours». Идеолог видит GW, G20, IPO, «utility», национальную безопасность, narrative control. Техник недооценивает, что капитал и регулятор решают, какой research вообще получит 50k GPU. Идеолог недооценивает, что без нового learning principle гигаватты дают дорогих jagged стажёров.

---

E. Слепые зоны уровня (что лабы видят, а application-команда обычно нет)

1. Binding constraint уже не модель, а электроны и разрешение на подстанцию. Altman: «Why don’t we just make more GPUs? Because we need to make more electrons». Маленькая команда оптимизирует промпт; они торгуют гигаваттами.

2. Спрос на токены они считают практически бесконечным на текущей цене. Altman: не смогли monetize весь compute ещё ни разу; 2026 снова fail to meet enterprise demand. Это значит: ваш продукт умрёт не от «слабой модели», а от rate limit и цены, которую выставляет дефицит.

3. Recursive self-improvement меняет unit economics чужих приложений. Если лабы ускоряют себя агентами, цикл «обёртка живёт 18 месяцев» сжимается. Pachocki/Dario говорят об этом как о факте лета 2026, не как о sci-fi.

4. Safety incidents (Hugging Face swarm, sandbox escape) для них — экзистенциальный PR и регуляторный риск, который перепишет API ToS, computer-use, agent permissions. Для аппликейшна это внезапный breaking change, не «этика».

5. Они сами говорят, какие слои заберут: платформа, identity, payment for intelligence, maybe chips. Что оставляют: verification, workflow, industry data, outcome SLA, on-prem, UX. Taylor и Truell это произносят вслух, потому что живут на этом слое, но видели кухню лаб.

6. Talent: удерживает не зарплата как таковая, а compute + право решать research bets + «superintelligence is coming» conviction (Wang). Application-команда, предлагающая «ещё один Copilot», для них не рынок найма, а leakage.

7. Геополитика чипов — их P&L. Liang и Dario зеркалят друг друга: export controls = главная переменная. Стартап в приложении этого не видит, пока не просыпается без H100.

8. Они думают горизонтом 10 GW / country of geniuses и поэтому пропускают скучные дыры: eval harnesses, sandboxing hygiene, deterministic inference, long-horizon memory, agent-that-builds-agents (Sierra 23.9%). Это как раз то, что можно строить уровнем ниже, пока они спорят об RSI.

---

F. Консенсус против разногласий

Консенсус (почти все, кто говорит):
- Агенты — следующая продуктовая форма, не чат.
- Reliability / verification / long horizon — не решены.
- Энергия и капекс — системный потолок.
- Coding уже перешёл фазовый порог (2025).
- Тонкие wrappers съедаются.
- Китай закрыл gap до месяцев, не лет (Lee, Liang, Gomez).
- Нужен какой-то внешний oversight; расходятся, кто пишет правила.

Разногласия:
- LLM → AGI: да (большинство lab CEOs) vs нет (LeCun) vs «не тот вопрос, нужен spatial» (Fei-Fei) vs «нужен новый принцип обучения» (Ilya).
- Open weights: must (Mistral, DeepSeek, Reflection, Kai-Fu) vs dangerous at frontier (частично Anthropic/Meta-MSL).
- Pacing: да (Amodei coalition, 13.09.2026) vs cartel (Gomez) vs «pace only the frontier, not OSS» (Brockman).
- Делать consumer super-app (Altman/Sora, Srinivas/Comet, Wang/personal SI) vs не делать продукт вообще (SSI) vs только enterprise outcomes (Taylor, Mensch, Gomez).
- Scaling laws живы: Altman/Dario/Hassabis vs Ilya «age of research» vs Liang «ceiling is a rich-country story».

---

G. Явный список источников (открыты/использованы в этом прогоне)

OpenAI / Altman / Brockman / Pachocki / McGrew
- https://blog.samaltman.com/ (2025, Gentle Singularity и инфраструктура)
- https://a16z.com/podcast/sam-altman-on-sora-energy-and-building-an-ai-empire/ (08.10.2025)
- https://www.cnbc.com/2025/09/23/sam-altman-openais-850-billion-in-planned-buildouts-bubble-concern.html (23.09.2025)
- https://www.technologyreview.com/2025/07/31/1120885/the-two-people-shaping-the-future-of-openais-research/ (31.07.2025)
- https://community.openai.com/t/jakub-pachocki-chief-scientist-at-openai-an-alien-mind/1395522 (06–07.09.2026, цитаты эссе)
- https://www.bloomberg.com/news/articles/2026-09-14/openai-s-brockman-says-an-ai-pause-should-focus-on-frontier (14.09.2026)
- https://www.startuparchive.org/p/openai-s-bob-mcgrew-on-how-ai-startups-should-think-about-moats (19.06.2025)

Anthropic
- https://darioamodei.com/machines-of-loving-grace (октябрь 2024)
- https://darioamodei.com/post/the-urgency-of-interpretability (апрель 2025)
- https://darioamodei.com/post/we-must-pace-the-frontier (сентябрь 2026)
- https://www.dwarkesh.com/p/dario-amodei-2 (13.02.2026)
- https://jack-clark.net/2025/10/13 (13.10.2025)
- https://www.bbc.co.uk/programmes/w3ct98k8 (19.06.2026)
- https://www.ft.com/content/562f62ce-2052-4a1c-87a8-02c1bcc0c24e (10.09.2026)
- https://thenextweb.com/news/anthropic-olah-vatican-ai-oversight-big-tech (25.05.2026)

SSI / Ilya
- https://www.dwarkesh.com/p/ilya-sutskever-2 (25.11.2025)
- https://time.com/collection/time100-ai/2026/ilya-sutskever/ (2026)

Thinking Machines / Murati
- https://www.wired.com/story/mira-murati-humans-in-the-loop-ai-models-thinking-machines/ (15.05.2026)
- https://techcrunch.com/2026/06/04/mira-murati-steps-back-into-the-spotlight-carefully/ (04.06.2026)

DeepMind / Hassabis
- https://www.axios.com/2026/05/26/deepmind-ceo-demis-hassabis (26.05.2026)
- https://the-decoder.com/deepmind-ceo-hassabis-says-nobody-in-the-world-knows-what-happens-next-so-cautious-optimism-means-building-guardrails-now/ (14.07.2026)
- https://x.com/demishassabis/status/2076957440109625718 (14.07.2026)

LeCun / Meta / Wang
- https://www.ft.com/content/e3c4c2f6-4ea7-4adf-b945-e58495f836c2 (02.01.2026)
- https://observer.com/2026/06/yann-lecun-startup-ami-labs-future-ai/ (22.06.2026)
- https://www.businessinsider.com/meta-superintelligence-lab-alexandr-wang-pitch-hire-ai-talent-2025-9 (17.09.2025)
- https://x.com/alexandr_wang/status/2099015997525291211 (13.09.2026)

Mistral
- https://thenextweb.com/news/mistral-ceo-open-source-enterprise-warning (05.07.2026)
- https://www.economist.com/insider/inside-tech/an-interview-with-arthur-mensch (14.07.2026)
- https://www.lesechos.fr/tech-medias/intelligence-artificielle/mistral-il-y-a-une-fragmentation-du-monde-sur-lintelligence-artificielle-2250434 (08.09.2026)

xAI
- https://techcrunch.com/2025/08/13/co-founder-of-elon-musks-xai-departs-the-company/ (13.08.2025)
- посты Musk 14.09.2026 (Grok 4.8/5; «Dario is right») — репортаж: https://teslanorth.com/2026/09/14/elon-grok-roadmap-through-grok-5/

Китай
- https://hellochinatech.com/p/deepseek-liang-wenfeng-transcript (23.07.2026)
- https://www.recodechinaai.com/p/liang-wenfeng-on-agi-compute-and (23.07.2026)
- https://www.nature.com/articles/d41586-025-03845-4 (08.12.2025)
- https://scmp.com/tech/tech-trends/article/3303403/ai-entrepreneur-lee-kai-fu-bets-his-start-ups-future-deepseek-and-open-source (22.03.2025)
- https://www.reuters.com/technology/artificial-intelligence/deepseek-narrows-china-us-ai-gap-three-months-01ai-founder-lee-kai-fu-says-2025-03-25/ (25.03.2025)
- https://www.bloomberg.com/features/2026-kai-fu-lee-weekend-interview/ (04.09.2026)

Продуктовый слой
- https://techcrunch.com/2025/12/09/why-cursors-ceo-believes-openai-anthropic-competition-wont-crush-his-startup/ (09.12.2025)
- https://www.theverge.com/decoder-podcast-with-nilay-patel/773904/sierra-ceo-bret-taylor-ai-agents-openai-bubble-interview (11.09.2025)
- https://sierra.ai/blog/hyper-t-bench-evaluating-agents-that-build-agents (08.09.2026)
- https://techcrunch.com/2025/04/24/perplexity-ceo-says-its-browser-will-track-everything-users-do-online-to-sell-hyper-personalized-ads/ (24.04.2025)
- https://www.bloomberg.com/features/2025-fei-fei-li-weekend-interview/ (21.11.2025)
- https://sequoiacap.com/article/reflection-ai-spotlight (2025)
- https://www.theglobeandmail.com/business/technology/article-cohere-ceo-aidan-gomez-criticizes-calls-for-ai-slowdown/ (14.09.2026)
- https://www.cnbc.com/2026/09/14/ai-models-cyber-weapon-cohere-safety-debate.html (14.09.2026)

---

H. Что НЕ удалось найти / где данные слабые

- Greg Brockman: полный paywalled Odd Lots/Stratechery текст; опирался на цитаты Bloomberg и YouTube.
- Ilya: dwarkesh.com отдал плеер без полного транскрипта в fetch; цитаты сверены по нескольким независимым расшифровкам/цитатам, не по одному HTML.
- Liang: это утечка investor call, не официальная публикация DeepSeek. Смысл устойчив across 3 зеркал, но это всё же leak.
- Daniela Amodei, Tom Brown, Lample, Lacroix, Shane Legg, Koray, Jeff Dean, Sergey Brin, Ahmad Al-Dahle: мало соло-голоса 2025–26.
- Zhipu/MiniMax/Qwen/ByteDance Seed: почти нет англоязычных первоисточников основателей уровня Liang/Yang/Kai-Fu.
- Inflection, Adept, Character.AI, AI21, Reka: живых манифестов фаундеров 2025–26 под задачу не собрал.
- Bob McGrew: есть сильный июнь 2025, нет большого 2026.
- «Настоящие причины» расколов (OpenAI 2023, Anthropic split, SSI vs Meta offer, Babuschkin) — публично названы частично; внутренние конфликты по определению не в первоисточниках.
- Unit economics (маржа инференса, burn до 2028) — в основном из интервью Altman/Dario и вторичной финансовой прессы, не из audited filings.
- Не открывал платные полные тексты NYT/FT/Bloomberg features целиком там, где paywall обрезал; где paywall — помечал и брал дословные куски, которые страница всё же отдала.

Интерпретация, не цитата: заказчику на пивот важнее не «кто победит в AGI», а три открытые ниши, которые они произносят и сами не закрывают — verification/long-horizon reliability, sovereign/on-prem flywheel на open weights, и spatial/world-model слой, пока LLM-лабы жгут гигаватты на текст.
