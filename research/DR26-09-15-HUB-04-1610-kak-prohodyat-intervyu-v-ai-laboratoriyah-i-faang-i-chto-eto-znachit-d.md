---
dr_id: DR26-09-15-HUB-04-1610
title: "Как проходят интервью в AI-лабораториях и FAANG — и что это значит для Антона"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Как проходят интервью в AI-лабораториях и FAANG — и что это значит для Антона

> **Заказ:** DR26-09-15-HUB-04-1610 · **Потребитель:** коучинг Антона перед звонками (Пайп A) + обновление interview-prep и `/coach`.
> **Родители:** insight-DR-DR26-09-14-ZB-11-2009-bd-partnerships-fde-eir (кого берут), insight-DR-DR26-08-24-HUB-03-1747-llm-hiring-mechanics (механика найма), insight-2026-08-31-recall-anton-candidate-positioning-full-map (позиционирование).
> Предыдущие ДР закрывали отбор **до** интервью. Этот — первый про **сам процесс**.

## Честность веера

| рельса | режим | объём | источников | в кворум подтверждения |
|---|---|---|---|---|
| chatgpt | codex CLI + web_search | 98 КБ | 231 | ✅ |
| claudeai-bb | `claude -p`, bb-аккаунт | 68 КБ | 45 | ✅ |
| grok | grok CLI, `--prompt-file` | 58 КБ | 27 | ✅ |
| gemini | gemini CLI | 30 КБ | **0** | ⛔ эхо заказа + короткий ответ без ссылок |
| glm | ff-рельса | 14 КБ | 0 | ⛔ 75% файла — наш же заказ |
| claudeai-a2 | `claude -p` | 5.7 КБ | — | ⛔ доехал методологический хвост, не отчёт |

Кворум **сбора** 4/6 взят, кворум **подтверждения** — 3 рельсы. Ниже «сошлись» означает три независимых корпуса, не четыре.

---

## 1. Главное в трёх строках

1. **Дверь у Антона одна и она не FDE-по-умолчанию, а FDE/Applied при зелёном no-AI coding baseline.** Все три рельсы независимо ставят предиктор OpenAI «если ты был одним из первых 10 инженеров стартапа — ты уже делал эту работу», и все три независимо предупреждают: без самостоятельного Python за 40 минут этот луп ломается на живом раунде.
2. **Ломает кандидатуру не техника, а values-раунд Anthropic — и ломает именно тем рефлексом, который у Антона отработан десять лет: питчем.** Формулировка grok: интервьюеры аллергичны к заученному STAR и к энтузиазму без скепсиса. Вопрос «что Anthropic делает неправильно прямо сейчас» с ответом «ничего не приходит в голову» — прямой провал.
3. **«Оркестратор, не кодер» — не позиция, а красная тряпка, и переформулировка известна дословно:** «я проектирую систему, в которой модель — компонент с SLO, и вот как я это измеряю» (grok). Три рельсы сходятся, что сам вопрос «что написал ты, а что модель» в 2026 — ожидаемый, не ловушка; ловушка — расплывчатый ответ на него.

---

## 2. Анатомия лупов (то, где рельсы сошлись)

### Anthropic
Recruiter ~30 мин → coding assessment 60–90 мин (CodeSignal, **не LeetCode**: spec + скрытые тесты, уровни открываются последовательно) → HM/project deep dive 45–60 мин → virtual onsite 4–5 ч (1–2 coding, system design часто про LLM serving/batch inference в Google Doc, **values/culture 45–60 мин с нетехническим интервьюером**) → team matching. Срок 3–6 недель; Glassdoor-агрегат 21 день по 204 отзывам, позитивный опыт — только у 35.8%.

