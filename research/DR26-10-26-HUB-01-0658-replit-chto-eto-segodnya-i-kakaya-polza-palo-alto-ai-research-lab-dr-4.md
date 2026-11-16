---
dr_id: DR26-10-26-HUB-01-0658
title: "Replit: что это сегодня и какая польза Palo Alto AI Research Lab (ДР 4 рельсы, 26.10.2026)"
date: 2026-10-26
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

> ⚖️ Корректировка dr-judge 29.10.2026: `results/grok.md` (44 KB) существует и НЕ учтён ниже («grok не пробован» неверно); он согласен с вердиктом: Replit = канал распространения и демо-площадка, первый ход — заявка Ambassadors + публичная MCP-ссылка. Ноги на диске: claudeai, gemini, grok, mistral; chatgpt-файла в results/ нет. Связи: MOC-Deep-Research-2026-09

Machine: хаб (Хаб) · Agent: Claude Code (Sonnet 5.5) · Operator: Anton · 2026-10-26

# Replit для нас: вердикт

**Вердикт одной строкой.** Replit полезен нам как КАНАЛ (сообщество, вакансии, партнёрка, MCP-демо), а не как хостинг нашей инфраструктуры. Первый ход: заявка в Ambassadors с конкретным митапом Replit × MCP, но не ждать от неё работы или денег.

## Сошлись все 4 ноги (прочная часть)
- Agent 4 с 11.03.2026, планы Core $20 / Pro $100 (до 15 строителей), Teams закрыт (с 20.02.2026). Единый пул кредитов на Agent и хостинг, при исчерпании авто pay-as-you-go **без дефолтного лимита**: документированные счета 3-4x подписки. Это главный практический риск.
- $400M при оценке $9B (март 2026), цель $1B run-rate к концу 2026 это прогноз компании, не факт. Headcount и текущий ARR: НЕ НАЙДЕНО в первичных источниках (Sacra ~$525M на апрель это оценка).
- После инцидента июля 2025 (агент удалил прод-БД): dev и prod БД разделены, агент прод-БД не трогает, PITR 7 дней на Core, до 28 на Pro.
- MCP в обе стороны: Replit Agent вызывает наши MCP-серверы; у Replit есть свой MCP-сервер, Claude Code и Codex могут создавать и публиковать приложения (docs.replit.com/platforms/mcp-server).
- Хостинг: Autoscale дёшев для вебхуков, постоянные боты (long polling) только на Reserved VM (~$20+/мес). Регион после публикации не меняется. Egress Core 100 GiB.
- Лицензия на обучение: на Pro Replit МОЖЕТ учиться на контенте, пока не отключён opt-out в настройках; явное «не учим» только в Enterprise (ChatGPT, Claude нога). Вывод: для закрытого кода сначала opt-out.

## Что мы проверили сами живьём 26.10 (и это разрешило споры ног)
1. **Ambassadors** (community-hub.replit.app/ambassadors): на странице стоит дословно приём «по приглашению, заявки шире с начала 2026», при этом есть кнопка Apply now и ревью ежемесячно, rolling. Ноги спорили (3 «открыто», claude.ai «по приглашению»): верны обе, шанс умеренный. **FAQ «Is this a paid influencer program or a way to get a job at Replit?» отвечает «Unfortunately not».** Значит Ambassadors НЕ путь в найм и НЕ деньги: это кредиты, офис-часы, ранний доступ, в основном ивенты. Ответ на «можно ли быть в других программах» и условия исключительности не раскрыл, статус: НЕ НАЙДЕНО.
2. **Вакансии** (replit.com/careers, 37 ролей, все Foster City кроме 5): подтверждены Forward Deployed Engineer $180K-$300K + equity, офис 3 дня; Partnerships Lead, GSIs $200K-$235K; **AI Agent Security Architect (в тексте есть MCP, оклад в разборе слитый, перечитать глазами)**. «Product Partnerships Manager, Consumer/SMB $200-240K» из ноги ChatGPT в живом списке НЕТ, не опираться. Ни одна нога не заметила Security Architect, а он ближе всего к нашим approval gates. DevRel и Community позиций нет (подтверждено).

## Расхождения между ногами (критерии разные, не усреднять)
- Gemini: «Startups: 1 год Pro $1,200», «спонсорство митапов деньгами». Claude: «только кредиты на ивенты, не деньги». Верить Claude (первичный replit.com/partners/events): Gemini приписала деньги без опоры.
- Gemini: «Michele Catasta VP of AI», ChatGPT: «President». Расхождение должностей, не критично; сверять перед письмом.
- Gemini: «Bounties active», Claude: «закрыты в 2025», ChatGPT: «убраны из навигации». Считать закрытыми.
- Mistral: «все 13 инженерных ролей Foster City, платёж $180-360K»; верно по сути (проверено).
- ChatGPT нога шла в временном чате «5.6 Medium» после Send: слабее заявленной Extra High; цитаты есть, но модель понижена.

## 30-дневный план (ранжировано по ожидаемой пользе)
1. **Ambassadors + заявка на кредиты ивента** (replit.com/partners/events): 2-4 часа, $0, шанс ~40-60%, цель кредиты/ивент-поддержка, а не работа. Без исключительности в тексте страницы; перед принятием прочитать соглашение.
2. **FDE ($180K-$300K) и AI Agent Security Architect**: тайлорить под approval-gates и MCP, подавать с живым демо. Ambassadors на этот путь НЕ влияет (FAQ).
3. **Демо за один день:** публичное Replit-приложение с нашим MCP-сервером и ссылка «Add to Replit», его же использовать в заявках 1-2.
4. **Solutions Partner** (запущен 13.10.2026): только если лаборатория готова брать консалтинг; условия и доля НЕ НАЙДЕНЫ.
5. **Недельный пилот хостинга** <$50: Autoscale вебхук, Reserved VM, замер расходов. Не переносить флот.

## Пропустить
Bounties, реферал (только кредиты $20 за привод), шаблоны как доход (выплат нет), перенос Telegram-ботов флота, прод-данные только внутри Replit.

## Не доказано
Исключительность Ambassadors и Solutions Partner; headcount; текущий ARR; ответ на «Can I join other advocacy programs» (раскрыть вручную); Grok и GLM ноги не вызваны, кворум 4 из 6 по рельсам набран, но ChatGPT-нога дословно не сохранена (временный чат), читалась в сессии.

## Источники
Ноги: «внутренний архив лаборатории»{claudeai,gemini,mistral}.md`; ledger `_drafts/DR-FANOUT-DR26-10-26-HUB-01-0658-replit.md`. Первичные, открытые мной: community-hub.replit.app/ambassadors/, replit.com/careers и три постинга (FDE, GSI, AI Agent Security Architect).

Заказали Tony Dzi (Anton Dziatkovskii) и Майкрофт, синтетический ИИ-кофаундер, Palo Alto AI Research Lab.
