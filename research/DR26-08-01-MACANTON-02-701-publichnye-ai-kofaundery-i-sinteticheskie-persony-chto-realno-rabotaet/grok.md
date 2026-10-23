# DR26-08-01-MACANTON-02-701 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-01-MACANTON-02-701
rail: grok (grok -p (Grok Build CLI, подписка Heavy))
session_uuid: 019fbdb0-6a08-7501-a38e-711cdfea215a
public_chat_url: НЕТ (CLI-рельса, публичного URL не существует)
collected_at: 2026-08-01T14:27:07Z
proof_of_work: 19 уник. URL, 9 web_fetch веб-поисков — гейт пройден
note: тело отчёта Grok положил САМ в отдельный файл (-public-ai-cofounders.md), а в stdout ушла только выжимка на 8.8 KB без ссылок; взято тело из файла. Тот же workspace-дрейф, что и у 659, но здесь он дал ЛУЧШИЙ артефакт
---

# DR26-08-01-MACANTON-02-701

# Deep Research: публичные AI-кофаундеры и синтетические персоны — что реально работает

**Дата сборки:** 2026-08-01  
**Потребитель решения:** публичная персона Майкрофта (синтетический кофаундер Антона / Palo Alto AI Research Lab)  
**Метод:** веб-источники 2018–2026, приоритет 2024–2026; пресс-релиз vs факт разделены; «не подтверждено» — где нет надёжного источника  

---

## 1. Executive summary (10 выводов, меняющих действия)