Официально с [careers-страницы](https://www.anthropic.com/careers): всё по Google Meet, гуглить можно, синтаксис знать обязан; половина технического штата пришла **без предыдущего ML-опыта**; независимый ресёрч и OSS просят выносить в верх резюме; реаплай 12 месяцев или раньше при существенном изменении опыта.

⚠️ **Расхождение не разрешено.** grok нашёл два несовместимых утверждения: interviewing.io/Bloomberg говорят «values режет большинство даже после зелёного кода» (late-stage фильтр), а prep-блог hack2hire утверждает жёсткий technical gate — один провал кода отменяет оставшиеся раунды, включая culture. Оба источника заинтересованы (продают моки/подготовку). Практический вывод не зависит от исхода спора: обе версии требуют и зелёного кода, и живого values-ответа.

### OpenAI
Официально ([interview-guide](https://openai.com/interview-guide/)): recruiter → один или несколько skills assessment → финал 4–6 часов с 4–6 людьми за 1–2 дня, virtual по умолчанию → решение примерно за неделю. Всё остальное — вторичка, включая «платный 48-часовой work trial за ~$1000».

FDE-петля (две рельсы независимо): take-home — рабочее приложение на их API + **записанный walkthrough «как для клиента»**, затем 60-минутная защита: почему этот chunk size, что сломается на грязных данных, как под 100×. Цикл 3–4 недели.

📌 **Единственная численная рубрика во всём корпусе** (community-гайд, не компания): recruiter ~80% pass · coding ~55–60% · **decomposition ~40% pass при ~30% веса**. Прыжок к решению до скоупинга назван причиной отказа №1. Цифры некомпанейские — как направление верны, как статистика нет.

### Google DeepMind
Официальный PDF: 4 стадии (initial → skills → final → decision), 4–10 недель, competency-based, STAR рекомендован явно, NDA через Ironclad. **AI на живых интервью и на interview tasks запрещён**, если интервьюер не разрешил. Для не-ML PhD research-дверь практически закрыта; applied/SWE открыта, но coding bar = Google, без модели.

### Mistral
Две публичные ветки: GTM (intro → 1–3 разговора → case study/role-play → values) и science/product/engineering (intro → 2–5 технических упражнений → values → **обязательные references до оффера**). Технический бар жёстче LeetCode: реализация attention/RoPE/sampling с нуля, KV-cache, квантование. Средний цикл 26 дней, AI Engineer — до 60. Equity = французские BSPCE, не RSU.

### xAI
CV + statement of exceptional work → короткий технический скрин → несколько технических сессий → role match. Процесс явно нестабилен: раунды добавляются посреди лупа, HM меняется. Финалы очные. В открытом списке на 15.09.2026 **нет ролей со словами Forward / Solutions / Applied AI** — ближайшая живая дверь — X Developer Platform.

### FAANG-контраст
Amazon: 4–5 интервью, 16 Leadership Principles, **Bar Raiser** из другой команды ведёт debrief; официальный текст говорит о консенсусе, вторичные источники — о праве вето (расхождение в формулировке). Google: hiring committee, который кандидата **не видел**, нужен средний ≥3.5, слабая Googleyness не компенсируется сильным кодом; 6–8 недель. Meta: **AI-assisted coding round** с октября 2025 стал штатным — не «можно ли», а «насколько хорошо ты работаешь с ассистентом»; оси — problem solving, code quality, verification, communication.

---

## 3. AI в найме 2026 — политики расходятся принципиально

| | подготовка | заявка | take-home | живой раунд |
|---|---|---|---|---|
| Anthropic | ✅ поощряют | черновик сам, Claude шлифует | ⛔ без Claude, если не сказано иное | ⛔ без AI |
| OpenAI | спроси рекрутёра | — | зависит от команды | FDE иногда AI-enabled |
| DeepMind | ✅ | — | ⛔ | ⛔ |
| Google (не DeepMind) | ✅ | — | — | пилот: свой Gemini разрешён |
| Meta | — | — | — | AI **встроен в раунд** |
| Amazon | — | — | — | ⛔ дисквалификация |

Вывод, который дали все три рельсы одинаково: **письменная инструкция конкретного раунда выше любого чужого опыта**; политика различается даже внутри одной компании между подразделениями. Спрашивать рекрутёра письмом до раунда.

Детекция: платформы ловят переключение вкладок, вставку, паттерн набора; но самым надёжным методом три источника называют **один незапланированный follow-up-вопрос по коду**, который кандидат якобы написал сам.

---

## 4. Персональный разбор — где профиль Антона ломается

| раунд | риск | перевод в сигнал |
|---|---|---|
| Recruiter | founder/PhD/lab/crypto/fleet = четыре карьеры без функции | одна функция на заявку, первые 30 секунд — какую работу для этой команды он уже делает |
| Coding | «оркестратор, не кодер» против требования production Python и no-AI раундов | не переименовывать себя в кодера; показать, какие модули способен объяснить, изменить и отладить **без модели** |
| Past-work | пять машин и два препринта как любопытные факты | цифры: задачи, пользователи, uptime, инциденты, recovery, eval pass rate, latency, стоимость, карта вклада Антон/люди/LLM |
| Decomposition | прыжок к архитектуре до скоупа | сначала цель и метрика, потом стейкхолдеры, потом тонкий срез с названными рисками |
| Values (Anthropic) | питч-рефлекс, safety-косплей или «revenue решает» | один серый случай, одна настоящая эмоция, одно «вот здесь вы ошибаетесь» |
| Presentation | founder-keynote подавляет диалог | формат технического design review, разрешать перебивать, показывать незащищённые места |
| Committee | харизма не переносится в письменные пакеты разных интервьюеров | каждый раунд повторяет ОДНУ функциональную историю |

**Три почти гарантированных вопроса** (совпали у всех трёх рельс):
1. «Что построил лично ты, а что — модель?»
2. «Ты 10 лет фаундер — почему сейчас найм и что мешает уйти через полгода?» (flight-risk тест; рекрутёры прицельно ищут намёк на «снова запущу своё»)
3. «Как ты понимаешь, что система реально работает?» (evals, failure modes, rollback, стоимость)

**Что конвертируется в сигнал:** production-флот → work sample и Statement of Exceptional Work (редкость, а не хвастовство) · PhD не по ML → буквально совпадает с политикой Anthropic «половина штата без ML-опыта», оправдываться не нужно · препринты → входной билет в applied/RE, **не** в Research Scientist · суперконнектор → только через «три конкретных касания», не «много знакомых» · O-1 + ЕС → строка рекрутёру рано, на техническом раунде не упоминать.

**Что не произносить:** «оркестратор, не кодер» · EIR / «возьмите как фаундера» · крипто как главная идентичность · «код пишет модель» · название своей лаборатории без пользователей и метрик · сравнение себя с research scientist на Applied-заявке · завышенные цифры флота (drill-down будет).

---

## 5. Что осталось не найдено

Ни одна компания не публикует stage-by-stage pass rates — «больше всего режут на раунде X» нигде не подтверждено статистикой. Не найдено: оплата take-home (кроме слуха про OpenAI ~$1000), внутренние scorecard-якоря Anthropic и OpenAI, формальное право вето у Anthropic/OpenAI/xAI/Mistral, официальная реаплай-политика OpenAI/Mistral/xAI. Отдельная находка claudeai-bb: ниша «гайд по интервью в AI-лабораторию» залита LLM-генерированными SEO-фермами, которые дают очень конкретные на вид цифры без первоисточника — одна из них приписала Anthropic четырёхмерную рубрику, тогда как официальная страница говорит «формальной рубрики нет».

---

## Источники
Первоисточники: [Anthropic Careers](https://www.anthropic.com/careers) · [Anthropic Candidate AI Guidance](https://www.anthropic.com/candidate-ai-guidance) · [OpenAI Interview Guide](https://openai.com/interview-guide/) · [DeepMind Interview Guide PDF](https://storage.googleapis.com/deepmind-media/DeepMind.com/Assets/Docs/interviewing-at-google-deepmind.pdf) · [Mistral Careers](https://mistral.ai/careers/) · [xAI Careers](https://x.ai/careers) · [Amazon process](https://www.aboutamazon.com/news/workplace/amazon-interview-process-phone-screens-loops) · [Amazon Bar Raiser](https://www.aboutamazon.com/news/workplace/amazon-bar-raiser) · [Google how we hire](https://careers.google.cn/how-we-hire/interview/) · [Anthropic AI-resistant evaluations](https://www.anthropic.com/engineering/AI-resistant-technical-evaluations)

Вторичные с пометкой заинтересованности: [interviewing.io Anthropic](https://interviewing.io/anthropic-interview-questions) · [Implicator/Bloomberg про $4600 на коучинг](https://www.implicator.ai/anthropic-candidates-spend-4-600-coaching-for-a-no-code-interview/) · [Exponent OpenAI FDE](https://www.tryexponent.com/guides/openai-forward-deployed-engineer-interview) · [FDE rounds.md, community](https://raw.githubusercontent.com/aishwaryanr/awesome-generative-ai-guide/main/interview_prep/roles/forward-deployed-engineer/rounds.md) · [Hello Interview про Meta AI-coding](https://www.hellointerview.com/blog/meta-ai-enabled-coding) · [Axios про values-вопрос Anthropic](https://www.axios.com/2026/08/24/scoop-anthropic-candidates-face-blunt-money-question)

Оригиналы: «внутренний архив лаборатории»

Заказали Tony Dzi (Anton Dziatkovskii) и Майкрофт, Palo Alto AI Research Lab.

## Что из этого построено
- interview-playbook-by-round-type — рабочий плейбук по КАЖДОМУ типу раунда (recruiter · deep dive · coding · system design · decomposition · customer sim · values · presentation) + план тренировки на моках и на живых нецелевых собеседованиях. Заказ Антона голосом 15.09: «хочу потренироваться проходить собеседование на кошках».
- insight-DR-DR26-09-15-HUB-05-1826-interview-behavior-seven-rooms — поведенческая половина: как вести себя в тех же семи комнатах (темп, паузы, давление, акцент, founder-регистр)

## Связано
- 2026-09-21-coaching-platforms-census-adplist-leland-interviewingio — сирота цитирует этот ДР как доказательство для пересмотра вердикта
