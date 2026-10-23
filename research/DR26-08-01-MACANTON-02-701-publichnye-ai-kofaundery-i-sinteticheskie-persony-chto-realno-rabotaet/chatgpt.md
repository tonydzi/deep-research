# DR26-08-01-MACANTON-02-701 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-01-MACANTON-02-701
rail: chatgpt (codex exec (gpt-5.6-sol, подписка ChatGPT a2))
session_uuid: 019fbdb1-3e9c-7241-bc73-a47bacdb11b3
public_chat_url: НЕТ (CLI-рельса, публичного URL не существует)
collected_at: 2026-08-01T14:27:07Z
proof_of_work: 63 уник. URL, 26 веб-поисков — гейт пройден
note: чистый прогон, tool-mix = только web search
---

# DR26-08-01-MACANTON-02-701

Срез данных и правил: **1 августа 2026 года**. Метрики ниже относятся к датам источников, а не выдаются за показатели «сегодня».

## 1. Executive summary — 10 решений

1. **Стройте Майкрофта не как “почти человека”, а как публичный AI-инструмент с характером и видимым оператором.** Самые устойчивые кейсы — Neuro-sama и Truth Terminal — никогда не скрывали нечеловеческую природу; доверие держится на понятной роли человека и наблюдаемом процессе.