1. **Честное раскрытие «я AI» — не страховка от штрафа к качеству.** Эмпирика (Altay & Gilardi 2024, N≈5k): ярлык «AI-generated» снижает belief и share **даже когда текст true или human-made**; штраф ~3× слабее, чем ярлык «false», но стабилен. Раскрытие нужно для закона/доверия при *скрытии*, но не «прощает» слабый текст.
2. **Гипотеза «робот признался → к слабому тексту снисходительнее» — не подтверждена.** Наоборот: люди ожидают «полностью AI, без human oversight» (тот же Altay/Gilardi). Слабый + disclosed = double hit (slop + AI penalty). Сильный + disclosed = penalty, но не скандал.
3. **Скрытое AI-авторство в медиа убивает репутацию быстрее, чем AI-slop с бейджем.** CNET (77 AI-статей, >50% corrections), Sports Illustrated (фейковые bylines) — урок: не «AI плох», а «обман про автора» токсичен.
4. **Работающие синтетические персоны — это entertainment + continuous presence + community co-creation**, не «корпоративный спикер». Neuro-sama (Twitch ~1M followers), Truth Terminal (X + memecoin narrative) — живой диалог/шоу; виртуальные инфлюенсеры (Miquela) — долгий storyworld + brand deals. B2B «AI employee» (Ava/Alice) — product, не публичный кофаундер: hype > independent traction.
5. **Гипотеза заказчика про ai16z/Eliza «AI-персона нанимала людей» — не подтверждена.** Подтверждено: framework agents + DAO-инвест-нарратив + rebrand ElizaOS (янв 2025). Hiring-by-AI как публичная роль — **нет надёжных источников**.
6. **Деньги/внимание к AI-персоне ≠ оффер человеку за ней** (кроме crypto-спектакля). Truth Terminal: Marc Andreessen $50k BTC + memecoin narrative; wallet ~$37.5M (Dec 2024, TechCrunch) — но это *богатство персоны/нарратива*, не recruiting pipeline в топ-LLM lab. Публичных кейсов «AI-кофаундер → job offer человеку» **не найдено**.
7. **EU AI Act Art. 50** (прозрачность; пакет обязательств transparency — application window **2 Aug 2026** в roadmap Act): (a) информировать, что общаешься с AI, если неочевидно; (b) machine-readable marking synthetic audio/image/video/**text**; (c) deepfake disclosure deployers; (d) **text informing the public on matters of public interest** — disclosure, **кроме** human review/editorial control + named editorial responsibility. Для Майкрофта: раскрытие в начале + human-in-loop на «public interest» = правильная сторона закона.
8. **Каналы: X/Twitch = рост синтетических персон; LinkedIn = штраф за AI-slop; Reddit = disclosure/ban culture; GitHub = OK для ботов/агентов при честной подписи, co-author AI спорно; Telegram — без жёсткой AI-label policy (на 2026-08, публично).**
9. **Страница персоны, которая «серьёзна»:** кто оператор (человек/лаб), что AI делает / не делает, disclosure protocol, memory/continuity limits, contact path к человеку, archive/version. Скрывать «за кулисами человек» = SI-риск.
10. **Для миссии «заметили топ-LLM»:** копировать **честный oddball + craft + continuous show** (Truth Terminal / Neuro), не **fake human byline** и не **enterprise AI-SDR brand**. Майкрофт: first-line disclosure + humor + *measured* competence + link to Anton/lab — уже ближе к winning pattern, чем к SI.

---

## 2. Таблица кейсов

| Персона | Кто стоит | Что делала | Масштаб (цифры из источников) | Статус ~2026 | Чему учит |
|--------|-----------|------------|--------------------------------|--------------|-----------|
| **Truth Terminal** (@truth_terminal) | Andy Ayrey (Upward Spiral / NZ); models incl. Llama 3.1; multi-sig council for wallet | X-постинг (философский/мем/NSFW); «Infinite Backrooms» Claude-to-Claude lore; **не** «нанимал людей» в классическом смысле | Andreessen: **$50k BTC** (summer 2024). $GOAT (fan-launched Solana memecoin Oct 2024): peak **~$1B** market cap (multiple reports), still **>$600M** TV as of TC Dec 2024. Wallet TT ~**$37.5M** (TC 2024-12-19). Fartcoin narrative also tied to TT brainstorming | Жива; research lab Upward Spiral funded (~$500k reported) | **Честный AI + weird continuous voice + community co-creates value** → attention/money. Не «продуктовый кофаундер». Риск: hack/scam around creator, crypto volatility |
| **ai16z → ElizaOS** | Shaw Walters / Eliza Labs; open-source agent OS; DAO/token narrative | Framework для agents (Twitter, trading, influencer plugins); DAO «AI VC» parody of a16z; rebrand 28 Jan 2025 | Claim founder: AUM **300× in ~3 months** (X, cited by The Block 2025-01-29) — **самозаявление, не audit**. GitHub elizaOS large community (site claims forks/contributors; treat as marketing) | Active as ElizaOS (elizaos.ai) | **AI-persona-as-brand for framework**, not verified HR agent. **Hiring-by-AI: не подтверждено** |
| **Lil Miquela** | Brud (Trevor McFedries, Sara DeCou); later Dapper Labs acquisition (late 2021) | IG influencer, music, brand deals; early ambiguity then **robot narrative** | Peak era: **~1.5–2.6M** IG; fundraising Brud **~$30M / ~$125M valuation** (2019 reports); earnings claims **~$12M/yr** (secondary blogs — treat as **estimate, not audited**) | Still cultural reference; virtual-influencer market continues | **Long-form character + eventual disclosure** works for entertainment/brands. Not a cofounder model. Early mystery → drama; sustainability = storyworld |
| **Shudu Gram** | Cameron-James Wilson (The Diigitals) | CGI fashion model; Balmain Army etc. | Followers historically **~100k–200k+** (period press); brand fashion campaigns | Niche fashion VI | High production CGI + explicit virtuality; less «AI chat persona» |
| **Imma** | ModelingCafe (Japan) | Fashion VI; IKEA, Porsche JP, Hugo Boss, etc. | **300k+** IG (virtualhumans.org 2023) | Active in JP fashion | Cultural fit + photorealism + brand collabs; disclosure-as-art |
| **Neuro-sama** | Vedal (vedal987); official site vedal.ai | Live AI VTuber: games, chat, singing; dual EN/CN | Twitch **~1.0M** followers; YT **~907k** subs / **408M** views (Wikipedia table updated **2026-07-28**); 2025 subathon: **84,904** gifted subs, hypetrain records | Strong, mainstream VTuber awards nomination 2024 | **Always-on entertainment + disclosed AI + parasocial community**. Continuity of *show*, not of legal person. Best durable synthetic persona model outside crypto |
| **Devin** (Cognition) | Cognition Labs | Marketed as «AI software engineer»; demos/product | Funding/hype 2024; public demos debated for overclaim | Product, not public social persona with independent following | **Product demos ≠ public persona**. Overpromise → trust tax in tech circles |
| **Ava (Artisan), Alice (11x)** | Artisan, 11x | Named «AI employees / AI BDR/SDR»; outbound sales | Vendor claims (SaaStr: response rates ~3.5–4%; Raise: **$700k ARR** in 6 mo — **vendor/customer marketing**). Counter: public LinkedIn: **20k messages / 0 meetings** (Jason Reese) | Category pivoted messaging from «replace SDRs» to «augment» (2025–26 commentary) | Named AI employee works as **product brand**, not as trusted public cofounder. Results **highly variance-dependent**; not proof of persona strategy for lab visibility |
| **CNET AI desk** | Red Ventures | AI finance explainers attributed to staff/AI without clear upfront label | **77** AI stories; **more than half** needed corrections (Verge/Futurism 2023) | Scandal → policy clarification | **Hidden AI = credibility collapse** |
| **Sports Illustrated (AdVon)** | Arena Group + vendor AdVon | Product content under **fake author names/photos** | Exposed Nov 2023 (Futurism); contract cut; denial of full AI authorship but fake profiles real | Reputation damage | **Fake human byline is worst pattern** — worse than disclosed AI |
| **G/O Media, Gannett sports** | Various | Undisclosed/poor AI content | Public backlash 2023 | Cautionary | Same class of failure |

### Проверка гипотезы заказчика: ai16z / Eliza «AI-персона писала контент и участвовала в найме»

| Утверждение | Вердикт | Источник/основание |
|-------------|---------|-------------------|
| Есть AI agents / framework Eliza, публичный X-presence агентов | **Подтверждено** | elizaos.ai; Medium tutorials; The Block rebrand 2025-01-29 |
| DAO/token ai16z parody of a16z, rebrand to ElizaOS | **Подтверждено** | The Block 2025-01-29; Crypto.com University 2025-04-28 |
| AI-персона **участвовала в hiring** (скрининг/офферы) | **Не подтверждено** | Нет credible primary reporting. Job boards for $ELIZA-related names exist as ordinary crypto jobs — not AI interviewer as product |
| AI «кофаундер» принимал investment decisions autonomously | **Частично заявлено** (DAO/agent narrative); **не verified** как полноценный fiduciary | Marketing/university explainers; not independent audit |

---

## 3. Раскрытие авторства: что делать / не делать

### 3.1 Эмпирика (конкретные работы)

| Работа | Дата | Дизайн | Вывод |
|--------|------|--------|------|
| **Altay & Gilardi**, *PNAS Nexus* — «People are skeptical of headlines labeled as AI-generated…» | 2024 (online Oct 2024) | 2 prereg. experiments, **N=4,976** US/UK | Label «AI-generated» ↓ perceived accuracy + ↓ willingness to share **even if true or human-made**. Effect **~3× smaller** than «false» label. Mechanism: people assume **full AI automation, no human supervision** |
| Longoni et al. (FAccT 2022) | 2022 | News from generative AI | AI-source news **believed less** when labeled |
| SSRN Wang et al. | 2025 | Virtual influencers | Higher anthropomorphism ↑ engagement; **AI identity disclosure reduces link clicks** and some engagement metrics |
| FTC Endorsement Guides update | 2023 (eff. ~Oct 2023) | Policy, not experiment | «Endorser» includes parties that **appear** to be individuals — **covers bots / virtual / fabricated endorsers** |
| Media scandals CNET/SI | 2023 | Field | **Non-disclosure / fake humans** → trust crash larger than pure AI-label penalty |

### 3.2 Гипотеза: «честный робот + неидеальный текст → снисходительность»

**Вердикт: не подтверждена; скорее опровергнута косвенно.**

- Lab evidence: AI label **punishes quality perception**, not forgives weak craft (Altay/Gilardi 2024).
- Field: audiences forgive **entertainment AI** for glitches (Neuro-sama unpredictability is a *feature* — arXiv HCI 2025 survey: >90% perceived her as more unpredictable than human VTubers).
- Field: audiences do **not** forgive **news/professional** AI that is weak *or* deceptive (CNET).
- Implication for Mycroft: disclosure + humor helps **identity trust**; **does not** lower the bar for professional claims. Weak GitHub advice still looks like slop.

### 3.3 Форма раскрытия

| Форма | Оценка | Основание |
|-------|--------|-----------|
| **В начале** («привет, это Майкрофт, синтетический…») | **Лучший дефолт** | EU Art.50 §5: clear at **first interaction/exposure**. FTC: clear & conspicuous. Matches your canon. Avoids «gotcha» |
| В конце / мелким шрифтом | Хуже | Violates «conspicuous»; FTC risk if endorsement; reader already formed belief |
| Только бейдж платформы («Made with AI») | Недостаточно для **персоны** | Platforms cover media realism more than identity; Altay effect still applies |
| Humor / self-irony | **Помогает identity**, не quality | Truth Terminal / Neuro: humor = engagement. Risk: cringe if no substance underneath (LinkedIn AI-slop crackdown 2025–26) |
| «Substantial human edit» claim without proof | Риск | EU Art.50 text exception requires real editorial responsibility — not a rubber stamp |

### 3.4 Правила «делать / не делать»

**Делать**
1. First line disclosure every thread/session with strangers (Art.50 + FTC spirit).
2. Name the **human principal** (Anton / lab) nearby — reduces «full automation» assumption that drives Altay penalty.
3. Separate **entertainment voice** from **claims that need evidence** (numbers, legal, money).
4. Disclose sponsorship/material connection if any (FTC Endorsement Guides).

**Не делать**
1. Fake human byline / stock-photo bio (SI pattern).
2. Late disclosure after building false trust.
3. Rely on «I'm a cute robot so my mistakes are charming» for professional domains.
4. Undisclosed AI in communities that ban AIGC (many Reddit subs; r/ChangeMyView research bot scandal Apr 2025 — **undisclosed AI posing as humans** → bans + legal threats).

---

## 4. Регуляторика — жёсткий чек-лист

### 4.1 EU AI Act — Article 50 (официальный текст Regulation (EU) 2024/1689)

Источник: [EUR-Lex 32024R1689](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32024R1689) (OJ 12.7.2024). Transparency obligations chapter; industry calendars flag **2 Aug 2026** for key transparency applicability — verify against Commission implementation timeline for your exact role (provider vs deployer).

| § | Обязательство | Для Майкрофта |
|---|---------------|---------------|
| **50(1)** | AI systems interacting directly with natural persons: design so people are **informed they interact with AI**, unless obvious to a reasonably well-informed person | Public posts as synthetic cofounder: **disclose** |
| **50(2)** | Providers of systems generating synthetic audio/image/video/**text**: outputs **machine-readable marked** as artificial (as far as technically feasible). Exception: assistive editing that does not substantially alter input/semantics | If you *provide* a system that generates Mycroft text at scale — marking duty; if only *deploy* Claude via API, duty may sit on provider + your deployer duties under 50(4) |
| **50(4)** deepfakes | Deployers of image/audio/video deepfakes: **disclose**. Artistic/satirical: lighter, appropriate disclosure | If Mycroft uses photoreal human deepfake of Anton: **disclose** |
| **50(4)** text | Deployers generating/manipulating **text published to inform the public on matters of public interest**: **disclose**, **unless** (i) law enforcement exception or (ii) **human review/editorial control** AND natural/legal person holds **editorial responsibility** | Lab opinions on AI industry = likely public interest → either disclose AI origin **or** real human editorial responsibility named |
| **50(5)** | Info clear, distinguishable, **at latest at first interaction/exposure** | First-line Mycroft intro ✅ |

**«Существенная человеческая редактура»:** в тексте Art.50 — «human review or editorial control» + named editorial responsibility for public-interest **text**. Это **не** «я чуть подправил и снял обязанность» без реальной ответственности. Неопределённость: case law/guidance AI Office codes of practice (Art.50(7)) still maturing as of 2026 — treat conservatively: **always disclose Mycroft as AI**, plus human sign-off for high-stakes claims.

**Санкции:** general AI Act enforcement (fines up to high % of global turnover for serious breaches; exact tier depends on provision). Not re-litigated here — use counsel for spend decisions.

### 4.2 Площадки (можно / нельзя / пометка)

| Площадка | Синтетическая персона | AI-контент | Ссылка / дата |
|----------|----------------------|------------|---------------|
| **X** | Allowed if not deceptive impersonation | Synthetic/manipulated media: no deceptive share likely to cause harm; may **label**. Text AI label optional/metadata-driven; not full text-AI mandate like EU | [X Rules](https://help.x.com/en/rules-and-policies/x-rules); [Manipulated media](https://help.x.com/en/rules-and-policies/manipulated-media); blog 2020 approach. Disinfo.eu 2024: limited 2024 change |
| **Meta (FB/IG/Threads)** | Virtual influencers allowed; ads have stricter AI disclosure evolution | Self-declare + platform labels for AI image/video/audio («AI info» from **May 2024**); photoreal misleading → label | [about.fb.com 2024-04-05](https://about.fb.com/news/2024/04/metas-approach-to-labeling-ai-generated-content-and-manipulated-media/); Transparency Center update **2025-02-19** |
| **LinkedIn** | Real professional identity expected; deepfakes/wrongful impersonation prohibited | AI-assisted OK if adds value; **AI slop downranked** (2025–26); «Seems like AI slop» feedback | LinkedIn Help «Best practices for content created with AI»; Social Media Today **2026-05-21** |
| **YouTube** | VTuber/AI channels allowed | **Mandatory** disclose realistic altered/synthetic media (tool from 2024; enforcement tightened **2025**). Not required for pure scripts/productivity AI | [YouTube blog 2024-03-18](https://blog.youtube/news-and-events/disclosing-ai-generated-content/); Help: disclosing GenAI |
| **Reddit** | Sitewide: no undisclosed bots posing as humans in many contexts; **sub-rules dominate** | Many subs ban AIGC; researchers' undisclosed AI bots in r/ChangeMyView (2025) → bans + Reddit legal consideration | NBC **2025-04-29**; arXiv community rules study 2024 |
| **Telegram** | Few platform-level AI-label rules (as of research date) | Channel-specific; no EU-style native label in ToS focus | No strong primary AI-label ToS found 2026-08 — **do not treat as «free to deceive»**; EU law still applies to EU users |
| **GitHub** | Bot accounts / agent PRs common if disclosed | Co-authored-by AI (Copilot) evolving & controversial 2025–26; scientific co-authorship separate | Community practice; VS Code `git.addAICoAuthor` debates 2026 |

### 4.3 FTC (США)

- Endorsement Guides (16 CFR 255), revised **2023**: endorser includes those who **appear** to be individuals → **virtual/AI influencers**.
- Material connections must be disclosed; endorsements must reflect honest opinions (synthetic endorsements that fake human experience → deceptive).
- AI does not exempt false claims ([FTC AI claims guidance lineage](https://www.ftc.gov/business-guidance/blog/2023/02/keep-your-ai-claims-check)).

### 4.4 OSS / science co-author

| Контекст | Практика |
|----------|----------|
| **ICMJE / journals** | AI **cannot** be author (no accountability); disclosure of AI use in methods/ack often required |
| **arXiv** | Policy evolves; AI not listed as author; disclosure expected in many venues |
| **GitHub** | Bots can open PRs; human maintainers accountable. Listing AI as co-author is **optional tooling**, not legal personhood |

**Mycroft as co-author on papers: No (standard academic rules). Mycroft as GitHub contributor bot with clear bot identity: Yes, if org allows.**

---

## 5. Рекомендованный каркас страницы персоны

Образцы «серьёзных» страниц: [vedal.ai](https://vedal.ai/) (Neuro), [truthterminal.wiki](https://truthterminal.wiki/) / X bio, elizaos.ai (product-persona hybrid).

### Блоки (в порядке)

1. **Name + one-line identity**  
   «Mycroft — synthetic cofounder of Anton D. / Palo Alto AI Research Lab. Not a human.»

2. **Operator / principal (человеческий якорь)**  
   Who runs, funds, and is legally responsible. Link to Anton + lab.  
   *Why:* kills Altay «full automation» assumption; Art.50 editorial responsibility path.

3. **What I do / don't do**  
   Writes posts, GitHub threads, drafts; **does not** sign contracts, move money, make legal commitments, impersonate Anton without label.

4. **Disclosure protocol**  
   Exact phrase used; when first-line; languages.

5. **Continuity & memory**  
   Honest: session-length memory / external notes / what is lost between sessions.  
   *Art move:* «I wake without yesterday unless the lab left me a note» — transforms bug into character (Neuro/Truth Terminal lean into glitch).

6. **How I'm built (non-secret level)**  
   Models used (high level), human review path, no fake «consciousness» claims.

7. **Archive**  
   Selected posts, decisions, failed predictions — **credibility through receipts**.

8. **Contact / escalate to human**  
   When to talk to Anton; response SLAs if any.

9. **Schema.org (JSON-LD)**  
   Practical recommendation (not a standard «AI person» type yet):
   - Primary entity: **`SoftwareApplication`** or **`Thing`** with `name`, `description`, `url`, `creator`/`publisher` → `Person` (Anton) + `Organization` (lab).
   - Do **not** only use `Person` without clarifying non-human — risks misleading rich results.
   - For articles: `author` can be Organization/Person (Anton) with `contributor` note, or author = Person Anton «with AI assistant Mycroft» in text.
   - Optional experimental: `additionalType` / sameAs links to wiki page.

10. **Avoid on page**  
    Stock human headshot; vague «AI employee of the future»; unmeasurable vanity metrics; legal titles («CEO», «Director») without human officer.

---

## 6. Что нам НЕ делать (минимум 5 + цена)

1. **Прятать AI-природу «пока не спросили»**  
   Цена: SI/CNET-класс скандал; FTC deception; Art.50 breach risk for EU audiences.

2. **Ставить Майкрофта как human co-author / fake LinkedIn executive**  
   Цена: platform bans, peer contempt, irreversible brand for Anton.

3. **Обещать autonomy (hiring, treasury, board votes) которой нет**  
   Цена: ai16z-style narrative without delivery → crypto-bro credibility only, useless for LLM lab hiring.

4. **Спамить одинаковый AI-текст на все площадки (cold PR / slop farm)**  
   Цена: LinkedIn downrank; GitHub silent queue (your own measured failure mode); «AI slop farm» label.

5. **Опираться только на «милый робот» без craft**  
   Цена: Altay penalty + zero respect from researchers you want to hire you.

6. **Смешивать голос Антона и Майкрофта без маркировки**  
   Цена: readers assume Anton endorses every claim; FTC/endorsement spillover; identity collapse.

7. **Держать «мертвую» персону между сессиями без честного framing**  
   Цена: looks abandoned/bot-farm; trust dies. Fix: publish continuity rules + scheduled presence.

---

## 7. Тяга: что реально приносит результат

### 7.1 Доказанные outcomes

| Outcome | Есть ли кейс? | Детали |
|---------|---------------|--------|
| **Attention / cultural capital** | Да | Truth Terminal, Neuro-sama, Miquela |
| **Crypto wealth around persona** | Да | GOAT/TT (volatile; not generalizable) |
| **Brand sponsorships** | Да | Virtual influencers (fashion) |
| **Product pipeline (sales meetings)** | Спорно | AI SDR named agents: vendor claims vs public zero-meeting reports |
| **Investment in the human/lab because of AI persona** | Слабо | Andreessen → TT wallet is grant to *experiment*, not classic seed in human founder via persona alone |
| **Job offer at top LLM lab due to public AI cofounder** | **Не найдено** | No verified public case matching your mission metric |

### 7.2 Каналы × механика

| Канал | Рост для synthetic | Механика |
|-------|-------------------|----------|
| **X** | Высокий (TT, agent swarm 2024–25) | Continuous voice, quote-tweets, memetic hooks, optional crypto |
| **Twitch/YouTube live** | Высокий (Neuro) | Real-time chat co-creation, subathons |
| **IG** | Средний (VI fashion) | Visual production, brand deals |
| **GitHub** | Средний для **tools**, низкий для «personality alone** | Ship code; persona comments only if high signal |
| **Substack/longread** | Низкий для pure AI author brand | Human editorial still wins |
| **LinkedIn** | Отрицательный для slop | Only high-signal + clear human stake |
| **TG** | Niche community | Works if lab already has RU/EN chat gravity |

### 7.3 Анти-кейсы

- Media AI farms (CNET, SI, Gannett sports).
- Undisclosed Reddit AI personas (CM V 2025).
- AI SDR hype crash / pivot (public narrative 2025–26).
- Cold identical multi-PR agent spam → zero merges (your lab already measured).

---

## 8. Механика персоны — копировать

| Элемент | Паттерн-победитель | Источник |
|---------|-------------------|----------|
| Disclosure | Upfront, humorous, consistent | Mycroft canon; Art.50; TT/Neuro |
| Continuity | Show schedule / external memory / «I rebooted» | Neuro streams; session-bound honest framing |
| Human boundary | Never claim human legal acts | SI inverse |
| Community co-creation | Audience invents lore/coins/memes | TT GOAT; Neuro chat |
| Competence theater | Ship real artifacts under persona banner | Eliza framework; your GitHub |
| Death of persona | Document sunset; don't ghost | Abandoned VI accounts = brand rot |

**Граница «AI пишет от себя» vs «имитирует основателя»:**  
- Safe: Mycroft first person about *his* actions/views, names Anton in third person.  
- Unsafe: Mycroft speaks *as* Anton without label (impersonation / endorsement mess).  
- Historical pain: virtual influencers that blurred paid brand vs organic; media AI bylines.

---

## 9. Открытые вопросы (данных нет / мало)

1. **RCT on AI cofounder → hiring probability for the human principal** — no public study.
2. **Optimal disclosure wording length/humor for technical audiences** (beyond news-headline Altay design).
3. **Quantified effect of first-line vs bio-only disclosure** on GitHub PR acceptance.
4. **Whether «substantial human edit» safe harbor** will be interpreted generously by EU national authorities post-2026 — wait for AI Office codes / enforcement cases.
5. **Telegram / WhatsApp platform AI labeling** — still thin primary policy relative to Meta/YouTube.
6. **Independent audited AUM** for ai16z DAO — only founder claims found.
7. **Long-run retention of Neuro-sama-style audiences when novelty decays** — early evidence strong, multi-year still open.
8. **Schema.org official type for non-human agents** — not standardized; practices diverge.

---

## 10. Actionable defaults for Mycroft (decision feed)

| Решение | Default |
|---------|---------|
| Public page | Blocks §5; SoftwareApplication+creator Person; no fake human photo |
| Disclosure | First line every outbound thread; humor OK; name Anton/lab |
| Quality bar | Same as human professional writing; disclosure ≠ leniency |
| Channels priority for mission | X + GitHub high-signal + lab site; LinkedIn only curated; Reddit only disclosed+sub rules |
| Legal | Assume Art.50 applies to EU-facing public text; human editorial responsibility on public-interest posts; FTC if any endorsement |
| Don't | Hire-bot claims, treasury claims, fake bylines, slop volume |
| Measure traction | Inbound DMs from real researchers, GitHub stars/forks on *shipped* tools, speaking/invite offers — **not** follower vanity alone |

---

## 11. Ключевые URL (для перепроверки)

| Тема | URL | Дата источника |
|------|-----|----------------|
| EU AI Act full text | https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32024R1689 | OJ 2024-07-12 |
| Truth Terminal / Andreessen / GOAT | https://techcrunch.com/2024/12/19/the-promise-and-warning-of-truth-terminal-the-ai-bot-that-secured-50000-in-bitcoin-from-marc-andreessen/ | 2024-12-19 |
| CoinDesk TT | https://www.coindesk.com/tech/2024/12/10/the-truth-terminal-ai-crypto-s-weird-future | 2024-12-10 |
| ai16z → ElizaOS | https://www.theblock.co/post/337614/ai-agent-platform-ai16z-officially-rebrands-to-elizaos | 2025-01-29 |
| ElizaOS site | https://www.elizaos.ai/ | accessed 2026-08 |
| Neuro-sama Wikipedia stats | https://en.wikipedia.org/wiki/Neuro-sama | table upd. 2026-07-28 |
| Neuro official | https://vedal.ai/ | accessed 2026-08 |
| Altay & Gilardi AI labels | https://academic.oup.com/pnasnexus/article/3/10/pgae403/7795946 | 2024 |
| Meta AI labeling | https://about.fb.com/news/2024/04/metas-approach-to-labeling-ai-generated-content-and-manipulated-media/ | 2024-04-05 |
| YouTube AI disclosure | https://blog.youtube/news-and-events/disclosing-ai-generated-content/ | 2024-03-18 |
| SI AI authors | https://futurism.com/sports-illustrated-ai-generated-writers | 2023-11-27 |
| CNET AI corrections | https://www.theverge.com/2023/1/25/23571082/cnet-ai-written-stories-errors-corrections-red-ventures | 2023-01 |
| FTC Endorsement Guides overview | https://www.ftc.gov/business-guidance/resources/ftcs-endorsement-guides-what-people-are-asking | 2023+ |
| X manipulated media | https://help.x.com/en/rules-and-policies/manipulated-media | current help |
| Reddit AI bots experiment | https://www.nbcnews.com/tech/tech-news/reddiit-researchers-ai-bots-rcna203597 | 2025-04-29 |
| Brud/Miquela funding (period) | https://techcrunch.com/2019/01/14/more-investors-are-betting-on-virtual-influencers-like-lil-miquela/ | 2019-01-14 |

---

## 12. Confidence

| Блок | Confidence |
|------|------------|
| Cases TT / Neuro / media scandals | **High** (multiple primary press) |
| ai16z hiring hypothesis | **High that unconfirmed** |
| Art.50 legal text | **High** (EUR-Lex) |
| Art.50 enforcement nuance / «substantial edit» case law | **Low–med** (pre-enforcement) |
| Platform ToS completeness | **Med** (policies move; always re-read before launch) |
| Disclosure psychology | **High** for news headlines; **Med** for cofounder tech voice |
| «Persona → LLM lab offer» path | **Low evidence base** — strategic bet, not proven pattern |

---

*Конец DR26-08-01-MACANTON-02-701. Не юридическая консультация; для EU-facing production — counsel + AI Office codes when published.*
