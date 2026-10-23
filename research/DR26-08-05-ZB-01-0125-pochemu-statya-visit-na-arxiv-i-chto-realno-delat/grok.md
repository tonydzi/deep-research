# DR26-08-05-ZB-01-0125 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Исследую актуальные (2025–2026) источники по модерации arXiv: официальные страницы, посты модераторов и треды авторов.# Deep Research: arXiv on hold 2026 — факты, не фольклор

Контекст вашей ситуации (первая подача, Independent, cs.MA + cs.CE + cs.CR, 12 дней hold без писем, старый email-block, LLM-assisted, 2-я статья 04.08, help@ 04.08) вписывается в **узкий, но документированный** режим «ручная модерация + tighter CS 2025–26», а не в «сломанный цикл анонса».

---

## 1. Реальная статистика `on hold` 2025–2026

### Вывод
Официально arXiv обещает **«several days»** / **«1–4 days, sometimes longer»** для quality assurance. Для **успешно анонсированных** статей в cs.MA медиана ≈ **1 рабочий день**, p90 ≈ **2**. Хвост **недель–месяцев** существует, особенно у пограничных категорий, первых подач и interdisc — но **медианы «для первой подачи нового автора» arXiv публично не публикует**. 12 календарных дней (~8–9 рабочих) — **уже длиннее типичного cs.MA**, но **не аномалия** по шкале community reports 2024–26.

**Уверенность: emerging** (официальные SLA + один независимый scrape 2026; нет open dataset «hold duration × new author»).

### Источники

