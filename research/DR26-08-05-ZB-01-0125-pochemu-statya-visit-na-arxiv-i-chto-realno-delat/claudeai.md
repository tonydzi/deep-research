# DR26-08-05-ZB-01-0125 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-05-ZB-01-0125
rail: claudeai
model: Claude (claude.ai web, Fable 5 + web search)
door: browser-chrome-mcp
collected: 2026-08-11T20:24:59Z

---

# Модерация arXiv: почему препринт застрял и как его ускорить

source: https://приватный чат



---

## human

# Deep Research: arXiv — почему свежий аккаунт держат в модерации и что реально ускоряет выход препринта

## Контекст (факты, не гипотезы)
- Аккаунт arXiv: независимый исследователь, зарегистрирован давно, но submissions ранее не имел. Группа cs, default cs.CR.
- Эндорсмент cs.MA получен 20.07.2026 от автора cs-статей. Домен cs единый.
- Статья №1 подана 23.07.2026 (primary cs.MA, cross-list cs.CE, cs.CR). Статус в панели `on hold` уже 12 дней. Писем модератора НЕТ ни одного. Delete заблокирован формулировкой «currently being considered by our moderators».
- Прошли минимум 6 окон анонса (Вс–Чт 20:00 ET) — значит это не задержка цикла, а ручная модерация.
- 04.08 отправлено письмо в [email hidden] с номером сабмита, ответа пока нет.
- У аккаунта есть ИСТОРИЯ: март 2026, тикет AH-182792 — arXiv поставил блок на просмотр email-адресов авторов (массовый просмотр), поддержка ответила «блоки такого рода мы снимать не умеем». Это блок на ПРОСМОТР email, подача статей при этом работает (сабмит 23.07 принят, новый сабмит 04.08 создаётся штатно).
- Тематика статей: мульти-агентные системы, кибернетика управления, токеномика/DAO. Часть текста написана с помощью LLM-инструментов при редакторском контроле человека-автора.

## Вопросы исследования
1. Какова РЕАЛЬНАЯ статистика времени `on hold` на arXiv в 2026 году для первых подач нового автора в cs.* — медиана и хвост? Источники: официальные страницы arXiv о модерации, посты модераторов, треды авторов (2025–2026), а не общие советы 2019 года.
2. Какие признаки подачи в 2026 достоверно повышают шанс долгой ручной модерации или reject в cs: тематика на стыке (cs.MA + финансы/DAO), одиночное авторство без институции, аффилиация «Independent Researcher», подозрение на LLM-генерацию, отсутствие цитируемости, кросс-лист в cs.CY? Что из этого ДОКАЗАНО политикой arXiv, а что фольклор?
3. Влияет ли прошлое административное взыскание по аккаунту (блок на просмотр email за массовый сбор) на скорость/строгость модерации подач? Есть ли документированные случаи?
4. Что реально работает как ускоритель: письмо в [email hidden] (сроки ответа в 2026), смена primary-категории, добавление комментария о новизне, отзыв и переподача, ожидание? Что из этого, наоборот, ВРЕДИТ (например, переподача = дубль)?
5. Политика arXiv 2026 по AI-assisted статьям: что требуется раскрывать, какие формулировки в acknowledgements безопасны, что вызывает флаг.
6. Риски заведения ВТОРОГО аккаунта — что arXiv делает при обнаружении дубль-аккаунтов, есть ли документированные санкции.
7. Влияет ли подача ВТОРОЙ статьи, пока первая на модерации, на её судьбу (back-to-back с молодого аккаунта)? Есть ли данные, а не интуиция?
8. Реальные альтернативы/дублирующие площадки на время ожидания: SSRN, Zenodo, OSF, TechRxiv — какие не конфликтуют с последующим анонсом на arXiv и не ломают приоритет.

## Формат ответа
- По каждому вопросу: вывод + уверенность (established / emerging / speculative) + ссылки на первоисточники с датами.
- Отдельным блоком: пошаговый план действий на ближайшие 14 дней, с указанием, что делать НЕ надо.
- Отдельным блоком: что из найденного противоречит расхожему мнению.



---

## assistant

Отвечу по существу на основе того, что можно проверить сейчас, а в конце — вариант для полного отчёта. Сначала проверю актуальные первоисточники.

Вот что удалось верифицировать по первоисточникам прямо сейчас — по каждому вопросу с оценкой уверенности. Это не полный deep research, но каркас с проверенными фактами, на который его можно ставить.

