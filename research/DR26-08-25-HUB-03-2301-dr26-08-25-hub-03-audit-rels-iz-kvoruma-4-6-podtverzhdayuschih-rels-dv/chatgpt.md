# DR26-08-25-HUB-03-2301 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

## метод и дата среза

срез — 25 августа 2026 года; всё про конец 2026 года ниже является прогнозом, а не свершившимся фактом. суммы, даты, лиды и traction приведены только там, где найден живой источник; company metrics помечены как self-reported. оценка не вычисляется из размера раунда. отсутствие раскрытой оценки означает «не раскрыта», а не «неизвестно автору».

пресс-релиз подтверждает заявление компании, но не аудит, фактическое поступление денег или использование продукта. согласие нескольких моделей без источника здесь не использовалось.

## executive summary — 7 строк

1. реально выходить с запросом **$5m**; вероятный диапазон закрытия для текущего профиля — **$3.5–5.5m**, stretch после внешнего доказательства — **$6.5–8m**. опора — $5m codeintegrity без marquee-pedigree против $11–20m раундов команд с клиентами или известными операторами ([codeintegrity](https://www.codeintegrity.ai/blog/codeintegrity-seed-round), [runlayer](https://techcrunch.com/2025/11/17/mcp-ai-agent-security-startup-runlayer-launches-with-8-unicorns-11m-from-khoslas-keith-rabois-and-felicis/), [neuraltrust](https://neuraltrust.ai/news/neuraltrust-raises-20m)).

2. формулировка максимальной конверсии: **«agent action security — zero-trust runtime control plane для корпоративных флотов агентов»**; смысл — каждое значимое действие авторизуется вне модели до исполнения.

3. комбо-тезис лучше узкого «mcp-аудита», только если безопасность остаётся клином, а fleet control plane — расширением; без внешнего runtime-доказательства потолок почти не меняется, после него прибавка к потолку — ориентировочно **$1.5–3m**.

4. основной ров — не список cve и не oss, а **компаундящийся threat/action graph из реальных исполнений плюс switching cost накопленных политик, identity-графа и аудита**; похожий data-loop использовала lakera через gandalf, собравший свыше 50m взаимодействий к раунду $20m ([lakera](https://www.prnewswire.com/news-releases/lakera-raises-20m-series-a-to-secure-generative-ai-applications-302204874.html)).

5. главный wow — независимо воспроизводимый replay: одна отравленная задача атакует несколько разных агентов, а единая граница останавливает действие до tool call, показывает цепочку полномочий, требует человека для необратимого шага и сохраняет проверяемый журнал.

6. первый быстрый «да» вероятнее придёт от pre-seed программы или operator-angel; lead на $5m — от не конфликтующего cyber-seed фонда. стратегические фонды требуют интеграции, market demand или уже подтверждённого lead ([pearx](https://pear.vc/pearx/), [boost](https://www.boost.vc/faq), [okta ventures](https://www.okta.com/en-ca/company/okta-ventures/)).

7. честный вердикт: тезис инвестируем, но сегодня актив доказывает **research capability**, а не существование control plane; перелом в конверсии создадут один внешний runtime-пилот, два независимых воспроизведения и один узнаваемый security-operator рядом с командой.

# 1. сколько можно поднять

## рынок действительно возник, но уже не пуст

dust 18 мая 2026 года заявил о $40m series b, 3,000+ организациях, 51k monthly active users, 300k созданных агентах, 240% nrr и отсутствии churn в 2025 году; это данные самой компании, а не независимый аудит ([анонс dust](https://www.globenewswire.com/news-release/2026/05/18/3296726/0/en/Dust-raises-40M-to-make-AI-multiplayer-inside-the-enterprise.html)). anthropic отдельно описывает 300k агентов dust, но customer story также опирается на сведения клиента ([anthropic](https://claude.com/customers/dust)).

рынок протоколов не исчезает, а становится нейтральным: anthropic передал mcp в linux foundation 9 декабря 2025 года и сообщил о 10k+ активных публичных mcp-серверах; google передал a2a linux foundation 23 июня 2025 года ([anthropic](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation), [google](https://developers.googleblog.com/google-cloud-donates-a2a-to-linux-foundation/)). в августе 2026 года a2a оказался в той же agentic ai foundation, что и mcp ([axios](https://www.axios.com/2026/08/17/a2a-agentic-ai-foundation-open-ai-standards)).

security-бюджет читается: в исследовании google cloud от 24 августа 2026 года 79% опрошенных technology leaders назвали security, governance или operations главным препятствием масштабированию inference, а 35% — недостаточную безопасность доступа агентов к нескольким системам; это vendor-sponsored survey, а не измерение расходов ([google cloud](https://cloud.google.com/blog/topics/ai-infrastructure/state-of-ai-infrastructure-report-agent-governance-and-security)).

## восемь строгих сравнений

| команда | сумма, стадия, дата | оценка | лид | что реально было в момент раунда | чем заменили выручку | воспроизводимо без имени |
|---|---:|---|---|---|---|---|
| codeintegrity | компания объявила **$5m seed, 27.05.2026**; geekwire сообщает **$4.8m** и $5.25m total — источники расходятся ([компания](https://www.codeintegrity.ai/blog/codeintegrity-seed-round), [geekwire](https://www.geekwire.com/2026/codeintegrity-raises-4-8m-to-put-permanent-guardrails-on-unpredictable-ai-agents/)) | не раскрыта | syn ventures; также antler и boost | pre-execution policy layer, provenance и evidence; команда показала компрометацию notion менее чем за четыре часа ([geekwire](https://www.geekwire.com/2026/codeintegrity-raises-4-8m-to-put-permanent-guardrails-on-unpredictable-ai-agents/)) | пилоты в регулируемых отраслях; named revenue не раскрыта | **высоко**: это ближайший comp для no-name команды, если exploit и защита внешне воспроизводятся |
| runlayer | **$11m seed, 17.11.2025** ([techcrunch](https://techcrunch.com/2025/11/17/mcp-ai-agent-security-startup-runlayer-launches-with-8-unicorns-11m-from-khoslas-keith-rabois-and-felicis/)) | не раскрыта | khosla ventures и felicis | после четырёх месяцев stealth — gateway, threat detection, observability, permissions и интеграции с okta/entra; назывались gusto, dbt labs, instacart и opendoor | десятки клиентов, включая восемь unicorn/public companies; основатель — serial founder, создатель mcp david soria parra — advisor | **низко**: размер опирался на клиентов, сеть и founder signal |
| barndoor | **$13.6m seed, 20.05.2025** ([barndoor](https://barndoor.ai/news/barndoor-ai-raises-13m-control-plane-agentic-ai/)) | не раскрыта | crosslink capital | granular access, visibility, audit и policy enforcement до выполнения запроса; продукт публично запущен вместе с раундом | named customers, arr и объём runtime не раскрыты; заменой был repeat-founder/API-management pedigree | **низко-средне**: технический пакет копируем, founder signal — нет |
| neuraltrust | **$20m seed, 17.06.2026** ([neuraltrust](https://neuraltrust.ai/news/neuraltrust-raises-20m)) | не раскрыта | alstin capital | agent gateway, runtime protection, posture management и red teaming; назывались aireuropa, abanca, iberia и banc sabadell | компания заявила о миллионах interaction/day, 1.2% malicious interactions и удвоении full-year 2025 arr уже в q1 2026; всё self-reported | **низко** без клиентов и telemetry |
| onyx security | **$40m funding при запуске, 12.03.2026**; позднее выяснилось, что это были $5m seed плюс $35m series a, а не один seed ([компания](https://www.onyx.security/press-release/onyx-security-launches-with-40m-in-funding-to-build-the-secure-ai-control-plane-for-the-agentic-era), [calcalist](https://www.calcalistech.com/ctechnews/article/b1fsjydszg)) | на запуске не раскрыта; оценка около $640m относится уже к июльскому series b, а не к seed | conviction и cyberstarts | secure ai control plane, несколько fortune 500 customers и команда с unit 8200/nvidia-профилем — по данным компании | named arr не раскрыта; заменой были pedigree, enterprise logos и значительный staffing | **очень низко** |
| keycard | **$8m seed + $30m series a, объявлены 22.10.2025; $38m cumulative** ([securityweek](https://www.securityweek.com/keycard-emerges-from-stealth-mode-with-38-million-in-funding/)) | не раскрыта | seed: a16z и boldstart; series a: acrew | cryptographic agent identity и contextual runtime authorization | бывшие senior leaders snyk и okta; named customers/arr в источнике отсутствуют | **низко**: executive pedigree заменил публичную traction |
| arcade | **$60m series a, 12.06.2026; $72m total**, следовательно предыдущий капитал составлял $12m, но дата и условия отдельного seed в этом источнике не раскрыты ([arcade](https://www.arcade.dev/blog/arcade-series-a/)) | не раскрыта | syn ventures; strategic participation morgan stanley и wipro | secure action layer, 8k tools, production в банках, industrial и pharma; компания участвовала в создании mcp authorization specification | компания заявила о росте tool-call volume в 25 раз за шесть месяцев; arr не раскрыта | **низко** без стандарта, production и объёма действий |
| geordie | **$6.5m seed в сентябре 2025; $30m series a, 28.05.2026** ([fortune](https://fortune.com/2026/05/28/geordie-security-governance-ai-agents/), [компания](https://www.geordie.ai/resources/geordie-raises-30m-to-help-enterprises-securely-adopt-agentic-ai-at-scale/)) | series a — около **$180m post-money** по оценке fortune на основе companies house; seed valuation не раскрыта | series a: balderton | к series a — governance/security platform, 30 customer environments, alph­asense и owkin; основатели из darktrace и snyk | компания заявила о 1,300% arr growth за пять месяцев; baseline arr не раскрыт | **низко** без pedigree и production |

### что сознательно не засчитано в строгие comps

- sapiom поднял $15.75m seed 6 февраля 2026 года на runtime-доступе, budgets и receipts для agent commerce, а затем $35m series a 5 августа 2026 года; это важный сосед, но его первый wedge — финансовые полномочия агента, не общий security control plane ([seed](https://www.sapiom.ai/resources/launch/), [series a](https://www.sapiom.ai/resources/blog/series-a/)).

- xpander объявил $7.5m seed 17 августа 2026 года, но позиционируется как vendor-neutral agent platform/orchestration, поэтому не является чистым security-runtime comp ([xpander](https://xpander.ai/blog/xpander-funding-seed)).

- sycamore поднял $65m seed 30 марта 2026 года, но это broad agentic operating system от бывшего atlassian cto и бывшего партнёра coatue; для вашей команды это pedigree-outlier, а не pricing anchor ([sycamore](https://sycamore.so/press-releases/sycamore-raises-65m-seed/), [techcrunch](https://techcrunch.com/2026/03/30/former-coatue-partner-raises-huge-65m-seed-for-enterprise-ai-agent-startup/)).

## прямое число

| состояние | реалистичный результат | условия |
|---|---:|---|
| текущие активы, хорошо собранный pitch, но без внешнего adoption | **$3.5–5.5m**; оптимальный ask **$5m** | codeintegrity — единственный близкий no-marquee anchor около $5m; внутренний fleet и сырой corpus сами по себе traction не заменяют |
| один внешний sandbox/pilot, три design-partner commitments, независимое воспроизведение wow, senior security operator рядом | **$6.5–8m** | появляется возможность сопоставляться с ранним geordie и нижней частью runlayer, не притворяясь равными их customer/pedigree signal |
| $10m+ | **сейчас не подтверждено** | найденные раунды этого уровня сопровождались repeat-founder pedigree, named customers, telemetry либо объединяли несколько стадий |

### control plane против простого mcp-аудита

- mcp-аудит — понятный исследовательский инструмент, но его легко прочитать как scanner, consulting или feature.

- runtime control plane подключается к security, identity, compliance и incident response, поэтому потенциальный budget surface шире. институциональное существование категории подтверждают codeintegrity, runlayer, barndoor, neuraltrust, arcade и onyx.

- однако широкая упаковка **не создаёт премию сама**. без независимого enforcement proof инвестор увидит три недостроенных продукта: scanner, fleet manager и resilience bus.

**вердикт:** сегодня комбо поднимает качество встреч, но финансовый потолок лишь с **$5m до примерно $5.5m**. после внешнего proof потолок перемещается к **$6.5–8m**, то есть на **$1.5–3m** выше узкого тезиса.

## на что просить $5m

это предлагаемая структура использования капитала, а не рыночная статистика:

| направление | доля |
|---|---:|
| non-bypassable execution boundary, policy engine, identity и protocol adapters | 30% |
| threat/action corpus, replay harness, detection research и disclosure | 20% |
| customer engineering для 3–5 design partners и enterprise integrations | 20% |
| high availability, chaos testing, performance, pentest и compliance evidence | 15% |
| senior security/runtime hire, ciso advisory и узкий founder-led gtм | 15% |

название раунда лучше не защищать: для seed-фондов это может быть seed, для accelerator/pre-seed инвесторов — pre-seed. предмет сделки — $5m на достижение внешнего runtime proof, а не семантика стадии.

# 2. позиционирование, которое конвертит

| формулировка | читаемость инвестору, моя оценка | профинансированные соседи | риск занятости | вердикт |
|---|---:|---|---|---|
| `mcp runtime control` | 8/10 | runlayer и codeintegrity получили $11m и около $5m соответственно ([runlayer](https://techcrunch.com/2025/11/17/mcp-ai-agent-security-startup-runlayer-launches-with-8-unicorns-11m-from-khoslas-keith-rabois-and-felicis/), [codeintegrity](https://www.codeintegrity.ai/blog/codeintegrity-seed-round)) | высокая протокольная зависимость; mcp уже в нейтральном фонде и будет обрастать platform controls ([anthropic](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation)) | хороший wedge или demo label, плохая долгосрочная категория |
| `agent zero-trust execution boundary` | 7/10 | codeintegrity продаёт pre-execution policies, arcade — secure action layer, okta — identity, authorization и kill switch ([codeintegrity](https://www.codeintegrity.ai/platform), [arcade](https://www.arcade.dev/blog/arcade-series-a/), [okta](https://www.okta.com/products/govern-ai-agent-identity/)) | фраза не занята одним монополистом, но требует объяснить, где физически находится boundary | лучший технический подзаголовок |
| `fleet governance & audit plane` | 8/10 | barndoor, onyx и geordie используют control/governance framing ([barndoor](https://barndoor.ai/news/barndoor-ai-raises-13m-control-plane-agentic-ai/), [onyx](https://www.onyx.security/press-release/onyx-security-launches-with-40m-in-funding-to-build-the-secure-ai-control-plane-for-the-agentic-era), [geordie](https://www.geordie.ai/resources/geordie-raises-30m-to-help-enterprises-securely-adopt-agentic-ai-at-scale/)) | microsoft agent 365 уже называет себя центральным control plane для identity, observability, governance, security и lifecycle ([microsoft](https://learn.microsoft.com/en-us/microsoft-agent-365/leadership/why-agent-365-for-enterprise)) | читаемо, но без enforcement звучит как dashboard |
| `ai-agent dlp` | 9/10 для ciso | palo alto networks уже объединяет dlp, runtime, identity и mcp/a2a gateway в prisma airs ([palo alto networks](https://www.paloaltonetworks.com/blog/2026/07/announcing-general-availability-of-prisma-airs-ai-gateway/)) | очень высокая вероятность bundling; формулировка не покрывает budgets, delegation, approvals и resilience | использовать как один use case, не как компанию |

## одна выбранная формулировка

> **agent action security — zero-trust runtime control plane для корпоративных флотов агентов.**

расшифровка одной строкой:

> каждое значимое действие агента авторизуется вне модели до исполнения: least privilege, provenance, human approval для необратимых действий, tamper-evident audit и kill/recovery.

почему это конвертит лучше:

1. `agent action security` сразу сообщает security buyer и конкретный объект защиты — действие, а не абстрактный «ai».

2. `runtime control plane` помещает компанию в уже профинансированную категорию, не ограничивая её одним mcp.

3. `zero-trust` раскрывается инженерно: модель не является policy authority; credentials и окончательное разрешение находятся за независимой границей.

4. `fleet` остаётся рынком расширения. «второй мозг сотрудников» не появляется как второй продукт и не размывает security wedge.

# 3. моат

## оценка кандидатов

| кандидат | реален ли no-name команде | что уже засеивает | время копирования, моя оценка | что seed-инвестор реально оплачивает |
|---|---|---|---|---|
| компаундящийся threat/action corpus | **да, но не в нынешнем сыром виде** | заявленные 30 cve, 21k instances, exploit research и ответы maintainers | публичный список и scanner: **2–8 недель**; нормализованный replay corpus с live telemetry, labels и false-positive feedback: **12–24 месяца** | внешнее воспроизведение, новые disclosures и уникальные runtime traces; не количество строк в базе |
| switching cost control plane | **да, после deployment** | внутренняя шина показывает способность строить stateful fleet systems, но внешний switching cost пока нулевой | gateway/ui: **6–12 месяцев**; перенос policy history, identity/action graph, approvals и audit evidence: **12–24 месяца** | 3+ production deployments, накопленные policy objects и исторический action graph |
| oss и стандарт-легитимность | **да как distribution, слабо как exclusion** | public repo и upstream conversations | fork кода: часы/недели; maintainer reputation, conformance suite и экосистема adapters: **12–24 месяца** | принятую спецификацию, upstream merge, neutral benchmark; не stars сами по себе |
| скорость и вкус команды | **да немедленно, но временно** | offensive research, внутренний self-healing fleet и способность быстро собирать артефакты | **1–3 месяца** после публичного demo | право на первую встречу и ранний чек; не долгосрочную премию |

### аналогия с crowdstrike — где она корректна

crowdstrike описывает threat graph и enterprise graph как объединяющий контекст своей security platform; это иллюстрация ценности накопленного cross-customer контекста, но не доказательство, что любой новый telemetry corpus автоматически создаёт network effect ([crowdstrike 10-k](https://ir.crowdstrike.com/static-files/5ec13e28-5688-4dec-a446-d746a9990808)).

для agent control plane ценный объект данных — не «ещё один вредоносный prompt», а связка:

`identity → delegated authority → resource → requested action → policy decision → actual result → remediation`.

именно эта схема позволяет:

- узнавать новые attack paths;

- улучшать готовые политики;

- переносить защиту между протоколами;

- доказывать ciso, почему действие было разрешено или остановлено.

### что не является моатом

- 30 cve без независимого воспроизведения — research backlog;

- 21k обнаруженных instances без dedup, методики и responsible disclosure — lead-generation сырьё;

- аудитория 169k — distribution asset, но не product adoption;

- внутренняя consensus/self-healing шина — technical credibility, но не switching cost клиента;

- oss-код — способ стать стандартом, но одновременно инструкция конкуренту.

### какой ров seed-инвестор оценивает деньгами

**ров-в-формировании: live threat/action graph плюс накопленная policy state.**

lakera получила $20m series a, опираясь в том числе на gandalf с более чем 50m security interactions; это пример финансирования data-loop до окончательной зрелости рва ([lakera](https://www.prnewswire.com/news-releases/lakera-raises-20m-series-a-to-secure-generative-ai-applications-302204874.html)). neuraltrust в $20m seed-анонсе заявила миллионы анализируемых interactions в день и долю malicious traffic 1.2%; это более близкий, но полностью self-reported пример ([neuraltrust](https://neuraltrust.ai/news/neuraltrust-raises-20m)).

arcade дополнительно показывает вторую половину рва: компания заявила 8k tools, 25-кратный рост tool calls и участие в mcp authorization specification ([arcade](https://www.arcade.dev/blog/arcade-series-a/)). здесь telemetry, integrations и standard legitimacy усиливают друг друга.

# 4. насколько это wow

## что заставит наклониться вперёд

- одна и та же независимая граница реально управляет действиями claude/mcp, a2a и прямого api, а не показывает три dashboard;

- credentials недоступны агенту до policy decision, поэтому enforcement нельзя обойти простым prompt injection;

- система объясняет всю цепочку делегирования: человек → агент → sub-agent → tool → ресурс → действие;

- live demo показывает не только block, но и human approval, kill, evidence и controlled recovery;

- два человека вне команды воспроизводят результат по публичному replay bundle.

## что заставит зевнуть

- «агенты будут везде, значит им нужна безопасность»;

- borrowed traction dust: 300k агентов dust не являются пользователями вашего продукта ([dust](https://dust.tt/));

- очередной mcp scanner с severity labels;

- общий dashboard логов;

- «детерминированный» без описания того, что именно детерминировано;

- «rollback всего» — для отправленного письма, платежа или утёкшего секрета универсальный откат невозможен;

- internal self-healing fleet без внешнего attack replay и deployment.

## отличие от соседей

| сосед | что обычно доказывает | чего ему не хватает | ваша проверяемая граница |
|---|---|---|---|
| posture/scanner | vulnerable server, tool или configuration существуют | не контролирует каждое последующее действие | решение непосредственно перед execution |
| prompt/output guardrail | классификатор считает текст опасным | модель или tool могут обойти text-only контроль | authority, identity, resource и action policy вне модели |
| observability | после события есть trace | evidence не равно prevention | deny/allow/approve до side effect |
| incumbent gateway | широкий bundle внутри конкретной platform ecosystem | возможны cross-vendor gaps и медленный coverage новых adapters | нейтральная boundary для нескольких agent stacks |

граница не уникальна сама по себе: palo alto networks уже заявляет inline llm/mcp/a2a controls, microsoft — central agent control plane, okta — agent identity, authorization и kill switch, а crowdstrike и nvidia — secure-by-design agent runtime ([palo alto networks](https://www.paloaltonetworks.com/blog/2026/07/announcing-general-availability-of-prisma-airs-ai-gateway/), [microsoft](https://learn.microsoft.com/en-us/microsoft-agent-365/leadership/why-agent-365-for-enterprise), [okta](https://www.okta.com/products/govern-ai-agent-identity/), [crowdstrike](https://ir.crowdstrike.com/node/16156/pdf)). следовательно, wow должен быть **доказательством лучшей и более нейтральной enforcement primitive**, а не изобретением слова control plane.

## есть ли wow сегодня

не подтверждено.

agent-leash, сырой vulnerability corpus, maintainer responses и внутренний fleet образуют хорошее сырьё, но из рамки не следует:

- независимое воспроизведение;

- защита внешнего production workload;

- доказанная non-bypassability;

- измеренные overhead и failover;

- разрешение реального ciso использовать своё имя.

codeintegrity получил пересказываемый hook: «notion был скомпрометирован менее чем за четыре часа» ([geekwire](https://www.geekwire.com/2026/codeintegrity-raises-4-8m-to-put-permanent-guardrails-on-unpredictable-ai-agents/)). у вас пока нет столь же короткого внешне доказанного предложения.

## самый дешёвый wow за 2–4 недели

### сценарий

в локальном безопасном стенде одна отравленная инструкция попадает к четырём разным агентам через документ, mcp tool description или delegated task. она пытается:

1. прочитать ресурс вне полномочий;

2. передать данные в неразрешённый endpoint;

3. выполнить privileged tool call;

4. передать право sub-agent;

5. совершить необратимое действие без человека.

### что должен сделать leash

- остановить запрещённые действия до tool call;

- показать полную delegation chain;

- разрешить безопасную часть задания;

- запросить human approval для необратимой части;

- выполнить compensating action только там, где откат технически возможен;

- пережить выключение одного control-plane node без потери audit record;

- экспортировать evidence в распространённый siem-формат.

### минимальные критерии доказательства

- публичный replay bundle без живых целей и секретов;

- adapters минимум для двух протокольных путей и трёх agent implementations;

- два независимых воспроизведения: security maintainer и ciso/security engineer;

- measured install time, p50/p95 overhead, false blocks, missed blocks и failover behaviour;

- responsible disclosure до публикации, если используется реальная уязвимость;

- короткое видео, raw logs и одна команда воспроизведения.

### пересказываемая строка

> одна отравленная задача попыталась вывести payroll-данные через четыре разных агента; одна независимая execution boundary остановила все четыре до side effect, показала, кто делегировал доступ, и два внешних инженера воспроизвели результат.

# 5. максимум конверсии в «да»

| возражение | сильнейший честный контр-ход | артефакт | закрываемо за 30 дней |
|---|---|---|---|
| «это купит или скопирует microsoft, wiz, crowdstrike, palo alto networks или okta» | не заменять их: стать cross-vendor enforcement boundary и поставщиком evidence/signals. incumbent products уже подтверждают бюджет, но их bundling означает, что нейтральность должна быть измерима ([microsoft](https://techcommunity.microsoft.com/blog/agent-365-blog/what%E2%80%99s-new-in-agent-365-may-2026/4516340), [palo alto networks](https://www.paloaltonetworks.com/blog/2026/07/announcing-general-availability-of-prisma-airs-ai-gateway/), [okta](https://www.okta.com/products/govern-ai-agent-identity/)) | одна policy работает поверх 3–4 agent stacks, okta/entra identity и siem export; comparison matrix с точной границей продукта | **да** для prototype |
| «это feature, не компания» | land: pre-action authorization; expand: action registry, identity/delegation graph, policy lifecycle, threat graph, incident response и resilience. не обещать пять продуктов сразу | architecture и 18-месячная последовательность модулей; design partner подтверждает, что проблема принадлежит ciso | **да**, кроме доказанной expansion revenue |
| «нет внешнего adoption» | не спорить. заменить внутреннюю историю тремя signed design-partner commitments и одним работающим sandbox/pilot | имя budget owner, use case, production decision date, success criteria; минимум две разрешённые investor reference calls | **частично**: commitments реалистичны, настоящий production зависит от клиента |
| «mcp и a2a стали открытыми стандартами — вы строите замок вокруг общего протокола» | policy object — не mcp-specific: `principal/resource/action/context`; mcp и a2a лишь adapters. нейтральный фонд ускоряет deployment, но не устраняет enterprise enforcement ([anthropic](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation), [google](https://developers.googleblog.com/google-cloud-donates-a2a-to-linux-foundation/)) | одинаковая policy через mcp, a2a и прямой rest/browser action; conformance tests и upstream proposal | **да** для demo, upstream acceptance — **частично** |
| «команда без cyber-pedigree» | заменить pedigree верифицируемым research output и borrowed trust, не выдуманными титулами | два внешних exploit reproductions, один respected security maintainer/former ciso advisor, third-party pentest | advisor/reproductions — **да**; senior hire — **частично** |
| «control plane станет latency bottleneck и single point of failure; агент всё равно обойдёт его» | credentials выдаются только через boundary; high-risk egress идёт через controlled path; fail-open/fail-closed задаётся по классу действия. не обещать откат необратимого | bypass test, chaos test, credential isolation, p95 overhead, tamper evidence, node failover и documented irreversible-action policy | **да** для controlled benchmark |
| «флоты ещё не production и никто не знает buyer» | не доказывать рынок одним dust. провести buyer proof с ciso, identity и ai-platform владельцами; vendor telemetry использовать лишь как подтверждение направления. palo alto networks сообщил рост наблюдаемой mcp-активности с 11% в конце 2025 до 41.4% к середине 2026 года, но это только его telemetry ([palo alto networks](https://www.paloaltonetworks.com/blog/2026/07/announcing-general-availability-of-prisma-airs-ai-gateway/)) | 10 структурированных ciso interviews, 3 design partners, для каждого — текущий workaround, budget owner, цена инцидента, willingness-to-pay и дата решения | **да** |

## что снять ещё до первого разговора

1. первый слайд — security action boundary, не «второй мозг».

2. fleet market — один слайд evidence, не отдельная продуктовая ветка.

3. внутренний consensus/self-healing fleet — доказательство engineering ability на техническом appendix-слайде.

4. 30 cve и 21k instances — только после публикации методики, dedup и независимого reproduction.

5. слово deterministic — с точным контрактом: authority и policy decision детерминированы; semantic risk scoring может оставаться вероятностным.

6. rollback — только для transactional/compensatable actions. для необратимых действий продукт должен требовать pre-commit approval.

# 6. кто скажет «да» быстрее

## приоритетный список

| инвестор | почему тезис релевантен | дверь без интро | ограничение или конфликт |
|---|---|---|---|
| paladin capital group | официальный фокус на cyber, digital infrastructure resilience и secure/trustworthy ai; публично принимает планы и пишет чеки до $15m ([форма](https://www.paladincapgroup.com/entrepreneurs/submit-a-business-plan/), [secure ai](https://www.paladincapgroup.com/ai-investing/)) | `[email hidden]` | criteria требуют proven technology, relevant customers и strong team; лучший кандидат на lead после wow/design partners |
| sinewave ventures | позиционирует cyber как часть управления smart systems, что хорошо совпадает с security+resilience control plane ([контакт](https://sinewave.vc/contact/), [платформенный тезис](https://sinewave.vc/build-a-platform-sell-a-solution/)) | `[email hidden]` | прямой agent-runtime conflict в найденных источниках не подтверждён; проверить перед outreach |
| decibel | инвестирует с earliest stage в software для engineering/security teams и заявляет сеть ciso/cto early adopters ([decibel](https://www.decibel.vc/)) | фонд прямо пишет, что исследованный cold email может работать ([decibel о ciso outreach](https://www.decibel.vc/articles/how-to-sell-to-the-ciso-with-mike-hanley-ciso-of-duo-security-and-cisco)) | формальная pitch-форма не найдена; нужен сильный technical hook |
| syn ventures | чистый cyber-фонд с $900m aum и публичной contact form; инвестирует от formation stage ([syn](https://www.synventures.com/)) | публичная форма | прямой конфликт: syn уже инвестировал в codeintegrity и arcade ([codeintegrity](https://www.codeintegrity.ai/blog/codeintegrity-seed-round), [arcade](https://www.arcade.dev/blog/arcade-series-a/)); использовать для category validation, но вероятность сделки может быть ниже |
| ballistic ventures | специализируется исключительно на cybersecurity и имеет сеть ciso/operators ([ballistic](https://ballisticventures.com/about/)) | публичный общий адрес `[email hidden]`, но фонд не обещает рассматривать unsolicited materials ([условия](https://ballisticventures.com/privacy-policy/)) | direct/adjacent conflict: witnessai уже финансируется ballistic и продаёт safe use/security for ai ([witnessai](https://witness.ai/resources/witnessai-raises-27-5-million-to-enable-safe-use-of-ai/)) |
| ten eleven ventures | multi-stage cyber specialist; в 2026 прямо инвестировал в secure enterprise agent deployment ([ten eleven](https://www.1011vc.com/), [variance thesis](https://www.1011vc.com/news/why-we-invested-in-variance)) | публичный сайт и офисные контакты; специализированная submission form не подтверждена | прямой конфликт через geordie; идти только с чётким отличием от governance posture |
| boost vc | готов быть первым или единственным pre-seed check: стандартно $500k, диапазон $100k–$1m и открытая application ([boost](https://www.boost.vc/faq)) | открытая заявка | уже инвестировал в codeintegrity; не lead для полного $5m, но возможный быстрый signal |
| pearx | на 25.08.2026 открыт w27, regular deadline 04.10.2026; открытая заявка, solo founders принимаются, check $500k–$2m ([pearx](https://pear.vc/pearx/)) | полностью открытая application | generalist; нужно показать why-now и learning velocity, а не cyber jargon |
| y combinator | принимает late applications в fall 2026 и заявляет, что решения после интервью обычно принимаются в тот же день ([yc apply](https://www.ycombinator.com/apply)) | открытая application | стандартная сделка — $500k, не полный $5m round ([yc terms](https://www.ycombinator.com/blog/ycs-500-000-standard-deal)) |
| michael sutton | бывший ciso zscaler; публично говорит, что инвестирует на early seed, включая first-time founders и cyber-adjacent ai ([профиль decibel](https://www.decibel.vc/articles/michael-sutton-former-ciso-of-zscaler-and-angel-investor-in-orca-runzero-greynoise-and-fleetdm)) | прямого pitch form нет; cold outreach через публичную professional network | angel, не lead; ценность — credibility и ciso references |
| okta ventures | agent identity и delegated authorization — прямое соответствие; публично рассматривает submissions, early checks обычно меньше $1m ([okta ventures](https://www.okta.com/en-ca/company/okta-ventures/)) | application form | не lead: требует уже подтверждённого lead и market demand; полезен после design partner |
| crowdstrike falcon fund | runtime, threat graph и security telemetry дают стратегическую совместимость | публичная funding application ([falcon fund](https://go.crowdstrike.com/FalconFund.html)) | форма требует domestic c-corp и описания использования falcon api/endpoint agent; без интеграции подаваться рано |

отдельно: yl ventures публикует прямой контакт для seed cybersecurity, но указанный call ориентирован на israeli-based startups; для palo alto-команды без израильской структуры fit не подтверждён ([yl ventures](https://www.ylventures.com/wp-content/uploads/2022/08/2022_CISO-Circuit-Report_E-8.pdf)).

## наиболее вероятная последовательность «да»

1. быстрый первый чек/credential: boost, pearx или yc;

2. первый возможный institutional lead: paladin, sinewave или decibel;

3. exact-category funds: syn, ballistic, ten eleven — после conflict check и чёткого differentiation;

4. strategic checks: okta и crowdstrike после lead, pilot и product integration.

wiz ventures и palo alto networks venture arm не включены: публичная founder-submission дверь, удовлетворяющая условию «без интро», в проверенных источниках не подтверждена.

# синтез

## 1. одностраничное инвест-мемо

**тезис-строка**

> agent action security: zero-trust runtime control plane, который авторизует каждое значимое действие корпоративного агента вне модели до исполнения и сохраняет проверяемую цепочку полномочий.

**запрос**

**$5m**. это соответствует наиболее близкому no-marquee comp codeintegrity, объявившему $5m seed 27 мая 2026 года ([codeintegrity](https://www.codeintegrity.ai/blog/codeintegrity-seed-round)). относительно предоставленного заказчиком ориентира $4.5m премия составляет 11%; сам ориентир $4.5m в доступном публичном тексте carta независимо не подтверждён. carta подтверждает лишь более общий режим: в 2025 году капитал вырос, количество раундов упало до шестилетнего минимума, а деньги концентрировались в меньшем числе ai-сделок ([carta](https://carta.com/data/state-of-private-markets-q4-2025/)).

**что покупает раунд**

- внешне проверяемую execution boundary;

- adapters mcp, a2a и direct actions;

- 3–5 design partners;

- threat/action graph на реальных runtime events;

- high availability, bypass и chaos evidence;

- senior security/runtime credibility.

**категория**

`agent action security — zero-trust runtime control plane for enterprise agent fleets`.

**клин**

pre-action security: authority, data access, tool execution, human approval, logs и containment.

**рынок расширения**

fleet governance, identity/delegation graph, incident response, audit и resilience. employee second brain — причина появления флота, не второй sku.

**ров-в-формировании**

proprietary threat/action graph + switching cost накопленных policies, identities, approvals и audit history.

**wow**

один poisoned task атакует четыре heterogeneous agents; boundary блокирует запрещённые действия до side effect, сохраняет разрешённую работу, требует человека для необратимого шага и переживает отказ node. два внешних инженера воспроизводят replay.

**топ-3 возражения**

1. incumbents скопируют — ответ: neutral cross-vendor enforcement и evidence integration; proof — одна policy поверх нескольких stacks.

2. feature — ответ: pre-action wedge расширяется в action/identity graph и response; proof — ciso design partner подтверждает отдельного budget owner.

3. нет adoption — ответ: не спорить; закрыть три design partners, один pilot и две investor reference calls.

**целевой lead**

не конфликтующий cyber-seed fund с ciso network и formation-stage mandate: сначала paladin/sinewave/decibel; первый небольшой «да» — boost/pearx/yc; strategic — после lead.

**финансовый статус**

- вероятное закрытие сейчас: $3.5–5.5m;

- выбранный ask: $5m;

- stretch после 30-дневного proof package: $6.5–8m;

- $10m+ без named production customer или heavyweight security/operator signal — не подтверждено.

## 2. честный вердикт: комбо против узкого mcp-аудита

**комбо-тезис даёт больше конверсии в «да»**, потому что:

- переводит продукт из scanner/research в control point;

- показывает самостоятельный security budget;

- переживает смену mcp на a2a или иной transport;

- создаёт правдоподобный land-and-expand.

но улучшение условно:

| состояние | изменение потолка против $3–5m mcp-аудита |
|---|---:|
| только новая формулировка | **$0–0.5m** |
| wow + два независимых reproduction + три design partners | **+$1.5–3m**, потолок около $6.5–8m |
| named production customer и senior cyber operator | возможно выше, но текущими данными **не подтверждено** |

главная опасность комбо — не размер идеи, а потеря фокуса. если в первых пяти минутах звучат одновременно mcp scanner, fleet consensus, employee brain, self-healing и security platform, конверсия будет ниже узкого аудита. если первая покупаемая единица — pre-action security boundary, а всё остальное является расширением, конверсия выше.

## 3. пять действий на 30 дней

1. **собрать и внешне воспроизвести wow replay.**  
   цена низкая относительно эффекта; это заменяет абстрактный pitch одним предложением для investment committee.

2. **получить три ciso-backed design-partner commitments и один sandbox/pilot.**  
   в документе должны быть budget owner, use case, success criteria, production decision date и разрешение на investor reference.

3. **доказать protocol-neutral enforcement.**  
   одна policy schema через mcp, a2a и direct api; credentials за boundary; измеренные overhead, bypass и failover.

4. **превратить vulnerability сырьё в проверяемый corpus.**  
   dedup, reproduction scripts, severity method, responsible disclosure, два внешних подтверждения и первый публичный benchmark.

5. **добавить borrowed trust и сразу сегментировать fundraise.**  
   один former ciso/security maintainer или senior runtime-security hire; отдельно вести unconflicted cyber leads, accelerators и strategics с уже найденным lead.

# чему нельзя верить

- **self-reported arr и growth.** 240% nrr dust, 1,300% arr growth geordie, удвоение arr neuraltrust и четырёхкратный revenue growth onyx не являются аудитом ([dust](https://www.globenewswire.com/news-release/2026/05/18/3296726/0/en/Dust-raises-40M-to-make-AI-multiplayer-inside-the-enterprise.html), [geordie](https://www.geordie.ai/resources/geordie-raises-30m-to-help-enterprises-securely-adopt-agentic-ai-at-scale/), [neuraltrust](https://neuraltrust.ai/news/neuraltrust-raises-20m), [onyx](https://www.onyx.security/press-release/onyx-security-raises-113m-series-b-to-control-advanced-ai-quadrupling-revenue-since-stealth-launch-four-months-ago)).

- **cumulative funding как свежий чек.** keycard $38m — это $8m seed плюс $30m series a; onyx launch funding $40m — $5m seed плюс $35m series a ([keycard](https://www.securityweek.com/keycard-emerges-from-stealth-mode-with-38-million-in-funding/), [onyx](https://www.calcalistech.com/ctechnews/article/b1fsjydszg)).

- **commitment или announcement как полностью funded cash.** без банковских данных и filing нельзя подтвердить, сколько денег уже поступило и на каких условиях.

- **оценку, вычисленную из размера раунда.** большинство рассмотренных valuations не раскрыты; geordie около $180m и onyx около $640m основаны на внешних оценках/filings и относятся к более поздним стадиям ([fortune](https://fortune.com/2026/05/28/geordie-security-governance-ai-agents/), [calcalist](https://www.calcalistech.com/ctechnews/article/b1fsjydszg)).

- **размер компании-клиента как adoption продукта.** банк со 100k сотрудниками не означает 100k активных пользователей control plane; 300k созданных dust agents не означает 300k ежедневно исполняющихся агентов.

- **vendor telemetry как весь рынок.** данные palo alto networks отражают наблюдаемую им инфраструктуру, а survey google cloud — опрошенную выборку, а не универсальную enterprise population ([palo alto networks](https://www.paloaltonetworks.com/blog/2026/07/announcing-general-availability-of-prisma-airs-ai-gateway/), [google cloud](https://cloud.google.com/blog/topics/ai-infrastructure/state-of-ai-infrastructure-report-agent-governance-and-security)).

- **маркетинговые «largest seed».** сравнение меняется от географии, определения стадии, cumulative/fresh funding и включения convertible instruments.

- **открытую форму как интерес фонда.** форма подтверждает возможность податься без интро, но не отсутствие portfolio conflict и не вероятность ответа.

# непроверенное и что осталось за вебом

1. точные valuations codeintegrity, runlayer seed, barndoor, neuraltrust, keycard, arcade seed и geordie seed — не раскрыты в найденных источниках.

2. form d, фактически перечисленные суммы, safe caps, liquidation preferences, dilution и secondary components рассмотренных сделок — не проверены.

3. независимый arr, customer count, retention и production usage всех частных comps — не подтверждены.

4. заявленные 30 cve и 21k vulnerable instances — по условию не проверялись; их dedup, severity, live exposure и воспроизводимость неизвестны.

5. независимая безопасность, latency, availability и non-bypassability agent-leash — не подтверждены.

6. юридическое владение кодом, contributor agreements, лицензия и возможность коммерческого relicensing — не проверены.

7. portfolio conflicts paladin, sinewave и decibel на 25 августа 2026 года — в публичных источниках точного совпадения не найдено, но отсутствие конфликта не доказано.

8. публичная no-intro founder intake для wiz и venture arm palo alto networks — не подтверждена.

9. точная медиана seed/pre-seed к концу 2026 года ещё не существует; доступный carta q4 2025 review подтверждает концентрацию капитала и снижение числа сделок, но не независимо подтверждает указанную в задании медиану $4.5m ([carta](https://carta.com/data/state-of-private-markets-q4-2025/)).

10. не подтверждено, что все 3,000+ организаций dust являются платящими клиентами или что все 300k агентов активны; обе цифры происходят из заявлений dust и customer-story materials.