| Источник | Дата | Что говорит |
|---|---|---|
| [submit_status.html — On Hold](https://info.arxiv.org/help/submit_status.html) | live 2026 | «several days»; hold не истекает; **не** делать duplicate / delete+resubmit |
| [availability.html](https://info.arxiv.org/help/availability.html) | live 2026 | QA: **1–4 days, sometimes longer** |
| [blog «Our Moderation Process»](https://blog.arxiv.org/2019/08/29/our-moderation-process/) | 2019 (процесс тот же) | ~15% submissions flag; цель 24h, иначе hold |
| [fi-le.net «Counting ArXiv Delays»](https://fi-le.net/arxiv/) | **9 Jul 2026** | 63 847 papers, 2015–2026; delay = submission→announcement в **рабочих** днях |
| Academia SE, hold 11 days → posted day 17 | Jul 2024 | weeks = known |
| Academia SE + comments | 2023–24 | cs: 2–3+ weeks «not unusual» for interdisc |
| Blog comments / SE Jul 2026 | post Cornell spin-out 1 Jul 2026 | anecdotal «>5 weeks», «>1 month» on cs.* without conference stamp |

**Цифры fi-le.net (primary category only, announced papers):**

| Category | n | median (workdays) | mean | p90 |
|---|---:|---:|---:|---:|
| **cs.MA** | 27 | **1.0** | 1.44 | **2.0** |
| cs.CR | 193 | 1.0 | 1.91 | 2.0 |
| cs.CE | 20 | 1.0 | 2.35 | 3.4 |
| cs.AI | 7290 | 1.0 | 2.02 | 2.0 |
| **cs.CY** | 1764 | 2.0 | **6.04** | **21.0** |
| cs.OH | 68 | 2.0 | 6.85 | 18.9 |

**Критичный bias:** таблица меряет только то, что **в итоге вышло**. Отклонённые, вечные hold, «require journal first» **не входят**. Поэтому «median 1 day for cs.MA» **не опровергает** ваш 12-дневный hold: вы, скорее всего, в **хвосте flagged queue**, который в mean/median announced почти не виден.

**Общий фон 2025–26 (не «медленнее всё»):** fi-le.net **не** находит устойчивого роста delay по времени; зато **политика CS ужесточилась** (surveys/positions Oct 2025; endorsement Jan 2026; AI-slop penalties May 2026) → **больше ручного triage** у «подозрительных» профилей, даже если средний cs.AI всё ещё 1 день.

---

## 2. Признаки долгой модерации / reject в cs (policy vs folklore)

### Вывод
**Доказано политикой:** content type (survey/position без peer review), scholarly standards (refs, form), originality/novelty, reclassification, prior delay/decline → closer scrutiny, excessive rate, AI **unchecked** (hallucinated refs / leftover prompts).  
**Фольклор / emerging (не написано явно, но согласуется с практикой модераторов):** Independent Researcher + first paper + DAO/tokenomics + multi-crosslist + LLM-style prose → **выше шанс flag**, не «авто-reject».

**Уверенность: established** (policy) / **emerging** (risk factors as practical correlates).

### Таблица policy vs folklore

| Фактор | Статус | Доказательство |
|---|---|---|
| **Survey / position paper в cs без peer review** | **POLICY → likely reject** | Blog 31 Oct 2025; content types |
| **Original research** (вкл. science & society, cs.CY) | **allowed** | Тот же blog: «scientific papers studying impact…» OK |
| **Scholarly standards** (refs, structure, tone) | **POLICY** | [moderation/index.html](https://info.arxiv.org/help/moderation/index.html) |
| **Lack of originality / not substantive research** | **POLICY decline** | same |
| **Previously delayed or declined** → closer scrutiny | **POLICY** | same: *«Submitters who have had works previously delayed or declined… closer scrutiny»* |
| **Excessive submission rate** | **POLICY** (limit request; ≤3/day for backlog) | same |
| **Duplicate / similar multiple submissions** | **POLICY** (consolidate/version) | same + versions.html |
| **Cross-list wrong category** | **POLICY** reclass/remove | same |
| **cs.MA + finance/DAO / cs.CE** | **emerging risk** (boundary / reclass) | not banned; cs.CE/q-fin adjacent; boundary papers → slower human read |
| **cs.CY cross-list** | **emerging** | fi-le: longest CS tail; if reclass toward CY → weeks |
| **Solo + Independent Researcher** | **folklore→emerging** | not forbidden; endorsement path for non-institutional is **personal endorse** (you already have cs.MA); Dietterich comments (May 2026): slop often from solo independents — **stereotype affects triage**, not formal ban |
| **No citation record** | **folklore** | not a stated reject criterion; endorsement is the gate |
| **LLM-assisted (disclosed, checked)** | **allowed by policy** | generative AI **must be reported** if significant; author fully responsible |
| **LLM-slop signals** (fake refs, leftover «here is a 200 word summary…») | **POLICY hard** (1y ban + journal-first after) | Dietterich May 2026; confirmed by arXiv leadership to Ars |

**Для вашей тематики:** multi-agent + control + tokenomics/DAO — **не запрещена**, но стоит на стыке cs.MA / cs.CE / (иногда) cs.CY / q-fin. Стык → **человеческий read + возможная reclassification**, а не «DAO = reject».

---

## 3. Старое админ-взыскание (email view block) → скорость модерации?

### Вывод
**Прямой связи «email-scrape ban → submission hold» в документации нет.**  
Есть **общая** формулировка: *previously delayed or declined → closer scrutiny* — но ваш случай (март 2026, AH-182792, block **просмотра** email авторов, support «снять не умеем») — это **account capability restriction**, не declined submission. Подача 23.07 **принята в pipeline** → submission path жив.

Гипотеза «старый ticket = red flag на аккаунте» — **plausible operationally** (support systems often share account notes), но **не доказана** публичными кейсами «AH-ticket → multi-week hold».

**Уверенность: speculative** (для causal link) / **established** (что email-block ≠ submission ban).

**Практика:** не «чинить» email-block письмами во время hold — это **другой** трек и может засорить очередь. Не смешивать AH-182792 с запросом по submit id.

---

## 4. Что реально ускоряет / вредит

### Вывод

| Действие | Эффект | Статус |
|---|---|---|
| **Ждать + не трогать** | Основной путь; hold не expire | **established** (official) |
| **moderation support portal / [email hidden]** | Правильный канал статуса hold / appeal | **established** |
| **[email hidden]** | User support (accounts, technical); *не* primary для moderation status | **established** — у вас, вероятно, **не тот inbox** |
| **Напоминание после «several days»** | Community: sometimes nudges queue (SE 2024: weeks → resolve after contact) | **emerging** |
| **Delete + resubmit** | **Вредит** — «further delays» | **established** (explicit) |
| **Duplicate same work** | **Вредит** | **established** |
| **Смена primary category «на лету»** | Пока on hold обычно **нельзя** редактировать; unsubmit/resubmit = delay; reclass делает **модератор** | **established** mechanics |
| **Comments field «novelty»** | Может помочь *до* submit; *после* hold — только если попросят replace | **speculative** as accelerator |
| **Указать journal/DOI если уже peer-reviewed** | Сильно помогает (SE cases; conference-accepted clear faster post-2026 anecdotes) | **emerging** |
| **Агрессивный daily ping** | Риск noise; staff: «be patient» | **emerging** harm |

**Официальный контакт (live 2026):**
- Status / appeals: [moderation support portal](https://arxiv.org/support/moderation_help) или **[email hidden]**
- Technical/account: portal / **[email hidden]** (M–F 09–17 ET; «most requests end of day» — **это user support, не CS moderator queue**)
- **Не писать модераторам лично**

**Уверенность: established** (каналы + anti-resubmit) / **emerging** (nudge efficacy).

---

## 5. AI-assisted 2026: что раскрывать, что флаг

### Вывод
arXiv **не запрещает** LLM-tools. Требует:
1. **Report significant use** of generative AI (как sophisticated tool / methodology), «consistent with subject standards».
2. **Authors fully responsible** for all content (errors, plagiarized text, wrong refs).
3. **AI ≠ author** — в author list не ставить.
4. **Hard penalty** only for **incontrovertible unchecked slop**: hallucinated references, leftover LLM meta-comments/prompts. Penalty (CS section chair, May 2026, confirmed by leadership to Ars): **1-year ban**, then future submissions need **prior acceptance at reputable peer-reviewed venue**.

Dietterich (comment May 2026): ban only with **irrefutable** evidence (leftover LLM comments **or pattern of hallucinated refs**); if strong suspicion without that → often «get it peer-reviewed first», not ban.

**Безопасные acknowledgements (типовой каркас, не «магическая формула»):**
> Parts of the text were drafted or edited with the assistance of large language model tools under continuous human editorial control. All scientific claims, references, and conclusions were verified by the authors, who take full responsibility for the content.

**Что вызывает флаг (не «use of AI», а quality signals):**
- Несуществующие / неверифицируемые citations  
- «As an AI language model…», «Would you like me to expand…» в PDF  
- Annotated-bibliography-as-survey (особенно под Oct 2025 CS practice)  
- Однообразный LLM-prose + zero novel method/result  

**Уверенность: established** (official moderation page + Dietterich + Ars confirmation May 2026).

---

## 6. Второй аккаунт

### Вывод
**Один пользователь = один аккаунт.**  
*«Each user may have only one registered arXiv account.»*  
Misrepresentation of identity/affiliation → **immediate and permanent suspension** possible.  
Fraud/abuse monitoring is explicit in privacy policy.

**Документированные санкции:** policy-level permanent suspension for identity abuse; **нет** публичного «case law» вида «caught dual account → X days ban», но правило **жёсткое и однозначное**.

**Уверенность: established** (policy) — **не делать**.

[Identity and affiliation](https://info.arxiv.org/help/policies/identity_and_affiliation.html)

---

## 7. Вторая статья, пока первая on hold

### Вывод
**Прямого запрета** «нельзя submit #2 while #1 on hold» **нет**.  
Есть:
- **Do not duplicate** the *same* work while on hold  
- **Excessive rate** may trigger rate-limit request  
- **Similarity** across concurrent submissions → consolidate/version  
- **Previously delayed** submissions → **closer scrutiny on future ones** (policy) — если #1 ещё в hold, #2 **может** наследовать account-level scrutiny

**Данных** «back-to-back from young account worsens #1» в open literature **нет**.  
Логика модерации (и SE anecdotes: multiple simultaneous uploads sometimes hurt) → **риск скорее на #2 и на account reputation**, не «гарантированно топит #1».

**Уверенность: speculative** (effect on #1) / **emerging** (account scrutiny compounds) / **established** (no duplicate of same paper).

**Практический вердикт для вас (2-я создана 04.08):**  
- Если **другая** работа (не rewrite #1) — допустимо, но **не спамит** 3-й.  
- Если близка по теме/тексту — риск «similarity / rate» flag.  
- **Не** unsubmit #1 ради «чистого листа».

---

## 8. Альтернативы / timestamp без конфликта с arXiv

### Вывод
arXiv **не** запрещает prior preprint на других серверах как таковой; *«prior publication does not guarantee acceptance»* (и не auto-reject). **Приоритет/timestamp** даёт **DOI + open deposit**; arXiv остаётся **дистрибуцией в CS community**, не единственным clock.

| Площадка | Скорость | DOI | CS visibility | Конфликт с arXiv | Заметки |
|---|---|---|---|---|---|
| **Zenodo** | минуты | да (CERN) | низкая | **нет** (common dual-post) | Immutable; license carefully |
| **OSF Preprints** | часы–дни | да | средняя | **нет** | COS; versioning |
| **SSRN** | days (screening) | да | mid (social/econ/fin better than pure MA) | **нет** | Elsevier; good for tokenomics/DAO adjacent |
| **TechRxiv** (IEEE) | moderated, often faster than arXiv hold | да | mid (eng/CS) | **нет** | IEEE moderated preprint; check their AI policy |
| **viXra** | fast | no prestige | toxic signal | social stigma | **не рекомендовать** для credibility |

**Priority practice:**
1. Post to **Zenodo or OSF** with **CC-BY** (or arXiv-compatible license) **now** → citable DOI + public timestamp.  
2. Keep arXiv submission **as-is** (don’t withdraw).  
3. When arXiv announces, add journal-ref / comment linking DOI if useful.  
4. **Не** менять title/abstract радикально между серверами без version note.

**Уверенность: established** (multi-preprint is normal in CS/open science) / **emerging** (whether dual-post affects arXiv moderator impression — rarely discussed, no policy against).

---

## План на 14 дней (05–19 Aug 2026)

### Делать

| День | Действие |
|---|---|
| **D0 (сегодня)** | Проверить: (1) email spam + folder for **moderation@** / arXiv; (2) был ли **auto-reply** на help@ 04.08 — если нет, письмо, возможно, в spam filter arXiv ([contact.html](https://info.arxiv.org/help/contact.html)) |
| **D0–1** | Открыть ticket в **[moderation support portal](https://arxiv.org/support/moderation_help)** (не help@). Текст: submit id · date 23.07 · status on hold ~12d · **no moderator email** · ask for status only. **Без** AH-182792, **без** «my deadline», **без** давления |
| **D0–2** | Timestamp: **Zenodo or OSF** той же PDF (+ license), если приоритет критичен. Одна строка в abstract/comments: «Also submitted to arXiv (pending moderation)» — optional |
| **D0** | PDF audit **до** любых replace: все citations open; zero leftover AI strings; AI acknowledgement present if significant use |
| **D3–7** | Молчание OK. Раз в 5–7 дней max one polite follow-up **в том же ticket** |
| **D7–14** | Если reject/decline → **appeals process** only ([appeals.html](https://info.arxiv.org/help/moderation/appeals.html)); если «needs peer review first» → target workshop/journal, not dual-account |
| **Параллельно** | #2 paper: only if **clearly distinct**; ensure not near-duplicate of #1; no third paper this fortnight |

### НЕ делать

1. **Delete / unsubmit / resubmit** #1  
2. **Второй arXiv-аккаунт**  
3. Ежедневный spam help@ + moderation@ + personal moderator emails  
4. Смешивать email-view ban ticket с moderation hold  
5. «Ускорить» сменой primary category без запроса staff  
6. Массово править текст «под AI detector» (cosmetic churn without request can look worse)  
7. Постить на viXra «на всякий случай»  
8. Публично ныть в соцсетях с submit id (редко помогает, иногда вредит tone)

---

## Что противоречит расхожему мнению

| Расхожее мнение | Реальность 2026 |
|---|---|
| «arXiv всегда выходит за 1–2 дня» | Только unflagged / easy categories; hold = human queue; official «several days» |
| «12 дней = баг / забыли» | Community: 2–3+ weeks known; months documented in edge cases; hold does not expire |
| «Пиши help@ — починят» | Hold → **moderation@ / portal**; help@ = user support, other queue |
| «Переподай — быстрее» | Official: **resubmit delays further** |
| «Independent = reject» | Endorsement is the gate (you passed cs.MA); Independent allowed; scrutiny ≠ ban |
| «LLM = ban» | **Unchecked slop** = ban; **disclosed assisted + verified** = allowed |
| «Второй аккаунт обойдёт hold» | Policy violation → permanent suspension risk |
| «cs.MA в принципе медленный» | Data: cs.MA median 1 day; **медленные** — cs.CY, cs.OH, gen-ph; ваш hold = **flag**, not category median |
| «После Cornell spin-out (1 Jul 2026) всё встало» | Anecdotes of backlog exist; **no** official «delays doubled»; conference-stamped papers still clear faster |
| «Без писем = всё ок, просто ждут» | Official: email **if action needed**; silent hold often = **moderator still deciding**, not «approved pending announce» |
| «Email scrape block = submission blacklist» | Not documented; submission still works; causal link unproven |

---

## Вердикт по вашей конфигурации (synthesis)

**Наиболее вероятная картина (confidence: medium):**  
Первая подача + Independent + стыковая тематика (MA/CE/CR, tokenomics/DAO) + post-2025 CS AI/flood climate → **manual hold без письма**. 12 дней — **длинный, но в пределах known tail**, не доказательство reject.  
**Не** «сломался анонс-цикл» (вы уже прошли 6 окон).  
**Не** чинится вторым аккаунтом.  
**Ускоритель №1 сейчас:** правильный **moderation portal ticket** + **не resubmit** + (опционально) **Zenodo/OSF DOI** для timestamp.  
**Ускоритель №2 (если попросят replace):** ironclad refs + AI acknowledgement + чёткий original contribution (не survey-position vibe).

---

🎙 **Майк:** вы не в «чёрном списке за email», вы в **хвосте ручной CS-модерации 2026** — это дорогое место, но проходимое. Главная ошибка, которую делают на 12-й день: **delete+resubmit** и **второй аккаунт**. Правильный ход — **moderation portal + DOI на Zenodo + тишина**.

---

### 🧒 Простыми словами

Представь почту редакции журнала. Твою первую статью положили в **отдельную стопку «надо прочитать глазами»**, а не в автомат «напечатать завтра».  
Обычные статьи из стопки «свои» выходят за **1 день**. Из «глазами» — **несколько дней, иногда недели**. 12 дней — **долго, но бывает**.  

**Что делать:** написать **тем, кто смотрит статьи** (moderation), а не «техподдержке аккаунта» (help). **Не** выкидывать статью и не заводить второй ящик — от этого стопка только растёт.  
**Пока ждут** — можно положить копию на **Zenodo** (как штамп времени с номером), arXiv от этого не злится.

Если нужно — следующим сообщением соберу **готовый текст тикета** в moderation portal (≤120 слов, без воды).