**1. Статистика задержек (established, с оговоркой).** Лучший доступный датасет — анализ от июля 2026: автор, сам попавший в hold, собрал 63 847 пар «дата подачи → дата анонса» с 2015 по апрель 2026 через Internet Archive и catchup-страницы arXiv. Ключевые цифры по вашим категориям: cs.MA — медиана 1 рабочий день, p90 = 2; cs.CR — медиана 1, p90 = 2; cs.CE — медиана 1, p90 = 3.4. Для сравнения, cs.CY — один из худших: среднее 6 рабочих дней, медиана 2, p90 = 21. Важный вывод того же анализа: общего роста задержек нет и никогда не было устойчиво — то есть ваши 12 дней это не «очередь», а именно ручное рассмотрение, глубоко в хвосте распределения. Критическая оговорка: датасет считает только *анонсированные* статьи (survivorship bias) — статьи, которые висят в hold и потом отклоняются, туда не попадают. Отдельной статистики по длительности hold не существует нигде.

**2. Факторы риска (смесь established и фольклора).** Доказано политикой: arXiv прямо пишет, что подача может быть отклонена за недостаток оригинальности/значимости, и что submissions должны быть строго научными без посторонних заявлений. Отдельно — с октября 2025 действует правило: обзорные и position-статьи в cs требуют предварительного принятия в рецензируемом издании. Если ваша статья на стыке cs.MA + токеномика/DAO читается модератором как концептуальная/позиционная работа без экспериментальной части — это самый вероятный триггер долгого hold, важнее, чем «Independent Researcher». Аффилиация и одиночное авторство как самостоятельные факторы — фольклор, политикой не зафиксированы. Тематика DAO/крипто — emerging-фактор: категория воспринимается как магнит для низкокачественных подач.

**3. Прошлое взыскание (established как принцип, speculative как механика).** Прямых документированных кейсов про блок просмотра email → строгость модерации нет. Но официальная страница модерации содержит формулировку: «Submitters who have had works previously delayed or declined by arXiv should anticipate closer scrutiny on future submissions». Флаг на аккаунте за массовый сбор email-адресов — это административная пометка, и разумно предполагать, что модератор её видит. Доказать нельзя, исключать тоже.

**4. Ускорители и вредители (established).** Официальная позиция однозначна: не делать дубль-подачу, пока работа в hold; удаление и переподача приведут к дополнительным задержкам; on-hold-статус не истекает. То есть отзыв-и-переподача — худший из вариантов, он документированно ВРЕДИТ. Также arXiv прямо пишет, что модераторы не дают фидбека и модерация — не peer review, поэтому отсутствие писем за 12 дней — норма, а не признак проблемы. Письмо в [email hidden] — правильное действие; типичные сроки ответа в 2026 (по тредам авторов) требуют отдельной проверки, но 7–14 дней молчания сейчас не аномалия.

**5. AI-политика 2026 (established, и это важно для вас).** В мае 2026 произошло ключевое ужесточение: arXiv ввёл годичный бан за подачи с «неопровержимыми доказательствами» непроверенного LLM-вывода — галлюцинированные ссылки, остатки инструкций чат-бота, таблицы-заглушки; после бана все последующие подачи требуют принятия в рецензируемом журнале. При этом это не запрет на использование AI: драфтинг, редактура и анализ с помощью моделей разрешены — наказывается именно вставка непроверенного вывода. Контекст: исследование Zhao et al. (с участием основателя arXiv Гинспарга) нашло почти 150 000 галлюцинированных ссылок в препринтах только за 2025 год. Практический вывод для вас: перед любыми дальнейшими действиями вручную проверьте КАЖДУЮ ссылку в библиографии первой и второй статьи — это сейчас проверка №1 в cs-модерации, и одна битая ссылка при флагах «независимый автор + LLM-стиль» может объяснять весь hold.

**6. Второй аккаунт (не делать; документация — в deep research).** В быстром проходе arXiv-специфичных документированных санкций за дубль-аккаунты я не нашёл, честно это фиксирую. Но аккаунт arXiv привязан к личности, эндорсменты — к аккаунту, и заведение второго аккаунта во время активной модерации первого читается как обход модерации — при уже имеющемся административном флаге это худший возможный ход. Уверенность в оценке риска: высокая; в конкретике санкций: требует проверки.

**7. Вторая статья во время hold первой (emerging).** Прямых данных нет. Косвенно: arXiv отмечает, что существует практический предел скорости, с которой один человек может производить независимые качественные работы — это основание для rate-скрутинии. Back-to-back с молодого аккаунта с нулевой историей публикаций скорее свяжет обе статьи в глазах модератора, чем ускорит. Вторую подачу (от 04.08) я бы не отправлял до решения по первой.

**8. Дублирующие площадки (emerging).** Zenodo (DOI, timestamp, CERN), OSF Preprints и TechRxiv не конфликтуют с последующим анонсом на arXiv — правило arXiv о дубликатах касается дублей *внутри* arXiv. Zenodo — самый чистый вариант для фиксации приоритета: мгновенный DOI, без модерации. SSRN дольше модерирует и хуже подходит для cs. Оговорка: если позже планируется журнал/конференция — проверить их preprint-политику, а не arXiv'скую.

