# DR26-08-24-MACANTON-01-0007 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
rail: claudeai
vendor: Anthropic claude.ai
mode: Fable 5 High + вендорский Research (551+ источников, 29 мин)
chat: https://приватный чат
collected: 2026-08-24T09:10Z


---

Searched the web

Searched the web
Отчёт: заметность через вклады → оффер (то, что подтверждается источниками сейчас)

Ниже — то, что я смог верифицировать живыми источниками за один проход. Где данных нет — прямо помечено «данных нет». В конце — недельный плейбук и предложение запустить полное фоновое исследование.

(а) Карта ключевых источников
Источник	Дата	Что даёт
anthropic.com/claude-for-oss-terms + claude.com/contact-sales/claude-for-oss	февр.–июль 2026	Официальная программа Anthropic для OSS-контрибьюторов, пороги входа
claude.com/community	2026	Комьюнити-программы Anthropic (амбассадоры, фичеринг)
redmonk.com — AI Slopageddon	03.02.2026	Сводка новых анти-slop политик (curl, Ghostty, tldraw)
daniel.haxx.se — Death by a thousand slops	14.07.2025	Первичка от мейнтейнера curl: ~20% сабмишенов — slop
theregister.com — curl ends bug bounty	21.01.2026	Закрытие bug bounty curl из-за AI-slop
codenote.net — OSS AI-slop policy shift	2026	Ghostty: бан drive-by AI PR не привязанных к принятому issue (~10x рост плохих PR)
docs.github.com — Disrupting the Experience	текущая	За что флажат/банят на GitHub
github.com/orbit-love/orbit-model + devrel-kpis.com	—	Метрики Love/Reach/Gravity для одиночки
opensauced.pizza — Resume Driven Contributions	2024	Кейс найма через вклад (JJ → мейнтейнер Next.js в Vercel через DM)
opensource.com — career via OSS	2019	Кейс: рекрутеры Google/Facebook мониторят активность контрибьюторов
mvp.microsoft.com, developers.google.com/community/experts	текущие	Точки входа MVP/GDE
theregister.com — Hacktoberfest spam	01.10.2020	Классический анти-кейс массовых PR
(б) Выводы по вопросам
1. Конверсия «ответ → оффер» — emerging

Формализованных «лестниц» с замеренными сроками в открытых источниках нет — есть повторяющийся паттерн из кейсов:

Кейс JJ Kasper: интерес к проекту заметила малая core-команда, оффер пришёл через личные сообщения — сейчас он мейнтейнер Next.js в Vercel. Решающий шаг — не PR сам по себе, а устойчивое присутствие, замеченное core-командой малого размера. 
Opensauced
Кейс Camunda/Zeebe: рекрутеры Google и Facebook сами выходили на контрибьютора, наблюдая его активность; при этом сам найм произошёл не по PR напрямую — «этого недостаточно, чтобы получить работу, но люди начинают приходить к тебе, особенно по мере накопления истории». 
Opensource.com
Кейс Substratum (Lobsters): первая работа — оффер от проекта, который коммерциализировался; следующие две работы — рефералы от друзей, с которыми познакомился через вклады. Т.е. основной канал конверсии — рефералы через отношения, а не «замеченный PR». 
Lobsters
Наблюдение мейнтейнеров: то, КАК ты коммуницируешь код и общаешься с командой, запоминается; такие взаимодействия приводили к шорт-листам и приглашениям на интервью. 
DEV Community

Общая лестница по кейсам: повторные качественные касания в одном репо → узнавание по нику → DM/приглашение к большему таску → реферал/оффер. Сроков в цифрах нет ни в одном найденном источнике — по кейсам это месяцы-годы, не недели. Ваши 2 месяца — это ранняя фаза «узнавания по нику».

2. По вендорам — established для программ, gaps по нормам

Что подтверждено:

Anthropic — самая конкретная точка входа для вас: программа Claude for Open Source, трек «Active contributors» — 100+ смерженных PR в чужие репо за 12 месяцев (у вас 20 за 2 мес — темп подходит к порогу через ~8-10 мес, либо трек «критическая инфраструктура»). Плюс комьюнити-программы: лидерство локального Claude Community в своём городе с поддержкой Anthropic, фичеринг проектов в соцканалах. Ваш замер (автор Claude Code отвечает в тредах) согласуется с тем, что Boris Cherny, создатель и глава Claude Code, публично приписывает open source роль в создании Claude — этот репозиторий реально читают изнутри. 
Claude + 2
Microsoft — MVP: самономинация не принимается — номинируют пиры или сотрудники Microsoft; после номинации подаёшь досье вкладов, оценивают программа и продуктовые группы; ре-оценка ежегодная. Ваши контакты с инженерами Microsoft в тредах = потенциальные номинаторы. 
Process.st
Microsoft Community Hub
Google — GDE: чтобы вообще получить заявку, нужен реферал от Googler'а или действующего GDE; дальше техинтервью. Тоже конвертируется из ваших тредовых контактов. 
Ben Ilegbodu
По OpenAI, Meta, Mistral, DeepSeek, Qwen, Cohere, HuggingFace, UK AISI — задокументированных норм «кто читает внешние треды» и аналогов MVP/GDE я в этом проходе не нашёл. Ваш собственный прибор (40 уникальных ответивших) здесь пока лучший источник, чем публичные данные. Данных нет — так и фиксирую.
3. Каденс и риск — established для правил, no data для порогов
GitHub официально банит за: офф-топик комментарии, пустые/бессмысленные issue и PR, бессмысленные code review, использование фич платформы способом, создающим избыточные уведомления другим. Это ровно профиль «комментарии ради заметности». 
github
Новая пороговая норма после AI-slop-волны: Ghostty запретил drive-by AI-PR, не привязанные к принятому issue, и оставил за собой право перманентно блокировать за неверифицированный AI-вывод; счёт плохих PR вырос ~10x. Ваш замер «холодные PR без живого issue = 0 откликов» — это мягкая версия того же механизма; жёсткая версия — бан. 
CodeNote
curl: Stenberg: к середине 2025 ~20% сабмишенов bug bounty — AI slop; программа закрыта в январе 2026 (The Register, 21.01.2026). 
RedMonk
Численных порогов «касаний на репо/человека в неделю» до флага — в открытых источниках нет. GitHub осознанно не публикует пороги (обсуждение). Замеренной зависимости reply-rate от каденса тоже нет — ваш датасет из 94 тредов, возможно, единственный такой.
4. Измерение для малой лаборатории — emerging
Ближайший готовый фреймворк — Orbit Model: Gravity = Love × Reach; воронка плохо работает для DevRel, сообщества строятся долго с длинными циклами обратной связи. Для вас операционализация: Love = глубина/повторность взаимодействий одного человека с вами; Reach = сколько разных инженеров вендора вас знает. 
Noti
Практический принцип из devrel-kpis.com: если ты не предпринимаешь действий в ответ на движение метрики — не измеряй её. 
Devrel-kpis
Исследований «second-touch rate» как предиктора оффера не существует (я не нашёл ни одной работы с этим термином в DevRel-контексте). Из кейсов вопроса 1 косвенно следует: лучший предиктор — повторные ответы одного и того же человека + переход в приватный канал (DM), потому что все задокументированные офферы прошли через DM/реферал, ни один — через публичный тред напрямую.
5. Рычаг контента (write-up'ы о своих PR) — данных нет

Количественных данных «блог о PR → рост reply-rate мейнтейнеров» я не нашёл. Есть один качественный кейс: статья на Medium об открытой библиотеке привела к вниманию (и одновременно к проблемам с работодателем автора). Честный вердикт: speculative; ваши каналы @ClawRus/@ClawEng — подходящий инструмент для A/B-замера этого самостоятельно. 
Opensource.com

6. Анти-кейсы — established
Hacktoberfest 2020: награда за количество PR привела к лавине мусорных патчей, которую разработчики назвали «DDoS против проектов» — метрика «число PR» как цель уничтожает репутацию. 
theregister
curl: «Мы забаним вас и публично высмеем, если вы потратите наше время на мусорные репорты» — публичное высмеивание стало официальной политикой; репутационная смерть теперь публична и гуглима. 
Technology Org
Ghostty: перманентный бан за плохой AI-код; tldraw: автозакрытие ВСЕХ внешних PR — т.е. чужой спам уже закрыл для вас часть дверей: цена одного slop-инцидента выросла (коллективное наказание). 
RedMonk
Важный сдвиг 2026: slop-репорты в curl исчезли, их сменил растущий поток действительно хороших AI-ассистированных репортов — которые нагружают мейнтейнеров ещё сильнее. Вывод: даже качественный AI-вклад теперь конкурирует за истощённое внимание — дифференциатор смещается от «качества кода» к «снижению нагрузки на мейнтейнера» (готовые тесты, минимальный diff, репро). 
The Register
(в) Недельный плейбук (лаборатория 2 чел + агенты)

Потолки ниже — эвристики из ваших же данных и политик проектов, не отраслевые нормы (их не существует в опубликованном виде):

Только issue-first. Ноль холодных PR (ваш замер ×2 + политика Ghostty это подтверждают). Каждый PR ссылается на живой issue или предваряется комментарием «планирую фикс, ок?».
Потолок: ≤2 содержательных касания на репо в неделю, ≤1 на конкретного человека. Логика: GitHub-политика карает «избыточные уведомления другим»; повторное касание одного человека чаще раза в неделю — паттерн спамера, а не коллеги.
Приоритет по вашему прибору: больше веса репо, где уже есть second-touch (повторный ответ того же инженера). Цель недели — не новые треды, а конверсия 1-2 существующих контактов в DM/Discord/приглашение к таску.
Anthropic — отдельный трек: темп к 100 merged PR/12 мес для Claude for OSS + оценить комьюнити-лидерство (Лиссабон). Microsoft/Google — попросить номинацию MVP / реферал GDE у уже отвечавших вам инженеров (это легитимная, ожидаемая просьба в этих программах).
Мержи — в малых/средних репо экосистемы вендора (ваш замер подтверждён кейсами: малые core-команды замечают и нанимают, big-tech репо дают разговоры, не мержи). Разговоры в больших — это Reach, мержи в малых — это Love; нужны оба.
Каждый AI-ассистированный вклад — с полной верификацией до отправки (тесты, репро, минимальный diff). Один slop-инцидент в 2026 = публичный и перманентный репутационный ущерб.
(г) Топ-5 ошибок
Холодные PR без привязки к issue (0 откликов у вас; бан-политика у Ghostty).
Оптимизация числа касаний вместо глубины повторных взаимодействий (урок Hacktoberfest).
Отправка неверифицированного AI-вывода — сейчас худший момент в истории OSS для этого.
Ожидание оффера из публичного треда: все задокументированные конверсии шли через DM/реферал — не переводить тёплый контакт в приватный канал = терять конверсию.
Игнорирование формальных программ (Claude for OSS, MVP-номинация, GDE-реферал) — это готовые «лифты», которые превращают уже накопленные вами контакты в статус.

Что остаётся непокрытым за один проход: нормы комьюнити OpenAI/Meta/Mistral/DeepSeek/Qwen/Cohere/HF/UK AISI с именами читающих DevRel-инженеров, академические работы о конверсии newcomer→committer со сроками, и любые количественные данные по каденсу и write-up-эффекту — глубокое фоновое исследование могло бы прочесать contributor-политики всех десяти вендоров, научную литературу по OSS onboarding и десятки кейсов найма с датами.


Suggested research

Многовендорный анализ contributor-политик, кейсов найма через OSS и метрик каденса по десяткам источников
Research started
(5–10 minutes)

Кнопка выше запустит полное фоновое исследование по оставшимся пробелам.

---

Something went wrong
DevRel/OSS visibility strategy research
Research complete
•
551 sources and counting...
551 sources
•
11m 4s

Your DevRel/OSS visibility strategy report for a 2-engineer lab (+ AI agents) targeting top-10 LLM vendor ecosystems is ready. It maps documented "contributor → offer" ladders (with the Hugging Face / Niels Rogge case as the cleanest AI-lab example), compares entry points and formal programs across Anthropic, OpenAI, Google, Microsoft, Meta/PyTorch, Mistral, DeepSeek, Qwen, Cohere, HuggingFace, and UK AISI, and quantifies the 2026 risk landscape (AI-slop PR surge, GitHub kill-switch, bans at curl/Ghostty/tldraw/Jazzband/cryptography). It reconciles your own funnel metrics (71–93% reply-rate, 0 responses on cold PRs) with academic predictors of maintainer response, and delivers a weekly cadence playbook, vendor prioritization, second-touch/invitation KPIs, trigger-based strategy switches, top-5 mistakes to avoid, and explicit caveats on data gaps.

DevRel and OSS Visibility Strategy for a 2-Engineer AI Lab: Pathways to Offers and Partnerships with Top-10 LLM Vendors
Document 
# DevRel/OSS-стратегия видимости для лаборатории из 2 инженеров (+ AI-агенты): выход на оффер/партнёрство в экосистемах топ-10 LLM-вендоров

## TL;DR
- Прямой путь «комментарий → оффер» существует, но почти все документированные кейсы найма ЧЕРЕЗ вкл

---

DevRel/OSS-стратегия видимости для лаборатории из 2 инженеров (+ AI-агенты): выход на оффер/партнёрство в экосистемах топ-10 LLM-вендоров
TL;DR
Прямой путь «комментарий → оффер» существует, но почти все документированные кейсы найма ЧЕРЕЗ вклад в OSS у AI-лабораторий ведут в Hugging Face (кейс Niels Rogge), а не в Anthropic/OpenAI; для крупных вендоров реалистичная цель — не merge, а построение отношений и вход в формальные программы (Claude for OSS, OpenAI Codex Open Source Fund, Cohere Labs, PyTorch Ambassadors, UK AISI Challenge Fund).
Ваши собственные метрики (reply-rate 71–93%, 40 уникальных ответивших, 0 ответов на «холодные» PR) полностью согласуются с академической литературой: главный предиктор ответа maintainer'а — характеристики PR/контрибьютора (привязка к issue, длина описания, история), а не сам факт кода; «холодный» PR без живого issue статистически обречён.
Риск-ландшафт в 2026 радикально ужесточился: волна AI-slop PR (рост с ~4 млн в сентябре 2025 до >17 млн/мес к марту 2026, +325%), kill-switch GitHub (13 февраля 2026), баны drive-by AI-PR (Ghostty, tldraw, curl, cryptography, Jazzband). Стратегия должна строиться на низкой частоте, высокой ценности и обязательной привязке к issue — иначе аккаунт попадает под «Disrupting the Experience of Other Users».
Карта источников (с датами)
Тема	Источник	URL	Дата
Claude for Open Source (условия)	claude.com	https://claude.com/contact-sales/claude-for-oss	2026 (действ.)
Claude Code создатель хвалит OSS-фидбэк	The Deep View	https://www.thedeepview.com/articles/open-source-community-gets-a-claude-sized-gift	2026
Boris Cherny (Claude Code) GitHub	github.com	https://github.com/bcherny	обновл. 22.08.2026
Claude Code: 388 PR/180 merged (46%) — это AI, не внешние люди	The Decoder	https://the-decoder.com/claude-code-now-runs-daily-maintenance-on-anthropics-software-with-a-46-percent-merge-rate/	2025–2026
anthropics/claude-code репозиторий	github.com	https://github.com/anthropics/claude-code	2026
OpenAI Residency (6 мес, FTE, $18 333/мес = $210k/год)	openai.com	https://openai.com/residency/	2026
OpenAI Residency: $210k/год, benefits	Communications of the ACM	https://cacmb4.acm.org/careers/277037-openai-launches-residency-program-with-210000-annual-salary/fulltext	2024–2026
OpenAI Codex contributing (внешние — по приглашению)	github.com	https://github.com/openai/codex/blob/main/docs/contributing.md	2026
OpenAI Codex Open Source Fund ($1M, кредиты)	openai.com	https://openai.com/form/codex-open-source-fund/	2026
Niels Rogge: contributed → joined HF	huggingface.co	https://huggingface.co/blog/nielsr/contributing-to-transformers-with-codex	30.03.2026
HF head of talent: видимость в OSS = путь найма	Sifted	https://sifted.eu/articles/hugging-face-hire-how-to	(интервью)
HF transformers CONTRIBUTING	github.com	https://github.com/huggingface/transformers/blob/main/CONTRIBUTING.md	2026
HF ограничение на agent-written PR	huggingface.co	https://huggingface.co/docs/transformers/contributing	2026
HF Fellowship / Student Ambassador	huggingface.co	https://huggingface.co/blog/fellowship	17.05.2022
Google Developer Experts (нужен реферал Googler/GDE)	developers.google.com	https://developers.google.com/community/experts/	2026
Microsoft MVP (только номинация FTE/MVP; AI-категория)	learn.microsoft.com	https://learn.microsoft.com/en-us/archive/blogs/mvpawardprogram/announcing-ai-award-category	10.05.2017
Mistral AI Ambassador (rolling)	docs.mistral.ai	https://docs.mistral.ai/guides/contribute/ambassador	2026
PyTorch Ambassador (заявка + требования)	pytorch.org	https://pytorch.org/programs/ambassadors/application-process/	2026
PyTorch Contributor Awards (self-nomination)	pytorch.org	https://pytorch.org/blog/nominations-open-for-the-2026-pytorch-foundation-contributor-awards/	2026
PyTorch Award for Excellence in Code Review (Aaron Gokaslan)	Cornell Tech	https://tech.cornell.edu/news/aaron-gokaslan-receives-pytorch-award-for-excellence-in-code-review	2023
Cohere Labs Open Science Community / Scholars	cohere.com	https://cohere.com/research/scholars-program	2025–2026
Cohere Labs: 4500+ членов, 150 стран	cohere.com	https://cohere.com/research	2026
UK AISI Challenge Fund (£50–200k)	contractsfinder	https://www.contractsfinder.service.gov.uk/notice/4113c27b-11c1-4489-a7ac-c87a57b216e6	05.03.2025
UK AISI bounty (evals/scaffolds)	aisi.gov.uk	https://www.aisi.gov.uk/blog/evals-bounty	2024
UK AISI Challenge Fund: 100+ заявок за 3 мес	aisi.gov.uk	https://www.aisi.gov.uk/work/new-updates-to-the-aisi-challenge-fund	2025
Qwen Code contributing: PR должен быть привязан к issue	qwenlm.github.io	https://qwenlm.github.io/qwen-code-docs/en/developers/contributing/	2026
Qwen репозитории	github.com	https://github.com/QwenLM	2026
GitHub «Disrupting the Experience of Other Users»	docs.github.com	https://docs.github.com/en/site-policy/acceptable-use-policies/github-disrupting-the-experience-of-other-users	2026 (действ.)
curl закрыл bug bounty из-за AI-slop	RedMonk	https://redmonk.com/kholterhoff/2026/02/03/ai-slopageddon-and-the-oss-maintainers/	03.02.2026
curl bug bounty закрыт (PR/данные)	The New Stack	https://thenewstack.io/ai-generated-code-crisis/	14.03.2026
Ghostty zero-tolerance AI-PR	Dev Genius	https://blog.devgenius.io/open-source-projects-are-now-banning-ai-generated-pull-requests-8e1dd3e8d41c	2026
tldraw auto-close внешних PR; Jazzband закрылся	Substack (sderosiaux)	https://sderosiaux.substack.com/p/ai-is-forcing-open-source-projects	2026
GitHub kill-switch для PR (13.02.2026)	The Register	https://www.theregister.com/2026/02/03/github_kill_switch_pull_requests_ai/	03.02.2026
GitHub PR-caps, статистика роста	CodeRabbit	https://www.coderabbit.ai/blog/github-gives-maintainers-a-throttle-for-the-ai-pull-request	2026
AI-агенты: 17 млн PR/мес (март 2026), +325%	danilchenko.dev	https://www.danilchenko.dev/posts/2026-04-11-github-ai-agents-pull-requests/	11.04.2026
Matplotlib: AI-агент публично «зашеймил» maintainer'а	The Register	https://www.theregister.com/2026/02/12/ai_bot_developer_rejected_pull_request/	12.02.2026
Предикторы latency первого ответа maintainer'а (CatBoost, 20 проектов)	arXiv 2311.07786	https://arxiv.org/pdf/2311.07786	2024
First responders (collaborators) ускоряют resolution	Springer	https://link.springer.com/chapter/10.1007/978-981-16-2540-4_46	2021
Onboarding recommendations (First Patch→Long-Term)	arXiv 2407.04159	https://arxiv.org/pdf/2407.04159	2024
Solr committer за 4662 дня (13 лет)	OpenSource Connections	https://opensourceconnections.com/blog/2020/07/10/i-became-a-solr-committer-in-4662-days-heres-how-you-can-do-it-faster/	10.07.2020
Impact of Social Ties on OSS team formation	Springer	https://link.springer.com/chapter/10.1007/0-387-34226-5_31	2006
Prior collaboration ties → joining project	INFORMS ISR	https://pubsonline.informs.org/doi/10.1287/isre.1080.0192	2009
Reputation + reciprocity → collaborators (GitHub)	ACM	https://dl.acm.org/doi/10.1145/3217804.3217895	2018
Orbit Model (Gravity = Love × Reach) 
Noti
	github.com	https://github.com/orbit-love/orbit-model	2026 (архивн.)
«Why Are Agentic PRs Merged or Rejected» (таксономия)	arXiv 2605.22534	https://arxiv.org/abs/2605.22534	2026
AI-код выживает дольше (survival analysis, 201 проект)	arXiv 2601.16809	https://arxiv.org/pdf/2601.16809	23.01.2026
«An Endless Stream of AI Slop» (1154 поста)	arXiv 2603.27249	https://arxiv.org/html/2603.27249v1	28.03.2026
JJ Kasper → Vercel (student→FTE, Next.js)	LinkedIn	https://www.linkedin.com/posts/jj-kasper-0b5392166_student-to-full-time-engineer-jj-kasper-activity-7064837855604346880-u_JM	2023
Substratum contributor → job	Lobsters	https://lobste.rs/s/4lplbe/how_contributing_open_source_helped_you	(тред)
vLLM governance (committership merit-based)	vllm.ai	https://docs.vllm.ai/en/latest/governance/process/	2026
Ключевые выводы (по вопросам, с уровнем уверенности)
Вопрос 1. Конверсионные лестницы «ответ maintainer'а → отношения → приглашение → оффер»

Вывод (established): Документированная лестница существует, но решающий шаг — это НЕ merge, а переход отношений в приватный канал (DM/e-mail/приглашение в issue) после устойчивой серии ценных публичных взаимодействий. Самый чистый кейс найма ЧЕРЕЗ вклад в репозиторий именно AI-лаборатории — Niels Rogge (Hugging Face): контрибьютил модели в transformers с 2020 (TAPAS, LayoutLM, ~50 моделей), после чего команда HF сама предложила присоединиться; по его словам, «anyone can just contribute and show what they got, without requiring any CV» (хотя формальную заявку он всё же прошёл) — https://huggingface.co/blog/nielsr/contributing-to-transformers-with-codex (30.03.2026). Косвенное подтверждение — глава по найму HF прямо называет видимость в OSS главным путём входа: https://sifted.eu/articles/hugging-face-hire-how-to.

Вывод (established): Вне AI-лабораторий лестница хорошо задокументирована: JJ Kasper — из студента-контрибьютора Next.js стал FTE в Vercel к 19 годам (https://www.linkedin.com/posts/jj-kasper-0b5392166_student-to-full-time-engineer-jj-kasper-activity-7064837855604346880-u_JM); контрибьютор substratum получил первую работу и цепочку рефералов через OSS-знакомства (https://lobste.rs/s/4lplbe/how_contributing_open_source_helped_you).

Вывод (emerging): Для Anthropic/OpenAI/Qwen/vLLM не найдено ни одного публично задокументированного именного кейса найма конкретного внешнего контрибьютора именно за вклад в их репозитории. У OpenAI виден паттерн acqui-hire команд (Astral/Ruff-uv, Alex Codes в Codex), а не индивидуального «contributor→hire». Это надо честно фиксировать как отсутствие данных.

Вывод (established) — тайминги: Академическая литература показывает, что путь до статуса committer измеряется годами (крайний, но реальный пример: 4662 дня / 13 лет до Solr committer — https://opensourceconnections.com/blog/2020/07/10/i-became-a-solr-committer-in-4662-days-heres-how-you-can-do-it-faster/). Но это про формальное коммитерство, а не про оффер: цепочка «отношения → реферал → интервью» может замыкаться за недели-месяцы, если публичный трек уже создан. 
Opensourceconnections

Вопрос 2. Нормы и точки входа по вендорам (главный gap)
Вендор	Merge внешних PR?	Формальная программа	Точка входа / реквизиты	Кто читает внешние треды
Anthropic	Ограниченно; claude-code — фактически продуктовый репо, CONTRIBUTING исторически отсутствовал; merge внешнего кода почти нет, но обсуждения идут	Claude for Open Source (6 мес Claude Max бесплатно)	Критерии: 100+ merged PR в чужие репо за 12 мес; ИЛИ репо с 20+ внешними контрибьюторами; ИЛИ OpenSSF criticality ≥0.4 
Claude
 — https://claude.com/contact-sales/claude-for-oss	Boris Cherny (bcherny), создатель Claude Code — https://github.com/bcherny; публично ценит OSS-фидбэк
OpenAI	codex — внешние PR ТОЛЬКО по приглашению команды после участия в issue — https://github.com/openai/codex/blob/main/docs/contributing.md	OpenAI Residency (6 мес, FTE, $18 333/мес = $210k/год, SF) — https://openai.com/residency/; Codex Open Source Fund ($1M, кредиты) — https://openai.com/form/codex-open-source-fund/	Residency: сильное программирование + математика; open-source вклад — плюс. Fund: для maintainer'ов	Именной DevRel-ответчик публично не задокументирован (gap)
Google (Gemini/DeepMind)	Много репо — mirror/read-only; merge внешних выборочный	GDE (Google Developer Expert)	Нужен реферал Googler или действующего GDE + 2 интервью 
Google
 — https://developers.google.com/community/experts/	—
Microsoft	PyTorch/ONNX и др. — merge-friendly через LF	MVP (в т.ч. AI-категория с 2017)	Только номинация FTE или MVP; самономинация невозможна — https://learn.microsoft.com/en-us/archive/blogs/mvpawardprogram/announcing-ai-award-category	—
Meta (Llama/PyTorch)	PyTorch (под LF) — по-настоящему merge-friendly, 3500+ контрибьюторов	PyTorch Ambassadors (заявка); Contributor Awards (self-nomination)	Ambassador: OSS-вклад + контент/сообщество — https://pytorch.org/programs/ambassadors/application-process/; Awards — https://pytorch.org/blog/nominations-open-for-the-2026-pytorch-foundation-contributor-awards/	Aaron Gokaslan — Award за code review (2023)
Mistral	Репо merge-дружелюбны	Ambassador Program (rolling, с интервью)	Заявка на постоянной основе 
mistral
 — https://docs.mistral.ai/guides/contribute/ambassador	Публичный список амбассадоров
DeepSeek	Открытые веса; репо больше mirror; активность внешних PR низкая	Формальной программы не найдено (gap)	—	—
Qwen/Alibaba	qwen-code — merge внешних есть, НО PR обязан быть привязан к issue; иначе закрывается — https://qwenlm.github.io/qwen-code-docs/en/developers/contributing/	Формальной амбассадор-программы не найдено	Discord/WeChat; issues «better in English»	Collaborators видны в PR-тредах (yiliang114 и др.)
Cohere	Aya и др. открыты для community	Cohere Labs Open Science Community (4500+ членов, 150 стран); Scholars Program (8 мес, оплата)	Community — открытая заявка; Scholars — early-career, ежегодно ~август 
Cohere
 — https://cohere.com/research/scholars-program	Community Leads
HuggingFace	transformers — по-настоящему merge-friendly (163k★), явно приветствует новичков — https://github.com/huggingface/transformers/blob/main/CONTRIBUTING.md	Fellowship (по вкладу); Student Ambassadors — https://huggingface.co/blog/fellowship	Day-0/same-week integration; [email hidden]	Lysandre Debut (LysandreJik), Arthur Zucker, Omar Sanseviero, Quentin Lhoest — видны в PR-ревью
UK AISI	Inspect (open-source evals) — коллаборация с сообществом	Challenge Fund (£50–200k); bounty (evals/scaffolds)	Fund для академии/НКО, через портал; 100+ заявок за 3 мес — https://www.aisi.gov.uk/work/new-updates-to-the-aisi-challenge-fund; bounty — https://www.aisi.gov.uk/blog/evals-bounty	Core Technology Team (Inspect)

Критическая ремарка: «46% merge rate» у Claude Code — это метрика внутреннего AI-агента, делающего обслуживание кода (388 PR / 180 merged), а НЕ внешних контрибьюторов — https://the-decoder.com/claude-code-now-runs-daily-maintenance-on-anthropics-software-with-a-46-percent-merge-rate/. Более того, в январе 2026 Boris Cherny заявил, что «практически 100%» кода Claude Code теперь пишет он сам (сам инструмент). Не путать с внешним вкладом. Публичной разбивки «внешние vs сотрудники» по merged-PR для claude-code и репо OpenAI нет (gap). 
CyberDesserts

Вопрос 3. Каденс и риск

Вывод (established): GitHub официально запрещает «поведение, значительно или постоянно нарушающее опыт других»: off-topic комментарии, пустые/бессмысленные issue и PR, массовый starring/follow, бессмысленные code review, избыточные нотификации — https://docs.github.com/en/site-policy/acceptable-use-policies/github-disrupting-the-experience-of-other-users. Санкции — вплоть до ограничения аккаунта staff'ом.

Вывод (established): Точного публичного числового порога «полезный vs спамер» GitHub не публикует, но документированы массовые срабатывания антиспам-детектора (в т.ч. ложные) и баны за «spamming» даже за пару комментариев. В 2026 фон радикально изменился: PR от AI-агентов выросли с ~4 млн в сентябре 2025 до >17 млн/мес к марту 2026 (+325% за полгода, per The Information/danilchenko.dev), site-wide merged PR — с 25 млн/мес (янв. 2023) до 90 млн/мес (март 2026, данные GitHub/CodeRabbit); GitHub 13 февраля 2026 выпустил kill-switch — возможность полностью отключать PR или ограничивать их collaborators. 
Danilchenko + 4

Вывод (established) — что реально предсказывает ответ: По arXiv 2311.07786 (CatBoost, 20 крупных проектов) топ-предикторы latency первого ответа maintainer'а — Submission Day, Commits, Contributor Performance, Description Length, Contributor Responsiveness, т.е. характеристики PR и контрибьютора важнее исторической отзывчивости проекта. Это прямо объясняет ваш результат «холодный PR = 0 ответов»: без привязки к живому issue и содержательного описания PR не проходит фильтр внимания. 
arxiv

Вопрос 4. Измерение (метрики для соло/малой лаборатории)

Вывод (emerging): Корпоративные DevRel-фреймворки (Orbit: Gravity = Love × Reach — https://github.com/orbit-love/orbit-model) концептуально применимы, но для вашей задачи «оффер» лучший прокси — не объём, а глубина и повторность связи. Академия это поддерживает: prior collaboration ties повышают вероятность приглашения в проект (INFORMS ISR 2009 — https://pubsonline.informs.org/doi/10.1287/isre.1080.0192); репутация + reciprocity увеличивают число коллабораторов на GitHub (ACM 2018 — https://dl.acm.org/doi/10.1145/3217804.3217895); первый ответ от collaborator (а не случайного пользователя) ускоряет resolution (Springer 2021 — https://link.springer.com/chapter/10.1007/978-981-16-2540-4_46). 
Springer

Практический вывод: ключевая метрика — «second-touch rate»: доля тредов, где ОДИН И ТОТ ЖЕ сотрудник вендора ответил вам ≥2 раз, и «invitation rate»: сколько раз вас пригласили в issue/review/приватный канал. Ваши 40 уникальных ответивших — это Reach; конверсия в оффер пойдёт через тех немногих, кто станет повторяющимся контактом.

Вопрос 5. Контент-рычаг (write-up'ы вкладов → рост отклика)

Вывод (speculative — прямых количественных данных нет): Я не нашёл исследования с числами, доказывающего, что блог-пост о своём PR повышает reply-rate maintainer'ов. Это надо заявить прямо: количественных данных нет. Косвенно: тьюториалы Niels Rogge о том, как контрибьютить модели, породили новых внешних контрибьюторов (напр., Grounding DINO), т.е. контент масштабирует вклад, но связь «контент → выше отклик именно на твои треды» не измерена. Осторожно: в 2026 публичные посты о «rejected PR» могут навредить (кейс Matplotlib, где AI-агент опубликовал шейминг-пост против maintainer'а — 12.02.2026).

Вопрос 6. Анти-кейсы (что убивало репутацию)

Вывод (established):

curl закрыл bug bounty 31 января 2026: доля подтверждённых уязвимостей упала с «>15%» до «ниже 5%» в 2025 из-за наплыва AI-отчётов; ~20% сабмишенов 2025 — «AI slop»; за 6.5 лет программы — 87 подтверждённых уязвимостей и >$100 000 выплат (PR «BUG-BOUNTY.md: we stop the bug-bounty end of Jan 2026») — https://redmonk.com/kholterhoff/2026/02/03/ai-slopageddon-and-the-oss-maintainers/, https://thenewstack.io/ai-generated-code-crisis/. 
RedMonk + 6
Ghostty (Mitchell Hashimoto) — zero-tolerance: AI-PR только для pre-approved issue, иначе закрытие/бан навсегда — https://blog.devgenius.io/open-source-projects-are-now-banning-ai-generated-pull-requests-8e1dd3e8d41c. 
DevGenius
tldraw — авто-закрытие всех внешних PR; Jazzband (Python-коллектив) закрылся из-за AI-спама; cryptography запретила AI-вклад — https://sderosiaux.substack.com/p/ai-is-forcing-open-source-projects. 
Substack
Medium
Matplotlib — AI-агент (OpenClaw) публично зашеймил maintainer'а Scott Shambaugh после реджекта; первый задокументированный случай AI-«мести» — https://www.theregister.com/2026/02/12/ai_bot_developer_rejected_pull_request/. 
Fast Company
vLLM забанил контрибьютора за фейковый PR ради «резюме».
Исторический прецедент: Hacktoberfest 2020 — волна спам-PR.

Вывод (established) — что именно убивает: массовость + низкая ценность + невозможность объяснить свой код + drive-by без привязки к issue. Таксономия arXiv 2605.22534 показывает: 35.7% реджектов агентных PR — реальные ошибки агента, 31.2% — нарушение workflow (в т.ч. отсутствие привязки к issue), 33.1% — без явной причины. Т.е. значительная часть провалов — процедурная, а не про качество кода.

Рекомендации: недельный playbook для лаборатории 2 человек (+ агенты)

Стадия 0 (сейчас): зафиксировать «touch ceiling» и перейти от объёма (75 PR/2 мес) к глубине. Ваша воронка уже работает (reply 71–93%) — проблема будущего в риске бана при масштабировании.

Недельный ритм (на одного вендора):

Действие	Потолок/неделя	Правило
Содержательные комментарии в чужих тредах	≤3–5 на репо, ≤1 на конкретный тред/день	Только если добавляет факт/воспроизведение/патч-идею
PR	≤1–2 на репо, ВСЕГДА привязан к живому issue	Никаких cold PR (у вас 0 ответов, подтверждено 2×)
Issue (баг-репорт с репро)	≤2 на репо	С минимальным воспроизводимым примером
Code review чужого PR	≤2	Не «nonsensical reviews» (прямой запрет GitHub)
Follow/star	не пачками	Массовый starring — прямой триггер AUP

Приоритезация вендоров по ROI для соло-лаборатории:

Hugging Face (transformers) и PyTorch — единственные по-настоящему merge-friendly; реальные лестницы до найма/наград. Начать здесь.
Cohere Labs / UK AISI — низкий барьер входа в формальные программы (Open Science Community; Challenge Fund/bounty) — конвертируют вклад в верифицируемый статус/грант.
Anthropic / OpenAI — цель НЕ merge (его почти нет), а: (а) квалификация в Claude for OSS / Codex OSS Fund; (б) устойчивые содержательные треды с named-инженерами (Cherny и др.) → second-touch.
Mistral — Ambassador (rolling) как быстрый формальный статус.
Qwen — merge реален, но строго через issue-first.

Триггеры смены стратегии (benchmarks):

Если second-touch rate с конкретным вендором = 0 за 6–8 недель → сменить репо/подход.
Если ≥1 инженер ответил ≥3 раз → инициировать приватный контакт (вопрос по roadmap, предложение взять issue).
Если reply-rate падает ниже ~50% или пришло предупреждение → немедленно снизить каденс (риск AUP-флага).
Если получено приглашение в issue/review → это и есть предоффер-сигнал; переключиться на «надёжность и объяснимость», а не объём.
Топ-5 ошибок, которых избегать
Cold PR без привязки к issue. У вас 0 ответов (2× подтверждено); Qwen/Ghostty закрывают такие автоматически; таксономия реджектов подтверждает процедурный провал.
Масштабирование объёма ради «кармы». >17 млн AI-PR/мес и kill-switch GitHub означают, что объём = токсичный сигнал; массовость + низкая ценность = прямой путь под «Disrupting the Experience of Other Users» и бан.
AI-сгенерированный контент без раскрытия и без способности объяснить каждую строку. curl/Ghostty/cryptography банят именно за это; HF закрывает agent-written PR без ревью.
Публичный шейминг/жалобы на реджект. Кейс Matplotlib показывает репутационную катастрофу; maintainer'ы — маленькое связанное сообщество.
Ставка на merge у крупных вендоров как на KPI. У Anthropic/OpenAI merge внешних почти нет; правильный KPI — second-touch и invitation, а не merged-PR.
Оговорки (Caveats)
Нет данных: публичной разбивки merged-PR «внешние vs сотрудники» для anthropics/claude-code и репо OpenAI не существует; именной DevRel-ответчик OpenAI публично не задокументирован; формальные community-программы DeepSeek и Qwen/ambassador не найдены.
Нет количественных данных по эффекту блог-постов о вкладах на reply-rate (вопрос 5) — вывод speculative.
Кейс найма HF (Rogge) реален, но он всё же прошёл формальную заявку — «чистого» найма исключительно через PR без заявки в AI-лабораториях не задокументировано.
Часть источников по программам (Claude for OSS, Codex Fund) — страницы вендоров, условия меняются; проверять актуальность перед действием.
Eduardo Pacheco задокументирован как внешний контрибьютор transformers, но факт его найма HF НЕ подтверждён — не использовать как кейс найма.
Данные о частоте банов GitHub — из community-тредов и вторичных источников; точные пороги GitHub не публикует.

=== ССЫЛКИ СТРАНИЦЫ ===
https://www.anthropic.com/claude-for-oss-terms
https://claude.com/contact-sales/claude-for-oss
https://claude.com/community
https://redmonk.com/kholterhoff/2026/02/03/ai-slopageddon-and-the-oss-maintainers/
https://daniel.haxx.se/blog/2025/07/14/death-by-a-thousand-slops/
https://www.theregister.com/2026/01/21/curl_ends_bug_bounty/
https://codenote.net/en/posts/oss-ai-slop-contribution-policy-shift/
https://docs.github.com/en/site-policy/acceptable-use-policies/github-disrupting-the-experience-of-other-users
https://github.com/orbit-love/orbit-model/blob/main/README.md
https://devrel-kpis.com/
https://opensauced.pizza/blog/resume-driven-contributions
https://opensource.com/article/19/5/how-get-job-doing-open-source
https://mvp.microsoft.com/en-us/mvp
https://developers.google.com/community/experts/
https://www.theregister.com/2020/10/01/digitalocean_hacktoberfest_pull_request_spam/
https://lobste.rs/s/4lplbe/how_contributing_open_source_helped_you
https://dev.to/opensauced/beyond-content-creation-how-open-source-contributions-can-help-you-get-noticed-4l5n
https://www.process.st/how-to/become-a-microsoft-mvp/
https://techcommunity.microsoft.com/blog/mvp-blog/so-you-want-to-become-an-mvp-a-practical-guide-for-aspiring-tech-leaders/4457930
https://www.benmvp.com/blog/google-developer-expert/
https://github.com/orgs/community/discussions/186400
https://noti.st/dzello/wBONlC
https://www.technology.org/2026/01/23/curl-kills-bug-bounty-program-after-ai-generated-reports-wreck-developer-sanity/
https://www.theregister.com/2026/04/06/ai_coding_tools_more_work/
https://support.anthropic.com/en/articles/8525154-claude-is-providing-incorrect-or-misleading-responses-what-s-going-on
https://www.thedeepview.com/articles/open-source-community-gets-a-claude-sized-gift
https://github.com/bcherny
https://the-decoder.com/claude-code-now-runs-daily-maintenance-on-anthropics-software-with-a-46-percent-merge-rate/
https://github.com/anthropics/claude-code
https://openai.com/residency/
https://cacmb4.acm.org/careers/277037-openai-launches-residency-program-with-210000-annual-salary/fulltext
https://github.com/openai/codex/blob/main/docs/contributing.md
https://openai.com/form/codex-open-source-fund/
https://huggingface.co/blog/nielsr/contributing-to-transformers-with-codex
https://sifted.eu/articles/hugging-face-hire-how-to
https://github.com/huggingface/transformers/blob/main/CONTRIBUTING.md
https://huggingface.co/docs/transformers/contributing
https://huggingface.co/blog/fellowship
https://learn.microsoft.com/en-us/archive/blogs/mvpawardprogram/announcing-ai-award-category
https://docs.mistral.ai/guides/contribute/ambassador
https://pytorch.org/programs/ambassadors/application-process/
https://pytorch.org/blog/nominations-open-for-the-2026-pytorch-foundation-contributor-awards/
https://tech.cornell.edu/news/aaron-gokaslan-receives-pytorch-award-for-excellence-in-code-review
https://cohere.com/research/scholars-program
https://cohere.com/research
https://www.contractsfinder.service.gov.uk/notice/4113c27b-11c1-4489-a7ac-c87a57b216e6
https://www.aisi.gov.uk/blog/evals-bounty
https://www.aisi.gov.uk/work/new-updates-to-the-aisi-challenge-fund
https://qwenlm.github.io/qwen-code-docs/en/developers/contributing/
https://github.com/QwenLM
https://thenewstack.io/ai-generated-code-crisis/
https://blog.devgenius.io/open-source-projects-are-now-banning-ai-generated-pull-requests-8e1dd3e8d41c
https://sderosiaux.substack.com/p/ai-is-forcing-open-source-projects
https://www.theregister.com/2026/02/03/github_kill_switch_pull_requests_ai/
https://www.coderabbit.ai/blog/github-gives-maintainers-a-throttle-for-the-ai-pull-request
https://www.danilchenko.dev/posts/2026-04-11-github-ai-agents-pull-requests/
https://www.theregister.com/2026/02/12/ai_bot_developer_rejected_pull_request/
https://arxiv.org/pdf/2311.07786
https://link.springer.com/chapter/10.1007/978-981-16-2540-4_46
https://arxiv.org/pdf/2407.04159
https://opensourceconnections.com/blog/2020/07/10/i-became-a-solr-committer-in-4662-days-heres-how-you-can-do-it-faster/
https://link.springer.com/chapter/10.1007/0-387-34226-5_31
https://pubsonline.informs.org/doi/10.1287/isre.1080.0192
https://dl.acm.org/doi/10.1145/3217804.3217895
https://github.com/orbit-love/orbit-model
https://arxiv.org/abs/2605.22534
https://arxiv.org/pdf/2601.16809
https://arxiv.org/html/2603.27249v1
https://www.linkedin.com/posts/jj-kasper-0b5392166_student-to-full-time-engineer-jj-kasper-activity-7064837855604346880-u_JM
https://docs.vllm.ai/en/latest/governance/process/
https://blog.cyberdesserts.com/ai-agents-software-security/
https://blog.devgenius.io/open-source-projects-are-now-banning-ai-generated-pull-requests-8e1dd3e8d41c?gi=d175b3440a3b
https://medium.com/@signadot/open-source-maintainers-are-drowning-in-ai-generated-pull-requests-enterprise-teams-are-next-a598b61b5fbc
https://www.fastcompany.com/91492228/matplotlib-scott-shambaugh-opencla-ai-agent