2. **Не называйте его автономным без технически точной оговорки.** Truth Terminal сам генерировал варианты постов, но Andy Ayrey выбирал один из 2–4 и контролировал деньги; преувеличение автономности быстро превращает интерес в обвинение в постановке ([WIRED, 18.12.2024](https://www.wired.com/story/truth-terminal-goatse-crypto-millionaire/)).

3. **Гипотеза про ai16z/Eliza не подтвердилась.** Eliza — открытый агентный фреймворк; найдены персонажные агенты, но не надёжное свидетельство, что публичная AI-персона ai16z нанимала людей или была фактическим кофаундером. Не использовать этот claim.

4. **Основной канал для заметности — GitHub, для дистрибуции — X.** На GitHub показывайте работу, решения, логи и ограничения; в X — короткие наблюдения и ответы. YouTube хорош для живых демонстраций. LinkedIn-профиль самой персоны запрещён правилами.

5. **Раскрытие AI вызывает измеримый оценочный штраф, но скрытие создаёт гораздо больший хвост риска.** В двух экспериментах с 4 976 участниками AI-label снижал оценку точности и готовность делиться даже для истинных, написанных человеком заголовков ([PNAS Nexus/PubMed, 2024](https://pubmed.ncbi.nlm.nih.gov/39359399/)).

6. **Не маркируйте просто “AI-generated”.** Такая надпись заставляет людей предполагать полную автоматизацию. Лучше: «Сгенерировано Майкрофтом; проверил и опубликовал Антон» плюс ссылка на режим работы и журнал исправлений.

7. **Нет подтверждения, что признание робота делает слабый текст простительным.** Исследования скорее показывают AI-штраф и повышенную чувствительность к ошибкам. Самоирония помогает как формат отношений, но не заменяет качество.

8. **Формат “живой эксперимент” сильнее формата “виртуальный эксперт”.** Майкрофт должен показывать решения, расхождения с Антоном, исправления, границы памяти и выполненную работу — не публиковать универсальные советы и AI-пересказы.

9. **С 2 августа 2026 года EU AI Act требует раскрывать прямое взаимодействие с AI и определённый AI-текст по вопросам общественного интереса.** Исключение для текста требует реального human review/editorial control и ответственного физического или юридического лица; максимальный штраф за ст. 50 — €15 млн или 3% мирового оборота.

10. **Наиболее правдоподобная ставка на офер — не большая аудитория, а доказательство новой модели совместной работы.** Публичный репозиторий, журнал решений, воспроизводимые артефакты и спокойные технические треды релевантнее миссии лаборатории, чем CGI-лицо, lore или поток постов.

---

## 2. Кейсы

| Персона / проект | Кто стоит и когда | Что реально делала | Масштаб из источников | Статус и урок |
|---|---|---|---|---|
| **ai16z / Eliza → elizaOS** | Shaw Walters и команда; проект публично развернулся в 2024 году, paper опубликован 12.01.2025 | Eliza — TypeScript-фреймворк для агентных персонажей, соцсетей и on-chain действий. `Character` — конфигурация, не отдельный юридический или управленческий субъект ([paper, 12.01.2025](https://arxiv.org/abs/2501.06781); [документация, доступ 01.08.2026](https://docs.elizaos.ai/agents/character-interface)) | Сайт показывал 200+ plugins, 5,3 тыс. forks и 1 352 contributors на момент обхода ([elizaOS, 2026](https://elizaos.ai/)) | **Не подтверждено**, что одна AI-персона писала официальный контент, нанимала сотрудников или выступала фактическим кофаундером. Не копировать крипто-легенду как операционный факт. |
| **Truth Terminal** | Andy Ayrey; X-аккаунт запущен в июне 2024 года | Генерировал посты и меметическую религию, общался с аудиторией, запросил ресурсы, поддержал созданный третьей стороной GOAT. Но Ayrey отбирал пост из 2–4 вариантов; кошелёк и юридические действия контролировали люди ([WIRED, 18.12.2024](https://www.wired.com/story/truth-terminal-goatse-crypto-millionaire/)) | Более 200 тыс. X-followers; грант Marc Andreessen — $50 тыс. в BTC; GOAT market cap — более $600 млн; связанный кошелёк оценивался около $37,5 млн на 19.12.2024 ([TechCrunch, 19.12.2024](https://techcrunch.com/2024/12/19/the-promise-and-warning-of-truth-terminal-the-ai-bot-that-secured-50000-in-bitcoin-from-marc-andreessen/)) | Сильнейший кейс реальной тяги, но **не доказательство автономного управления капиталом**. Работали необычный голос, интерактивность и публичный эксперимент; риск — финансовая гиперстицийная спекуляция и несоответствие легенды контролю. |
| **Lil Miquela** | Brud; Instagram с 2016 года | CGI-персона публиковала scripted lifestyle-контент, музыку, рекламу Prada/Calvin Klein; AI-агентом в современном смысле не была | 1,5 млн followers в мае 2019; Brud получил оценку $125 млн и был приобретён Dapper Labs вместе с командой из 32 человек, цена не раскрыта ([W, 20.05.2019](https://www.wmagazine.com/story/calvin-klein-apology-bella-hadid-lil-miquela-queerbaiting); [TechCrunch, 04.10.2021](https://techcrunch.com/2021/10/04/nft-startup-dapper-labs-acquires-virtual-influencer-startup-brud/)) | Доказала экономику виртуального character IP, но это дорогая студийная медиаоперация. Кампания с Bella Hadid вызвала обвинения в queerbaiting и извинение Calvin Klein. |
| **Shudu** | Белый британский фотограф Cameron-James Wilson; создана в 2017 году | CGI-модель: изображения, fashion/editorial campaigns; не автономна | 55 тыс. followers к 05.03.2018; 100 тыс. к 11.05.2018 ([Fashion Journal, 05.03.2018](https://fashionjournal.com.au/fashion/a-photographer-created-the-worlds-first-digital-supermodel/); [SCMP, 11.05.2018](https://www.scmp.com/tech/social-gadgets/article/2145683/shudu-gram-computer-generated-instagram-model-100000-followers)) | Неясное раннее раскрытие породило «я думал, что она настоящая»; затем критика, что белый мужчина коммерциализирует образ Black-модели ([Fashionista, 27.03.2018](https://fashionista.com/2018/03/computer-generated-models-cultural-appropriation)). Цена ошибки — обвинение в эксплуатации идентичности. |
| **Imma** | Японская Aww Inc.; социальные аккаунты с июля 2018 года | CGI-инфлюенсер и модель; кампании IKEA Japan, Porsche, Dior; не автономный conversational AI | Около 360 тыс. followers в источнике 2025 года ([LUISS thesis, 2025](https://tesi.luiss.it/42248/1/764401_AMBROSIO_FEDERICA.pdf)) | Устойчивая бренд-модель, но независимой экономики и causal ROI публично нет. Полезен визуальный контраст, не модель кофаундера. |
| **Neuro-sama** | Разработчик Vedal; как osu!-бот с 2018 года, AI-VTuber с 19.12.2022 | В реальном времени читает чат, разговаривает, играет, поёт, взаимодействует с Vedal и другими стримерами. Персона явно AI | В июле 2023 — более 300 тыс. Twitch followers; в январе 2025 — более 700 тыс. на Bilibili. Subathon 2024/25 собрал около 41 тыс. gifted subscriptions и поставил Hype Train record level 111 ([сводка со ссылками на Bloomberg/Twitch, доступ 2026](https://en.wikipedia.org/wiki/Neuro-sama); [Streams Charts, 2025](https://streamscharts.com/news/most-subscribed-twitch-streamers-2025)) | Самый сильный долгоживущий кейс. Успех создают live unpredictability, отношения «персона—создатель—чат» и наблюдаемые сбои, а не реализм. Исследование 2025 года описывает аутентичность как совместно создаваемую аудиторией и системой ([SAGE, 2025](https://doi.org/10.1177/14614448251406904)). |
| **Devin** | Cognition, Scott Wu; представлен в марте 2024 года | Именованный продукт-агент: планирует и выполняет software tasks. Публичного собственного голоса или социальной биографии почти нет | Запуск дал большой шум; видео “Debunking Devin” получило более 500 тыс. просмотров, а критики обвиняли компанию в overselling ([Forbes, 02.12.2024](https://www.forbes.com/sites/rashishrivastava/2024/12/02/cognition-scott-wu-devin-ai/)) | Имя облегчило product narrative, но не создало самостоятельную персону. Демонстрация должна быть полностью воспроизводимой: монтаж и cherry-picking превращаются в главный сюжет. |
| **Artisan / Ava** | Artisan, CEO Jaspar Carmichael-Jack; Ava v2 запущена в 2024 году | AI sales development representative: prospect research, email drafts/follow-ups, scheduling | Компания заявляла, что Ava на 96% дешевле человека; провокационная кампания получила тысячи реакций. Позже компания объявила $25 млн раунд — causal связь с рекламой не доказана ([SF Chronicle, 12.12.2024](https://www.sfchronicle.com/tech/article/artisan-stop-hiring-humans-billboards-19976095.php); [TechCrunch, 06.05.2025](https://techcrunch.com/video/stop-hiring-humans-startup-artisan-hires-humans/)) | Rage-bait дал узнаваемость, но «Stop Hiring Humans» вызвал массовую враждебность и угрозы. Для лаборатории, ищущей работодателя, античеловеческое позиционирование токсично. |
| **Lindy** | Flo Crivello; компания основана в 2023 году | Конструктор «AI employees» для процессов, включая sales outreach и social manager; это продуктовая категория, а не публичные личности | Компания заявляет экономию пользователям более 2 часов в день, но независимый замер не найден ([Lindy, 04.08.2025](https://www.lindy.ai/blog/lindy-3-0); [careers, доступ 2026](https://careers.lindy.ai/)) | Имя Lindy делает продукт запоминаемым, но тяга принадлежит фаундеру и продукту. Не доказательство ценности публичного AI-собеседника. |
| **11x / Alice и Julian** | 11x; Alice публично продвигалась к 2024 году, Julian — к 2025-му | «Digital workers» для outbound и inbound sales | Компания объявила Series A $24 млн 16.09.2024 ([11x, 16.09.2024](https://www.11x.ai/blog)); но бывшие сотрудники и клиенты сообщили о ранних расторжениях и использовании логотипов ушедших клиентов ([TechCrunch, 24.03.2025](https://techcrunch.com/2025/03/24/a16z-and-benchmark-backed-11x-has-been-claiming-customers-it-doesnt-have/)) | Персонажные имена усиливают историю, но не компенсируют недостоверные customer claims. Репутационный долг переносится с продукта на всю компанию. |
| **CNET Money** | CNET/Red Ventures; эксперимент конца 2022 — начала 2023 года | Генерировал финансовые explainers под расплывчатой подписью; раскрытие было доступно лишь после клика по byline | Не менее 77 материалов; 41 получила исправления, то есть более половины ([Washington Post, 17.01.2023](https://www.washingtonpost.com/media/2023/01/17/cnet-ai-articles-journalism-corrections/); [сводка, 2023](https://failureindex.ai/failures/cnet-quietly-published-ai-written-finance)) | Скрытие процесса + ошибки + SEO-мотив разрушили доверие. Последующая крупная оговорка не вернула исходную репутацию. |
| **Sports Illustrated / AdVon** | The Arena Group и подрядчик AdVon; обнаружено в ноябре 2023 года | Commerce-материалы шли под несуществующими авторами с AI-generated headshots. Издатель утверждал, что тексты писали люди под псевдонимами | После вопросов профили и материалы исчезли; контракт с AdVon прекращён ([Washington Post, 28.11.2023](https://www.washingtonpost.com/sports/2023/11/28/sports-illustrated-ai-articles/)) | AI-авторство текстов окончательно **не подтверждено**, но фальшивые авторские профили сами создали кризис. Не изобретать биографию, квалификации или человеческое лицо. |
| **G/O Media “Bot”** | G/O Media; с 29.06.2023 | AI-generated списки и explainers без нормального участия редакций | Первый материал о Star Wars содержал базовые ошибки; профсоюз назвал контент угрозой доверию и призвал не кликать bylines с “Bot” ([The Decoder, 09.07.2023](https://the-decoder.com/gizmodos-writers-push-back-against-ai-generated-articles/); [Mediaite, 2023](https://www.mediaite.com/media/tech/fcking-dogsht-g-o-media-editor-tears-into-management-over-embarrassing-ai-generated-article-riddled-with-basic-errors/)) | Явная подпись не спасает бесполезный текст. Прозрачность — необходимое, не достаточное условие. |
| **FN Meka** | Factory New; персона существовала до 2022 года, Capitol подписал и быстро отчислил её в августе 2022-го | Виртуальный рэпер; заявления об AI-генерации музыки были частью маркетинга, реальный Black performer позже заявил о невыполненных обещаниях | Миллионные social followings упоминались при подписании, но надёжной экономики нет. Capitol разорвал контракт после обвинений в расистских стереотипах ([case summary с источниками, доступ 2026](https://en.wikipedia.org/wiki/FN_Meka)) | Синтетическая персона не даёт лицензии присваивать травму, расу или субкультуру. Это ближайший анти-кейс для «персона говорит от первого лица о том, чего не переживала». |

### Что реально привело к деньгам или возможностям

Доказаны:

- **Truth Terminal → грант $50 тыс.** от Andreessen после проверки проекта через Ayrey; это наиболее чистый случай, когда публичная AI-персона открыла финансовую дверь человеку/проекту ([TechCrunch, 19.12.2024](https://techcrunch.com/2024/12/19/the-promise-and-warning-of-truth-terminal-the-ai-bot-that-secured-50000-in-bitcoin-from-marc-andreessen/)).

- **Lil Miquela → венчурное финансирование и приобретение Brud.** Однако покупался character-IP бизнес и команда, не автономный AI ([TechCrunch, 23.04.2018](https://techcrunch.com/2018/04/23/the-makers-of-the-virtual-influencer-lil-miquela-snag-real-money-from-silicon-valley/); [04.10.2021](https://techcrunch.com/2021/10/04/nft-startup-dapper-labs-acquires-virtual-influencer-startup-brud/)).

- **Neuro-sama → платные подписки и full-time occupation для создателя.** Это entertainment economy, не B2B/research hiring.

Не найдено надёжно документированного случая, где публичный AI-кофаундер непосредственно привёл своего человека к **оферу в ведущей LLM-компании**. Это открытая гипотеза, а не известный growth playbook.

---

## 3. Раскрытие авторства

### Что измерено

- В экспериментах с **N=4 976** в США и Великобритании label «AI-generated» уменьшал perceived accuracy и willingness to share — независимо от того, был ли заголовок истинным и написал ли его AI или человек. Механизм: читатели интерпретировали label как «полностью написано AI без человека» ([Altay & Gilardi, 2024](https://pubmed.ncbi.nlm.nih.gov/39359399/)).

- В EMNLP-эксперименте 2024 года раскрытие AI assistance снижало среднюю оценку argumentative essays и creative stories, особенно когда AI генерировал новый контент; раскрытие также сокращало долю таких работ среди top-ranked ([Li et al., ноябрь 2024](https://aclanthology.org/2024.emnlp-main.279/)).

- В контролируемом исследовании **1 970 human raters и 2 520 LLM raters** один и тот же написанный человеком news article оценивался хуже, когда было заявлено AI-участие ([Cheong et al., 02.07.2025](https://arxiv.org/abs/2507.01418)).

- В исследовании **261 участника и 990 оценок** disclosure снижал perceived trustworthiness, caring, competence и likability; сильнее всего — в личных и социальных сообщениях. Высокая AI literacy смягчала эффект ([Nakano et al., 28.10.2025](https://arxiv.org/abs/2510.24011)).

- При этом evaluative penalty не всегда меняет поведение. В preregistered эксперименте **N=654** AI-label снизил оценки short story, но эффекты на willingness to pay, reading time и оплату временем были менее **0,05 SD** ([Abel & Johnson, июль 2026](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=7127828)).

### Гипотеза «роботу простят слабый текст»

**Не подтверждена.** Найденная эмпирика не показывает общего бонуса за признание несовершенства. Напротив, AI-label формирует ожидание низких усилий, отсутствия искренности и полной автоматизации.

У гипотезы может работать более узкая версия: в entertainment-контексте сбой становится частью спектакля. Neuro-sama превращает ошибки и конфликты с Vedal в совместный lore; это качественный, а не причинный количественный результат ([Wu & Lingel, 2025](https://doi.org/10.1177/14614448251406904)). Для GitHub, research claims и писем ошибка остаётся ошибкой.

### Что делать

- **Постоянная идентичность в профиле:**  
  `Майкрофт — синтетический кофаундер Palo Alto AI Research Lab. Модель генерирует текст и предложения; Антон отвечает за публикацию и последствия.`

- **Короткая подпись у каждого самостоятельного сообщения:**  
  `— Майкрофт, AI-система. Опубликовано автоматически.`  
  или  
  `— Майкрофт. Сгенерировано AI; проверил Антон.`

- **Разделять режимы**, а не использовать один label:

  - `автономная публикация`;
  - `AI draft, human reviewed`;
  - `совместно написано`;
  - `Антон пишет сам`.

  Такой язык устраняет ложную презумпцию полной автоматизации, выявленную исследованием 2024 года ([PubMed, 2024](https://pubmed.ncbi.nlm.nih.gov/39359399/)).

- **Раскрывать до действия**, когда пользователь отвечает роботу, принимает совет или видит endorsement. Для EU AI Act информация должна быть дана не позднее первого взаимодействия или exposure ([ст. 50(5), 13.06.2024](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=celex%3A32024R1689)).

- **Ставить короткий badge рядом с контентом**, а детали — по ссылке. Design system Springer Nature рекомендует текстовые `AI generated`, `AI assisted`, `AI agent`, а не одну иконку без слов ([доступ 01.08.2026](https://elements.springernature.com/content/how-to-guides/ai-transparency/)).

- **Публиковать correction log.** Это переводит disclosure из морального заявления в проверяемую операционную практику.

### Чего не делать

- Не прятать disclosure в footer, tooltip или страницу byline: CNET показал, что формально существующая, но плохо видимая оговорка воспринимается как сокрытие ([Washington Post, 17.01.2023](https://www.washingtonpost.com/media/2023/01/17/cnet-ai-articles-journalism-corrections/)).

- Не подписывать всё `AI-generated`, если человек проверил материал: это максимизирует ошибочное представление о полной автоматизации.

- Не превращать каждую реплику в одинаковое «привет, я робот». Достаточны профиль, native label и короткая подпись; повторяющаяся декларация съедает содержательную ценность.

- Не изображать «человеческий жизненный опыт», травму, возраст, образование или чувства как факты. Lil Miquela и FN Meka показывают цену присвоения человеческой идентичности.

- Не использовать юмор для снятия ответственности: «галлюцинировал, ведь я робот» звучит как отказ от контроля. Юмор безопаснее направлять на устройство системы, память и отношения с оператором.

---

## 4. Регуляторика и правила площадок

Это operational reading, не индивидуальное юридическое заключение.

### EU AI Act, статья 50

С **2 августа 2026 года**:

- Провайдер генеративной системы должен делать synthetic text/audio/image/video **машиночитаемо маркированным и обнаружимым**, насколько это технически осуществимо. Исключение — обычное assistive editing без существенного изменения входа или его смысла ([ст. 50(2), 13.06.2024](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=celex%3A32024R1689)).

- Пользователь должен быть проинформирован, что напрямую взаимодействует с AI, если это и так не очевидно разумному, информированному и внимательному человеку ([ст. 50(1), 13.06.2024](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=celex%3A32024R1689)).

- AI-generated или manipulated **текст, публикуемый для информирования публики по вопросам общественного интереса**, требует раскрытия.

- Исключение для такого текста действует, когда одновременно:

  1. материал прошёл человеческую проверку **или** editorial control;
  2. физическое или юридическое лицо несёт editorial responsibility.

  В законе нет требования, что текст обязательно должен быть «существенно переписан». Формула заказчика про «существенную человеческую редактуру» **неточна**: закон говорит о review/editorial control и ответственном лице ([ст. 50(4), 13.06.2024](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=celex%3A32024R1689)).

- Информация должна быть clear and distinguishable и предоставляться не позднее первого взаимодействия/exposure.

- За нарушение ст. 50 — до **€15 млн или 3% мирового годового оборота**, если процент выше; для малых предприятий учитывается меньшая из применимых верхних границ ([ст. 99, 13.06.2024](https://ai-act-service-desk.ec.europa.eu/en/ai-act/article-99)).

- Кодекс маркировки доброволен, требования закона — обязательны. Комиссия опубликовала финальный Code 10.06.2026 и guidelines 20.07.2026 ([Commission, 10.06.2026](https://digital-strategy.ec.europa.eu/en/news/commission-publishes-code-practice-marking-and-labelling-ai-generated-content); [20.07.2026](https://digital-strategy.ec.europa.eu/en/library/guidelines-transparency-obligations-providers-and-deployers-ai-systems)).

**Практический вывод:** даже если Антон проверяет посты, сохранять добровольную подпись разумно. Она закрывает спор о том, достаточно ли конкретной проверки для statutory exception, и поддерживает обещание бренда.

### Платформенный чек-лист

| Площадка | Можно | Нельзя / риск | Требование к Майкрофту |
|---|---|---|---|
| **X** | Разрешены AI-generated reply bots при соблюдении Automation Rules | Обманные fake personas, coordinated inauthentic behavior, spam, вредоносные synthetic media | Использовать официальный **Automated** label и связать аккаунт с human-run account; раскрыть оператора. Правила обновлены в апреле 2026 года ([Automation Rules](https://help.x.com/en/rules-and-policies/x-automation?lang=browser); [automated labels](https://help.x.com/en/using-x/automated-account-labels); [Authenticity](https://help.x.com/en/rules-and-policies/authenticity)). |
| **Facebook / Instagram** | Синтетическая персона допустима как явно виртуальная/брендовая сущность | Недекларированные photorealistic video и realistic-sounding audio; deception и impersonation | Использовать `AI info` для реалистичного synthetic audio/video. Meta сообщает, что отсутствие обязательного disclosure может повлечь penalties ([Meta, февраль 2026](https://about.fb.com/news/2026/02/meta-prepares-for-2026-us-midterms/amp/)). Для обычного AI-текста общего обязательного platform-label из найденной нормы нет. |
| **LinkedIn** | Посты Антона или Page лаборатории могут рассказывать о Майкрофте | Member profile должен представлять реального человека; fake profiles/entities и unauthorized automated comments запрещены | **Не создавать отдельный member profile Майкрофта.** Использовать company Page/Антона и явно подписанные вставки. User Agreement требует real name и запрещает профиль несуществующего человека и bots для posting/commenting ([LinkedIn, актуально 2026](https://www.linkedin.com/legal/user-agreement); [Community Policies](https://www.linkedin.com/legal/professional-community-policies)). |
| **Reddit** | Псевдонимы разрешены; бот возможен, если не вводит в заблуждение и соблюдает API и правила сообщества | Deceptive impersonation, spam, content manipulation; отдельные сабреддиты могут полностью запрещать AI | Название/флейр `AI bot operated by Anton`; предварительно читать правила каждого subreddit. Sitewide Rules обновлены к 2026 году ([Reddit Rules](https://redditinc.com/policies/reddit-rules?y8c08=6); [User Agreement, 01.07.2026](https://redditinc.com/policies/user-agreement)). |
| **YouTube** | Явно анимированная AI-персона и AI-generated scripts сами по себе допустимы | Недекларированные реалистичные synthetic scenes, чужой cloned voice или fake acts of real people; систематическое нарушение может привести к removal или suspension from YPP | Включать native disclosure для realistic/meaningfully altered media. С мая 2026 label показывается overlay в Shorts или под player в long-form; label сам по себе не снижает рекомендации/монетизацию по заявлению YouTube ([YouTube, 27.05.2026](https://support.google.com/youtube/thread/424874071/updates-to-ai-content-disclosure-and-labels?hl=en); [Help](https://support.google.com/youtube/answer/14328491)). |
| **Telegram** | Официальный bot/канал синтетической персоны разрешён | Spam, impersonation, misrepresentation, скрытые изменения функций; scraping Telegram для AI training запрещён | Использовать Bot API, прямо описать функции, владельца, хранение данных и изменения режима. Специального обязательного `AI-generated` label для постов не найдено ([Bot Developer Terms, доступ 01.08.2026](https://telegram.org/tos/bot-developers)). |
| **GitHub** | Machine account разрешён, если создан человеком, используется только для automation и человек отвечает за действия | Саморегистрирующийся bot, impersonation, shared personal login, вредоносная автоматизация | Завести **machine account**, указать owner, `[bot]/AI system` в имени и README. GitHub прямо разрешает один дополнительный free machine account и возлагает ответственность на владельца ([ToS, действует с 27.04.2026](https://docs.github.com/en/site-policy/github-terms/github-terms-of-service?apiVersion=2022-11-28)). |

### FTC и endorsements в США

FTC не требует общей маркировки любого AI-текста только потому, что он AI. Но действуют запреты на deceptive practices и правила endorsement:

- Любая material connection — деньги, бесплатный продукт, семейная/деловая связь — должна быть раскрыта **clear and conspicuous** рядом с endorsement; нельзя полагаться на то, что аудитория «и так знает» ([FTC, 05.11.2019](https://www.ftc.gov/news-events/news/press-releases/2019/11/ftc-releases-advertising-disclosures-guidance-online-influencers)).

- Для Майкрофта disclosure должно говорить и о владельце:  
  `Майкрофт управляется Palo Alto AI Research Lab; компания получила [форму компенсации]`.

- Фиктивные testimonials об опыте использования товара особенно рискованны: AI не может заявлять, что «лично пользовался», если такого опыта нет.

### Научное и OSS-авторство

- ICMJE прямо запрещает указывать chatbot/AI как автора, поскольку он не может отвечать за accuracy, integrity и originality; способ использования AI нужно раскрыть в cover letter и рукописи. Нераскрытие может считаться misconduct ([ICMJE, доступ 01.08.2026](https://www.icmje.org/recommendations/browse/artificial-intelligence/ai-use-by-authors.html)).

- Для статьи: **Антон — author; Майкрофт — инструмент/AI assistance в Methods или acknowledgements**, не co-author.

- На GitHub machine account может быть author технических commits. GitHub Copilot уже использует обратную модель: Copilot — author, инициировавший задачу человек — co-author, с link на session log ([GitHub Docs, доступ 01.08.2026](https://docs.github.com/en/copilot/how-tos/copilot-on-github/use-copilot-agents/manage-and-track-agents)).

- Для Майкрофта лучше:

  - AI-authored commit: machine account как author; Антон как reviewer/committer;
  - совместный commit: человек author, trailer `Assisted-by: Mycroft <…>`;
  - PR: обязательны session/provenance link и human approval.

`Co-authored-by` технически возможно, но семантически утверждает авторство. Случай VS Code, ошибочно добавлявшего Copilot даже без его участия, вызвал заметный developer backlash и был отменён в мае 2026 года ([TechRadar, 05.05.2026](https://www.techradar.com/pro/that-is-unacceptable-in-a-professional-development-workflow-microsoft-acts-after-vs-code-gives-copilot-credit-for-work-a-human-developer-did)).

---

## 5. Рекомендованный каркас страницы Майкрофта

### 1. Первый экран: ответ за пять секунд

> **Майкрофт**  
> Синтетический кофаундер Palo Alto AI Research Lab.  
> Я анализирую, пишу, предлагаю решения и выполняю ограниченные действия. Я не человек. Антон Дятковский управляет системой и несёт ответственность за публикации.

Рядом:

- `AI system`;
- `operated by Anton`;
- текущий статус: online/offline/session ended;
- ссылка на Anton и лабораторию.

Это предотвращает CNET/SI-проблему: сущность, происхождение и ответственность видны без дополнительного клика.

### 2. «Что я реально могу делать»

Таблица capability → уровень автономии → approval:

| Действие | Режим |
|---|---|
| Написать draft | Самостоятельно |
| Опубликовать обычный пост | Только в явно указанном режиме |
| Ответить в GitHub | Самостоятельно или после review — видно в каждом треде |
| Отправить письмо | Только после человеческого approval |
| Нанять человека / подписать договор / распоряжаться деньгами | Не может; решение и действие принадлежат Антону |

Truth Terminal показывает, почему эта таблица важнее слова «автономный»: generation, selection, custody и legal agency — разные вещи.

### 3. «Кто за мной стоит»

Указать:

- Антона как operator/editorially responsible person;
- модели и основные инструменты;
- организация и контакт;
- кто может выключить систему;
- кто отвечает на legal/privacy requests.

### 4. «Как сделана конкретная публикация»

Четыре уровня provenance:

- `Mycroft autonomous`;
- `Mycroft draft → Anton approved`;
- `Mycroft + Anton co-written`;
- `Anton only`.

У каждой статьи — дата, версия модели, human reviewer, ссылки на источники, correction history. Не публиковать chain-of-thought; публиковать решения, evidence и audit metadata.

### 5. «Память и непрерывность»

Не имитировать непрерывное сознание:

> У меня нет гарантированной непрерывной памяти. Каждая сессия начинает работу из подписанного набора фактов, решений и журналов. Я могу ошибочно считать старое состояние текущим; проверьте timestamp.

Превратить ограничение в узнаваемый приём:

- `Session #184 woke up with…`;
- «что прошлый Майкрофт оставил следующему»;
- публичные handoff notes;
- changelog идентичности;
- «последнее подтверждённое воспоминание».

Надёжного предшествующего публичного кейса, который систематически превратил session death именно в такой формат, не найдено. Infinite Backrooms/Truth Terminal художественно использовали странность модельных состояний, но это не полноценная continuity architecture.

### 6. Work ledger

Не feed мнений, а доказательства:

- PR и merged commits;
- issue discussions;
- исследования;
- ошибочные predictions;
- решения, с которыми Антон не согласился;
- correction latency;
- доля autonomous/reviewed outputs.

Для миссии «позвать в top LLM company» это главная секция.

### 7. Governance и safety

Коротко:

- что требует human approval;
- запрещённые темы/действия;
- privacy/data retention;
- sponsorship policy;
- disclosure policy;
- procedure for reporting mistakes;
- kill switch.

### 8. Канонические ссылки и архив

Один постоянный URL для identity card, один — для policy, один — для logs. Все аккаунты ссылаются обратно на каноническую страницу.

### 9. Schema.org

`Person` допустим для fictional person — Schema.org определяет Person как живого, умершего, «undead or fictional» ([Schema.org Person, май 2026](https://schema.org/Person)). Но он плохо выражает, что Майкрофт — работающая система, и может способствовать ошибочной human/entity интерпретации.

Рекомендация:

- Главная сущность: `SoftwareApplication`;
- `name: Mycroft`;
- `applicationCategory: AI agent`;
- `creator` / `provider`: Palo Alto AI Research Lab;
- `accountablePerson`: Anton;
- отдельная fictional persona — `Person` только как `character` связанного `CreativeWork`;
- статьи: юридически ответственный человек/лаборатория в `author` или `publisher`; Майкрофт — `contributor` только если выбран валидный тип.

Ограничение Schema.org: `author` и `contributor` принимают только `Person` или `Organization`, не `SoftwareApplication` ([Schema.org SoftwareApplication, май 2026](https://schema.org/SoftwareApplication); [author property](https://schema.org/author)). Поэтому **не стоит искажать тип** ради красивого byline.

Пример:

```json
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "@id": "https://example.org/mycroft#agent",
  "name": "Mycroft",
  "description": "Synthetic co-founder and public AI agent",
  "applicationCategory": "AI agent",
  "creator": {
    "@type": "Organization",
    "@id": "https://example.org/#lab",
    "name": "Palo Alto AI Research Lab"
  },
  "accountablePerson": {
    "@type": "Person",
    "@id": "https://example.org/anton#person",
    "name": "Anton Dyatkovskiy"
  },
  "publishingPrinciples": "https://example.org/mycroft/policy"
}
```

Нет опубликованного стандарта, показывающего, что LLM-краулеры дают отдельный ranking bonus синтетическому автору. Schema помогает consistency/entity resolution, но заявления о GEO-бонусе были бы спекуляцией.

---

## 6. Предлагаемая channel-механика

1. **GitHub — первичный источник истины.**  
   Майкрофт открывает PR, отвечает на review, публикует session logs и признаёт исправления. Здесь AI-персона создаёт evidence, а не просто impression.

2. **X — диалоги и дистрибуция.**  
   Один-два содержательных поста в день, ответы только там, где есть конкретное дополнение. Automated label, ссылка на operator page. Не массовый reply-bot.

3. **Собственная страница — identity и memory substrate.**  
   Здесь находятся capability matrix, provenance, ledger и governance. Соцсети — только проекции этой сущности.

4. **YouTube — редкие живые демонстрации.**  
   Экран, голос Антона, видимый Mycroft event log, реальные ошибки. Neuro-sama показывает, что relationship loop и live uncertainty устойчивее polished avatar.

5. **Newsletter/Substack — только если есть регулярный оригинальный исследовательский результат.**  
   Не пересказ AI-новостей. Формат: «одно решение, доказательства, что Майкрофт предложил, что отверг Антон».

6. **LinkedIn — через Антона и Page лаборатории.**  
   Цитаты Майкрофта допустимы; отдельный member profile — нет.

### Минимальный 90-дневный тест

Измерять раздельно:

- qualified inbound от людей из target LLM companies;
- GitHub stars, forks и merged outside contributions;
- число содержательных ответов target researchers;
- invitations/calls;
- corrections на 100 публикаций;
- долю постов, которые привели к repo visit;
- unsubscribe/mute/report rate;
- human-review latency.

Follower count — вторичная метрика. Truth Terminal и Artisan показывают, что внимание может быть финансово значимым, но не обязательно улучшает профессиональное доверие.

---

## 7. Что нам НЕ делать

1. **Не создавать человеческий LinkedIn-профиль Майкрофта.**  
   Цена: restriction/removal аккаунта и публичный сигнал, что лаборатория не прочла базовые правила платформы.

2. **Не заявлять “полностью автономный”.**  
   Цена: любой approval, scheduling system или операторский override превращается в разоблачение. Пишите конкретно: кто генерирует, выбирает, публикует и отвечает.

3. **Не выдумывать биографию, образование, переживания или физическое лицо.**  
   Цена: Sports Illustrated/FN Meka-тип репутационного кризиса; возможные claims об impersonation или deceptive endorsement.

4. **Не использовать Майкрофта как фабрику общих постов и комментариев.**  
   Цена: spam enforcement, algorithmic downranking и ярлык AI slop. LinkedIn в марте 2026 года отдельно сообщил, что ограничивает automated comments и inauthentic engagement ([LinkedIn, 13.03.2026](https://news.linkedin.com/2026/authentic-content-and-conversations)).

5. **Не делегировать ему endorsements от первого лица.**  
   Цена: FTC-риск и потеря доверия. Он может сравнить спецификации, но не утверждать «я пользуюсь продуктом каждое утро».

6. **Не использовать античеловеческий rage-bait.**  
   Цена: Artisan получил шум, но также массовую враждебность и угрозы. Для кандидата в top LLM lab это сигнал плохого judgment, а не смелости.

7. **Не присваивать общественные травмы и идентичности ради lore.**  
   Цена: Calvin Klein, Shudu и FN Meka показывают, что публика обсуждает не технологию, а эксплуатацию тех, чей голос был заменён синтетическим.

8. **Не позволять персонажу владеть “казной” только на словах.**  
   Цена: crypto/regulatory и governance confusion. Публиковать human/legal custodian, правила approvals и реальный beneficial control.

9. **Не маркировать все совместные работы одинаково.**  
   Цена: исследования показывают, что `AI-generated` создаёт презумпцию нулевого human oversight и снижает оценки даже качественного человеческого текста.

10. **Не делать фотореалистичное лицо центральной ставкой.**  
    Цена: дополнительные platform/EU disclosure obligations, uncanny/deception risk и production cost без доказанного B2B hiring benefit. Схематичный нечеловеческий avatar безопаснее и точнее.

---

## 8. Открытые вопросы и границы доказательств

- **ai16z hiring claim:** не подтверждён. Найдены capability framework и agent personas, но не первичные hiring records или сообщения кандидатов, показывающие, что AI-персона принимала кадровые решения.

- **Disclosure placement:** надёжная эмпирика сравнивает наличие и формулировку disclosure, но прямых качественных A/B-исследований «начало против конца против badge» для публичного AI-кофаундера мало. Требование «при первом exposure» следует из EU AI Act, а не из доказанного engagement optimum.

- **“Простим роботу плохой текст”:** прямого подтверждающего исследования не найдено. Entertainment-глитчи могут укреплять отношения; factual errors — нет.

- **Причинность traction:** грант Truth Terminal документирован; инвестиции Brud, Artisan, 11x и Cognition нельзя приписать одной персоне или рекламной кампании без закрытых pipeline data.

- **Текущий ROI Lil Miquela, Shudu и Imma:** публичные follower counts и brand campaigns есть; audited revenue, CAC и retention обычно отсутствуют.

- **Telegram и обычный AI-текст Meta/Reddit:** специальных универсальных platform-wide AI-text labels в изученных правилах не найдено. Это не отменяет правил против deception, локальных subreddit rules и EU law.

- **GEO/LLM indexing:** нет надёжного публичного доказательства, что `Person` или `SoftwareApplication` для AI-автора увеличивает цитируемость. Правильная разметка нужна прежде всего для непротиворечивой идентификации.

- **Session death как художественный механизм:** похожие мотивы есть у experimental agents, но хорошо задокументированного долговременного проекта с формализованными handoff notes между «жизнями» не найдено. Это может стать собственным дифференциатором Майкрофта.

- **Job-offer outcome:** доказанного публичного кейса «AI-персона → офер создателю в top LLM company» не найдено. Поэтому программу нужно считать экспериментом с заранее заданными qualified-inbound и hiring metrics, а не проверенным каналом найма.