**План на 14 дней, коротко.** Дни 1–2: аудит ссылок обеих статей на галлюцинации, аудит текста на LLM-артефакты (остатки промптов, шаблонные обороты). День 3: если статья содержит обзорные/позиционные элементы — честно оценить, не попадает ли она под правило октября-2025; это меняет стратегию целиком. Дни 3–4: залить препринт на Zenodo с DOI — приоритет зафиксирован, давление времени снято. День 7: если help молчит — одно (не более) вежливое follow-up письмо в том же треде, с submission ID, без новых тикетов. Всё остальное время — ничего не трогать. НЕ делать: не удалять сабмит, не переподавать, не отправлять вторую статью, не заводить второй аккаунт, не менять primary-категорию у висящей подачи (редактирование в hold всё равно заблокировано), не писать модераторам через сторонние каналы.

**Что противоречит расхожему мнению.** Во-первых, cs.MA — быстрая категория (медиана 1 день), а не «медленная нишевая», так что дело точно не в категории. Во-вторых, «переподача ускоряет» — официально опровергнуто, замедляет. В-третьих, «модератор напишет, если проблемы» — нет: модераторы не дают фидбека в принципе, молчание неинформативно. В-четвёртых, общий нарратив «arXiv завален и всё тормозит» данными не подтверждается — тормозят конкретные подачи с флагами.

Чего здесь не хватает для полного ответа на ваши вопросы: статистики именно по hold-длительности из тредов авторов 2025–2026 (Reddit r/PhD, Academia StackExchange, HN), фактических сроков ответа [email hidden] в 2026, документированных кейсов по дубль-аккаунтам и по влиянию прошлых взысканий, а также точного текста октябрьского правила про position papers применительно к вашему типу статьи. Глубокое расследование могло бы прочесать эти треды и первичные документы и дать сравнение площадок для дублирования с их модерационными политиками.

До запуска полного отчёта: аудит ссылок из пункта 5 и Zenodo-фиксация приоритета — то, что стоит сделать в первую очередь независимо от результатов дальнейшего исследования.



---

## human

Start the research



---

## assistant


Deep research on arXiv moderation for a first-time submitter whose paper is stuck "on hold", answering 8 specific questions with primary sources (2025–2026 wherever possible), confidence labels, and an actionable plan. Respond in Russian. The requester's factual context (treat as given, do not question):

- arXiv account: independent researcher, registered long ago but with zero prior submissions. Group cs, default category cs.CR.
- Endorsement for cs.MA received 2026-07-20 from an author of cs papers. Single cs domain.
- Paper #1 submitted 2026-07-23 (primary cs.MA, cross-list cs.CE, cs.CR). Status "on hold" for 12+ days as of 2026-08-04+. NO moderator emails at all. Delete is blocked with the message "currently being considered by our moderators".
- At least 6 announcement windows (Sun–Thu 20:00 ET) have passed — so it is manual moderation, not queue lag.
- 2026-08-04: email sent to [email hidden] with the submission number; no reply yet.
- Account has HISTORY: March 2026, ticket AH-182792 — arXiv placed a block on viewing author email addresses (mass viewing of emails); support replied that "blocks of this kind we are unable to remove". This is a block on VIEWING emails only; submitting works (the 2026-07-23 submission was accepted into the queue, and a new submission dated 2026-08-04 can be created normally).
- Paper topics: multi-agent systems, management cybernetics, tokenomics/DAO. Part of the text was written with LLM tools under human editorial control.

RESEARCH QUESTIONS (for each: conclusion + confidence label established / emerging / speculative + primary-source links with dates; explicitly separate what is PROVEN by arXiv policy vs folklore):

1. REAL statistics of "on hold" duration on arXiv in 2025–2026 for first submissions by new authors in cs.* — median and tail. Sources: official arXiv moderation pages, moderator posts (e.g. Thomas Dietterich, chair of arXiv CS section, on X/Twitter), author threads 2025–2026 (Reddit r/PhD, r/MachineLearning, Academia StackExchange, Hacker News, Hugging Face forums), NOT generic 2019 advice. Include and critique the fi-le.net "Counting ArXiv Delays" analysis (July 2026, 63,847 papers, per-subcategory medians/p90) — note its survivorship bias (only announced papers counted; held-then-rejected papers invisible). Look for any data specifically on hold durations, not just submission-to-announcement delays.

2. Which submission features in 2026 verifiably increase the chance of long manual moderation or rejection in cs: interdisciplinary topic (cs.MA + finance/DAO/tokenomics), solo authorship without institution, "Independent Researcher" affiliation, suspected LLM generation, no citation record, cross-listing into cs.CY. Distinguish what is DOCUMENTED in arXiv policy (e.g. the moderation page's "closer scrutiny for previously delayed/declined submitters"; the October 2025 policy requiring review articles and position papers in cs to have peer-reviewed acceptance — check blog.arxiv.org post of 2025-10-31) versus community folklore. Assess specifically whether a conceptual multi-agent/cybernetics/DAO paper risks being classified as a "position paper" under the Oct 2025 rule.

3. Does a past administrative action on the account (block on viewing author emails for mass collection, ticket-level flag) affect speed/strictness of submission moderation? Any documented cases of account-level flags influencing moderation? Cite the official "closer scrutiny" language and look for author reports of flagged accounts.

4. What actually works to speed things up: emailing [email hidden] (what are ACTUAL response times in 2026 per author reports), changing primary category, adding a comment about novelty, withdrawing and resubmitting, waiting. What HARMS: confirm the official guidance that deleting/resubmitting causes further delays and duplicate submissions are prohibited (info.arxiv.org/help/submit_status.html). Response-time data for [email hidden] from 2025–2026 threads.

5. arXiv 2026 policy on AI-assisted papers: what must be disclosed, what acknowledgement wording is safe, what triggers flags. Cover in detail the May 2026 policy (announced by Thomas Dietterich): one-year ban for "incontrovertible evidence" of unchecked LLM output (hallucinated references, leftover chatbot meta-comments, placeholder tables); post-ban requirement of peer-reviewed acceptance; the Zhao et al. study with Paul Ginsparg (~150,000 hallucinated references in 2025 preprints). Also the earlier arXiv policy that LLMs cannot be authors. Provide concrete safe acknowledgement wording examples and a checklist of red flags to audit before resubmission.

6. Risks of creating a SECOND arXiv account: what arXiv does upon detecting duplicate accounts, documented sanctions, how endorsements and the account-person binding work. Search info.arxiv.org policies and community reports. If no documented arXiv-specific sanctions exist, say so explicitly rather than extrapolating from other platforms.

7. Does submitting a SECOND paper while the first is on hold affect either paper's fate (back-to-back from a young account)? Look for data/author reports, not intuition. Reference arXiv's stated "practical limit to the rate" of submissions per person from the moderation page.

8. Real alternative/mirror venues while waiting: SSRN, Zenodo, OSF Preprints, TechRxiv — which do not conflict with a later arXiv announcement and do not break priority claims. Check each platform's moderation speed, DOI issuance, and arXiv's duplicate-content policy scope (internal duplicates vs external preprints). Note journal/conference preprint-policy caveats.

OUTPUT FORMAT (in Russian):
- Per question: conclusion + confidence (established / emerging / speculative) + primary source links with dates.
- Separate block: step-by-step action plan for the next 14 days, explicitly listing what NOT to do (no delete/resubmit, no second account, no second paper submission, no duplicate help tickets).
- Separate block: findings that contradict common wisdom (e.g., cs.MA is a fast category — median 1 working day; resubmission slows things down; moderator silence is uninformative because moderators give no feedback by policy; no general arXiv-wide slowdown per data).

Your arXiv "on hold" moderation report is ready. It analyzes your situation as a first-time submitter across 8 key questions, drawing on official arXiv policy documents, the fi-le.net delay statistics (63,847 papers), and the 2025–2026 policy timeline (position-paper rules, LLM one-year-ban policy, endorsement tightening). The report covers realistic hold-duration benchmarks for cs.MA (median 1 business day, with your 12+ days being a rare but explainable tail), documented vs. folklore risk factors, the impact of your prior AH-182792 flag, AI-disclosure requirements with a safe acknowledgement template and red-flag checklist, risks of a second account or parallel submission, and alternative preprint venues (Zenodo, OSF, TechRxiv, SSRN). It closes with a day-by-day 14-day action plan, escalation thresholds, and an explicit "do-not" list — including why you should NOT delete-and-resubmit, NOT open a second account, and NOT file duplicate tickets.


# arXiv-модерация «on hold» для впервые подающего автора: 8 вопросов, доказательства и план действий

## TL;DR
- Ваш случай статистически нетипичен, но НЕ катастрофичен: медиана «submission→announcement» для cs.MA — 1 рабочий день (fi-le.net, 09.07.2026), а 12+ дней ручной модерации — это «хвост» распределения, характерный не для cs.MA, а для «общих»/междисциплинарных работ и работ, требующих переклассификации; молчание модераторов НЕ является сигналом отказа — по политике arXiv фидбэк не даётся вообще.
- Наибольшие задокументированные (не «фолклорные») риски именно в вашем профиле: (а) прямая норма arXiv о «closer scrutiny» для авторов с ранее задержанными/отклонёнными работами; (б) риск классификации концептуальной работы по MAS/кибернетике/DAO как «position paper» или как работы «без оригинального исследования»; (в) майская политика 2026 о годичном бане за неотредактированный вывод LLM.
- Что делать 14 дней: НЕ удалять/переподавать, НЕ создавать второй аккаунт, НЕ подавать второй препринт, НЕ дублировать тикеты; вместо этого — один вежливый запрос в moderation-портал, аудит рукописи на «красные флаги» LLM, и параллельная фиксация приоритета на Zenodo (мгновенный DOI) без нарушения будущего анонса на arXiv.

## Key Findings

**Вывод 1 — реальная статистика «on hold» (уверенность: emerging).** Лучший количественный источник — анализ fi-le.net «Counting ArXiv Delays» (09.07.2026, 63 847 работ, выборка 2015–2026). Медианы «submission→announcement» в рабочих днях (исключая выходные/праздники): cs.MA — median 1, mean 1.44, p90 2 (n=27); cs.CR — median 1, mean 1.91, p90 2 (n=193); cs.CE — median 1, mean 2.35, p90 3.4 (n=20). Для сравнения — cs.CY: median 2, mean 6.04, p90 21 (n=1 764); cs.OH: mean 6.85, p90 18.9. Ключевой вывод автора дословно: «Not only is there no recent increase in overall delays, but there never has been an increase for a sustained period of time» — то есть общего роста задержек по arXiv нет, «хвост» сосредоточен в «общих» категориях, куда часто попадают неверно классифицированные или требующие переклассификации работы.

КРИТИКА источника (обязательна): (1) **survivorship bias** — считаются ТОЛЬКО анонсированные работы; задержанные-и-затем-отклонённые невидимы, поэтому реальный «хвост» именно для held-работ длиннее наблюдаемого; (2) это самиздат одного автора, не прошедший рецензирование; (3) метрика — «submission→announcement», а не «длительность именно статуса on hold», хотя на практике они близки. Официально arXiv (submit_status.html) формулирует нейтрально: «Due to the large volume of submissions the process may take several days» и «On Hold submissions will not expire». Задокументированные «хвостовые» случаи из сообщества (трактовать как анекдоты): 5+ недель (комментарий на blog.arxiv.org, тред от 30.06.2026), ~4 недели при подозрении на неверную классификацию (ramblingacademic.com). **Итог: для успешной подачи cs.MA — 1 рабочий день норма; ваши 12+ дней — редкий, но не аномальный «хвост», объяснимый ручной проверкой междисциплинарного/концептуального контента.**

**Вывод 2 — какие признаки увеличивают риск (уверенность: смешанная — часть PROVEN политикой, часть folklore).**

ДОКАЗАНО политикой arXiv (moderation/index.html): «Submitters who have had works previously delayed or declined by arXiv should anticipate closer scrutiny on future submissions»; работы, «that do not contain original or substantive research, including course projects, research proposals, news…» могут отклоняться; «Submissions in need of significant review and revision may also be declined»; переклассификация primary/cross-list — штатная практика («Cross-lists… may be removed by moderators when the classification is deemed inappropriate»).

ДОКАЗАНО (blog.arxiv.org, 31.10.2025, дословно): «Before being considered for submission to arXiv's CS category, review articles and position papers must now be accepted at a journal or a conference and complete successful peer review»; без journal ref + DOI такие работы «will be likely to be rejected». **ВАЖНО для вашего профиля:** та же страница явно выводит из-под этой нормы научные работы о влиянии технологий на общество: «I have a scientific paper studying the impact of science and technology in society… Yes… These are scientific research papers and are not subject to this moderation practice change».

**Оценка вашего риска «position paper»:** концептуальный текст по MAS/кибернетике/DAO РИСКУЕТ быть квалифицирован как position paper или как «lacking original/substantive research», ЕСЛИ в нём нет собственного вклада (формальной модели, эксперимента, доказательства, данных, воспроизводимого метода). Если собственный вклад есть и явно оформлен — норму 31.10.2025 к вам применять не должны.

FOLKLORE (не подтверждено политикой как самостоятельный триггер отклонения/задержки): «solo authorship», «Independent Researcher» в affiliation, отсутствие цитируемости — сами по себе НЕ являются задокументированными причинами отказа. Косвенная связь: исследование Zhao et al. (arXiv:2605.07723) отмечает, что галлюцинации ссылок «especially pronounced… among small and early-career author teams», что может повышать вероятность close scrutiny для маленьких/новых команд, но это про контент, а не про статус автора.

**Вывод 3 — влияет ли прошлое админ-действие на аккаунте (уверенность: established для факта политики «closer scrutiny»; speculative для количественного эффекта вашего флага).** Прямая норма (moderation/index.html): «arXiv moderation decisions may be influenced by the publication status… Submitters who have had works previously delayed or declined by arXiv should anticipate closer scrutiny on future submissions. In some cases, authors may be required to establish a conventional publication record…». Это официально подтверждает, что предыстория может влиять на строгость. ОДНАКО ваша история (тикет AH-182792 — блок на ПРОСМОТР email-адресов авторов из-за массового сбора) — это НЕ «declined submission» и НЕ «delayed work»; буквально норма про ранее задержанные/отклонённые работы к ней не применяется. Публично задокументированных кейсов, где именно такой флаг доказанно замедлил модерацию, я не нашёл — заявляю это явно; на форумах есть только анекдотические «black-list»-теории (ResearchGate), которые следует трактовать как folklore. **Вывод: связь возможна на уровне общего внимания к аккаунту, но не доказана; не стройте план вокруг этой гипотезы.**

**Вывод 4 — что реально ускоряет и что вредит (уверенность: established для официальных норм; emerging для тайминга).**

ОФИЦИАЛЬНО ВРЕДИТ (submit_status.html, дословно): «Please do not make a duplicate submission while your work is on hold. Deleting and resubmitting your work will lead to further delays.» Дубликаты запрещены (moderation: при «multiple similar submissions… we may request that the submissions be consolidated or versioned»). Смена primary-категории или «комментарий о новизне» на этапе on hold ТЕХНИЧЕСКИ невозможны: «During this time you may not edit the submission».

Тайминг help-desk: официально (contact.html) «our user support team typically operates 0900-1700 EST/EDT, Monday through Friday. Most requests are responded to by the end of the day, subject to staff availability and holidays» — но это про ТЕХНИЧЕСКИЕ тикеты, НЕ про модерацию; для модерации arXiv прямо просит «be patient, as moderation appeals are complicated and may take some time». Из сообщества: ответы по модерации бывают быстрыми (~1 день), но по существу неинформативными («kind and yet uninformative email… the moderation team is unable to provide any updates»). Учтите: help-desk не работает в дни ряда праздников США (в 2026 в т.ч. 19.01 MLK, 19.06, 03.07, 07.09, 26.11, 25/29/31.12).

Что РАБОТАЕТ: терпеливое ожидание + ОДИН корректный запрос через moderation-портал (не повторные письма в help@). Смена категории ускоряет только на этапе НОВОЙ подачи (интегрированный классификатор снижает вероятность hold из-за misclassification — blog.arxiv.org, 20.05.2020), но не в текущем статусе.

**Вывод 5 — политика по AI-ассистированным работам 2026 (уверенность: established).** Базовая норма (moderation/index.html): значимое использование text-to-text генеративного AI ДОЛЖНО раскрываться как метод; автор несёт полную ответственность за весь контент «irrespective of how the contents were generated»; генеративный LLM НЕ может быть автором.

Ужесточение от 14.05.2026 (анонсировал Thomas Dietterich, председатель CS-секции arXiv, emeritus professor Oregon State University, в треде X): при «incontrovertible evidence that the authors did not check the results of LLM generation, this means we can't trust anything in the paper» — санкция «a 1-year ban from arXiv followed by the requirement that subsequent arXiv submissions must first be accepted by a reputable peer-reviewed venue». Триггеры (incontrovertible evidence): галлюцинированные ссылки; оставленные мета-комментарии модели (например «here is a 200 word summary; would you like me to make any changes?»); плейсхолдеры («fill in the real numbers from your experiments»); плагиат/явные ошибки. Это «one-strike»-правило, но с обязательным подтверждением section chair и правом на апелляцию ДО вступления бана в силу; рутинное LLM-редактирование под правилo НЕ подпадает.

Обоснование политики — препринт Zhao, Wang, Stuart, De Vaan, Ginsparg (сооснователь arXiv), Yin (arXiv:2605.07723, подан 08.05.2026): аудит 111 млн ссылок в 2,5 млн работ (arXiv, bioRxiv, SSRN, PubMed Central); дословно «a conservative estimate of 146,932 hallucinated citations in 2025 alone», с указанием, что явление «especially pronounced… among small and early-career author teams».

**Безопасная формулировка acknowledgement (пример для вставки):**
> «During the preparation of this manuscript, the author used [ChatGPT, версия, дата доступа; разработчик OpenAI] for language editing and drafting assistance. The author reviewed and verified all content, including every reference, and takes full responsibility for the final text.»

**Чек-лист «красных флагов» перед переподачей (аудит обязателен):** (1) проверить КАЖДУЮ ссылку в реальной базе — DOI/arXiv-id существует, авторы/год/название/журнал совпадают; (2) удалить любые фразы-обращения к/от модели; (3) убрать плейсхолдеры, «TODO», «fill in…»; (4) проверить таблицы/цифры на реальность и согласованность; (5) убрать шаблонные зачины («Certainly, here is…»); (6) назвать инструмент и версию точно, не писать расплывчато «AI was used».

**Вывод 6 — риски второго аккаунта (уверенность: established).** Прямая норма (identity_and_affiliation.html, дословно): «Each user may have only one registered arXiv account… It is a violation of our policies to misrepresent your identity or organizational affiliation. Misrepresentation of identity or affiliation, for any reason, is possible grounds for immediate and permanent suspension.» Второй аккаунт прямо запрещён и создаёт риск перманентного бана. Authority Records (authority.html) связывают личность с работами и лежат в основе endorsement — обход через второй аккаунт разрушает эту привязку и может быть расценён как misrepresentation. **Специфичных для arXiv публично задокументированных «санкционных кейсов» именно за дубль-аккаунт я не нашёл — заявляю это явно, не экстраполируя с других платформ; но текста политики достаточно, чтобы считать риск реальным и неоправданным.**

**Вывод 7 — влияет ли подача второго препринта при первом on hold (уверенность: emerging).** Прямой нормы «нельзя подавать вторую работу, пока первая на hold», НЕТ. Есть норма о «practical limit to the rate at which appropriate, independent submissions can be produced by any one person» и правило «no more than three papers per day» (moderation/index.html). Риски: (1) две работы подряд от молодого аккаунта могут усилить восприятие «closer scrutiny»; (2) если работы тематически близки — риск подпасть под «duplicated content / consolidate». Данных, доказывающих, что вторая подача ухудшает судьбу ПЕРВОЙ работы, я не нашёл; выгоды тоже нет, а даунсайд (усиление внимания к аккаунту) реален. **Рекомендация — не подавать вторую до разрешения первой.**

**Вывод 8 — альтернативные площадки, пока ждёте (уверенность: established).** arXiv-политика о дубликатах касается ВНУТРЕННИХ дублей (несколько подач внутри arXiv), НЕ внешних препринтов; выкладка на другой не-коммерческой площадке не нарушает будущий анонс на arXiv и не ломает приоритет (arXiv с 2022 присваивает DOI и поощряет связывание с внешними DOI). Опции:
- **Zenodo** (управляется CERN): мгновенный DOI, функция «Reserve DOI» до публикации, фактически без научной модерации; лучший вариант для быстрой фиксации приоритета междисциплинарной работы. Оговорка: после публикации файлы можно менять только в течение 45 дней.
- **OSF Preprints**: DOI после акцепта, но пре-модерация — «please allow at least 2 weeks», типично 4–5 рабочих дней на ревью.
- **TechRxiv** (IEEE): модерационный скрининг (не peer review), DOI, индексация в Crossref/Google Scholar, НО удалить опубликованный препринт нельзя.
- **SSRN** (Elsevier): по отзёывам задержки до ~3 недель и заметная доля desk-reject; менее удобен для быстрой фиксации.

**Приоритетная фиксация — Zenodo сейчас.** Оговорка: проверьте preprint-политику целевого журнала/конференции. Большинство (ACM, IEEE, APS, AAAS) явно разрешают не-коммерческие серверы (arXiv, Zenodo, OSF), но условия по версиям и лицензиям различаются; ряд издателей ограничивает пост-рецензионные версии.

## Details

### Хронология политик arXiv 2025–2026 (первоисточники)
- **31.10.2025** — CS: review/survey и position papers только после peer-review-акцепта (blog.arxiv.org). Причина — вал LLM-сгенерированных обзоров.
- **10.12.2025** (Math) и **21.01.2026** (весь arXiv) — ужесточение endorsement: институциональный email больше не является достаточным основанием авто-endorsement для новых авторов.
- **01.02.2026** — требование полной англоязычной версии всех новых работ.
- **08.05.2026** — препринт Zhao et al. (с Ginsparg), 146 932 галлюцинированных ссылки за 2025.
- **14.05.2026** — политика годичного бана за неотредактированный вывод LLM (Dietterich, тред X).
- **01.07.2026** — arXiv стал независимым НКО; >3 млн статей (объявлено 09.07.2026).

### Почему молчание модераторов неинформативно
Официально (moderation/index.html): «the arXiv moderation process is not a peer-review process. arXiv staff and moderators cannot give feedback on the submission»; апелляционные решения «are final, and… no feedback will be provided with the decision». Отсутствие писем 12+ дней НЕ означает ни отказа, ни одобрения — это штатный «тихий» режим. Более того, arXiv прямо пишет: если требуется действие автора — «arXiv editors will either send you an email… or resolve the hold without your needing to do anything… If you notice your submission is on hold, no action is required on your part at this time».

### Как ваш профиль соотносится с рисками
Endorsement на cs.MA получен 20.07.2026 — валиден. Подача 23.07.2026 попала в очередь — значит, submission-канал не заблокирован (блок AH-182792 касается только просмотра email-адресов, что вы и подтверждаете возможностью создать новую подачу 04.08.2026). Prime-риск — не аккаунт, а КОНТЕНТ: концептуальная работа по MAS/кибернетике/DAO без явных собственных результатов рискует трактоваться как position paper. Комбинация «tokenomics/DAO» + finance объясняет cross-list в cs.CE, но модератор может счесть тему частично вне охвата cs или потребовать переклассификации — это и даёт «хвостовую» задержку, наблюдаемую в «общих»/междисциплинарных категориях.

## Recommendations

**Дни 1–3:**
1. НИЧЕГО не удаляйте и не переподавайте. Оставьте работу в статусе on hold (он не истекает).
2. Отправьте ОДИН вежливый запрос через moderation-портал arXiv (arxiv.org/support/moderation_help — не повторно в help@). Укажите: submission number, дату подачи (23.07.2026), полученный endorsement на cs.MA (20.07.2026), и один вопрос — требуются ли действия от вас. Не поднимайте тему AH-182792.
3. Параллельно зафиксируйте приоритет на Zenodo (мгновенный DOI). Это не конфликтует с будущим анонсом arXiv.

**Дни 3–10:**
4. Проведите полный аудит рукописи по чек-листу «красных флагов» LLM (Вывод 5) — особенно верификация ВСЕХ ссылок и удаление любых мета-следов модели. Это ваша главная управляемая переменная риска.
5. Убедитесь, что работа читается как research paper с собственным вкладом (модель/метод/результат/данные), а не как обзор или position paper. При необходимости усильте и явно обозначьте разделы с оригинальным вкладом.
6. Добавьте корректный AI-disclosure (пример в Выводе 5) — превентивно снижает риск флага.

**Дни 10–14:**
7. Если статус не изменился и прошло 14+ дней с момента вашего запроса без ответа — допустим ОДИН follow-up в ТОТ ЖЕ moderation-тикет (не новый тикет, не новое письмо в help@).
8. Держите Zenodo-версию как страховку приоритета; при желании добавьте OSF Preprints как вторую точку.

**Пороги, меняющие план:**
- Пришло письмо модератора с конкретным требованием → выполняйте буквально в рамках той же подачи, не заводите отдельную работу.
- Работу переклассифицировали (сняли cs.CE или сменили primary) → это норма, не оспаривайте без веской причины.
- Пришёл decline как «position paper»/«insufficient original research» → идите через официальный appeals-процесс, а не через переподачу; при необходимости сначала получите peer-review-акцепт.
- Held-статус превысил ~30 дней → эскалация к Section Chair через moderation-портал (штатный механизм, указанный в moderation/index.html).

**Чего НЕ делать (явный список):**
- НЕ удалять и не переподавать работу — официально ведёт к «further delays».
- НЕ создавать второй аккаунт — нарушение правила «one account», риск перманентного бана.
- НЕ подавать вторую работу, пока первая на hold.
- НЕ дублировать тикеты (help@ + moderation одновременно) и не слать повторные письма ежедневно.
- НЕ выкладывать идентичный текст несколькими подачами внутри arXiv.

## Caveats
- fi-le.net — самиздат-анализ одного автора (не рецензирован) и с survivorship bias: измеряет «submission→announcement» только для АНОНСИРОВАННЫХ работ, поэтому занижает реальный «хвост» для held-then-declined. Медиана cs.MA = 1 день надёжна как ориентир для успешных подач, но не описывает вашу текущую ручную модерацию.
- Тайминги help-desk из сообщества фрагментарны; официальный SLA («by end of day») относится к техподдержке, НЕ к модерации, где arXiv прямо просит терпения.
- Норма «closer scrutiny» доказана как политика, но количественный эффект именно вашего флага AH-182792 не задокументирован — это оценка, а не факт.
- Политика годичного бана за LLM применяется узко (incontrovertible evidence), с подтверждением section chair и апелляцией; для добросовестно отредактированного и проверенного текста риск низкий — но верификация ссылок обязательна.
- Часть цитат сообщества (5-недельные холды, «blacklist») — анекдотические; трактуются как folklore, а не доказательство.
- Противоречия здравому смыслу (подтверждённые данными): cs.MA — быстрая категория (медиана 1 рабочий день), а не медленная; переподача НЕ ускоряет, а замедляет; молчание модераторов неинформативно (фидбэк не даётся по политике); общего arXiv-wide замедления по данным НЕТ — «хвост» локализован в «общих»/междисциплинарных категориях и в контенте с признаками LLM-генерации или без оригинального исследования.

