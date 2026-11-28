---
dr_id: DR26-11-20-MACSTUDIOM4-01-0251
title: "Срок жизни кэша промпта у Codex, Claude Code и Gemini: синтез ДР и наш замер"
date: 2026-11-20
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Срок жизни кэша промпта: синтез DR26-11-20-MACSTUDIOM4-01-0251

Заказали Tony Dzi (Anton Dziatkovskii) и Майкрофт, Palo Alto AI Research Lab. Потребитель: статья EN+RU и подкаст NotebookLM (заказ Антона 20.11.2026, голосом).
Расширяет выданный, но не исполненный DR26-11-15-HUB-03-2350 и дополняет insight-DR-DR26-11-19-MACSTUDIOM4-04-1621-storozh-ostyvshey-sessii (там про инструменты-сторожа, здесь про сам срок жизни кэша).

Ноги (все прошли `dr_leg_gate --strict --prompt`): ChatGPT Deep research, Pro, Extra High (246 источников); claude.ai Research, Fable 5.2 Extra (189); Grok Expert с навыком deep-research (162, в тексте 7 ссылок); Mistral Vibe deep-research (311). Тела: «внутренний архив лаборатории».
Оператор веера Claude: проба Codex 20.11 10:53 UTC ответила «The browser access host is unavailable».

Проверка: 22 несущих факта открыты по первоисточникам двумя помощниками 20.11.2026 (curl, код 200, цитата со страницы). Ноги дали наводки, факты ниже взяты со страниц. Mistral в сравнительную выжимку не вошёл, прочитан только гейтом.

## Главное в пяти строках

1. **Claude Code: стена на 60-й минуте подтверждена документацией.** Часовой кэш главного диалога только на подписке в пределах плана; на кредитах, API-ключе и облачных провайдерах 5 минут (code.claude.com/docs/en/prompt-caching). Наш замер: 58-60 мин 0 холодных из 10, 60-62 мин 6 из 7, 62-65 мин 11 из 11.
2. **Codex: стены нет, есть склон, и это согласуется с документацией.** Для GPT-5.6+ срок кэша задан как минимум (30 минут по умолчанию, 12 часов по запросу), хранить могут дольше. Режима «24h» у gpt-6.1-sol нет вообще, строка в нашем брифе про «до 24 ч без наценки» устарела.
3. **Заявление «кэш навсегда» с нашими журналами не сходится.** Tibo (OpenAI), 21.10.2026: для вошедших пользователей Codex кэш хранится «forever». Наш контроль после 22.10: 169 сессий, 53 вызова после паузы больше часа, 39 холодных. Выборка мала, узел один. Независимо то же видит openai/codex#61757 (26 912 запросов, вход через ChatGPT, 10-16.11): промахи первого запроса после длинного хода, «not an idle expiry».
4. **Prior art есть, но не такой.** TraceLab (arXiv 2606.30560, около 4 300 сессий): «after 1 hour, almost all steps miss the cache», точечная диаграмма, Claude и Codex вместе. Copilot (arXiv 2608.00101, 13 млн сессий): 90% внутри хода, 55% на границе ходов. Таблицы по корзинам паузы для Codex до 24 часов никто не публиковал: этот кусок у нас новый.
5. **Как кэш считается в лимитах подписки, вендоры не публикуют.** Это главный предел нашей модели денег. Единственное подтверждённое: у вошедшего Codex «no separate cache-write charge» (learn.chatgpt.com/docs/pricing).

## Где ноги ошиблись (в статью не брать)

- Grok: «правка CLAUDE.md сбрасывает кэш, подтверждено документацией». Документация говорит обратное: префикс остаётся целым.
- Grok и claude.ai: «prior art для кривых по паузе нет». Есть TraceLab §7.2 и Copilot.
- ChatGPT: «эффорт, MCP и обновление клиента не являются документированными сбросами Claude Code». Являются (эффорт кроме Fable 5.1+ и моделей 5.5).
- claude.ai: назвал «ближайшим независимым аналогом» таблицу в anthropics/claude-code#66115. Это наш собственный комментарий от 19.10.2026 (tonydzi). Ссылаться на него как на чужое подтверждение нельзя.
- Grok: цитаты OpenAI в кавычках оказались пересказом; «0.025x у Fable 5.x» верно только для Fable и Mythos 5.1 и 5.2.
- Совпадение трёх ног в объяснении склона Codex подтверждением не считаю: каждая привязала его к своему режиму документации, и ни одна не заметила, что наш флот сидит на GPT-5.6+.

## Что сбрасывает кэш кроме времени (по документации Claude Code)

Смена модели; смена эффорта (не на всех моделях); fast mode; подключение или снятие MCP-сервера; переключение плагина; отказ инструменту; сжатие; обновление клиента; накопление картинок. Не сбрасывает: правка CLAUDE.md посреди сессии. У Codex сверх этого наблюдаются промахи внутри хода при неизменных настройках (issues 54091, 35925, 30425, не проверял сам).

## Как решают «продолжить или начать заново»

- Anthropic: автосжатие от 200K после примерно 54 минут простоя (с версии 2.1.299, 14.10.2026); совет поддержки «/clear when starting a new task, and /compact when continuing a long one».
- Zed и Amp: сжатие на 90% окна. Aider: пинги для удержания кэша, по умолчанию выключены.
- Практики: сжимать простаивающие сессии только выше 450K (dthinkr, #66115); наши 300K для Claude и 6 часов паузы для Codex.
- Довод за новую сессию помимо денег: context rot (Chroma, 18 моделей). Довод против: пересказ теряет детали; контролируемого сравнения «холодный возврат против пересказа» на задачах кодинга никто не публиковал.

## Что это меняет у нас (Decision Memo)

**Проблема.** Нужно было понять, верна ли наша картина срока жизни кэша и можно ли нести замер наружу.
**Знали.** Обрыв Claude на 60-й минуте, склон Codex, 25% входа на холодных возвратах.
**ДР добавил.** Документальное объяснение обоих профилей; второй механизм промахов Codex (не простой); устаревшую строку про «24h»; отсутствие публикаций о весе кэша в лимитах подписки; prior art.
**Варианты.** (А) публиковать замер с честными пределами и контролем после 21.10; (Б) ждать ответа OpenAI в #61757; (В) сперва разделить холодные вызовы по причинам.
**Риски.** Малые выборки у границ; один флот; цены API вместо лимитов; «холодный» не равно «истёк».
**Решение.** Вариант А, сделано 20.11.2026: статья EN+RU, подкаст. Пороги хука (300K, пауза 6 ч для Codex) ДР не опроверг, менять их не предлагаю. Вариант В записан идеей в бэклог.

## Не проверено

Ноги Gemini и GLM (не собраны к синтезу); содержание ноги Mistral; веса кэша в лимитах подписки (источники только обратная разработка); «обрыв 5 минут» у Antigravity (один блог); цифры Harvard/FreeInference; корзины Copilot дальше 10 минут.

## Источники первого ряда

- https://developers.openai.com/api/docs/guides/prompt-caching
- https://code.claude.com/docs/en/prompt-caching · https://code.claude.com/docs/en/changelog
- https://platform.claude.com/docs/en/build-with-claude/prompt-caching
- https://x.com/thsottiaux/status/2112712984644784534
- https://github.com/openai/codex/issues/61757 · https://github.com/openai/codex/issues/56050
- https://arxiv.org/abs/2606.30560 · https://arxiv.org/abs/2608.00101 · https://wokey.ai/cache-hit-rate
- https://research.trychroma.com/context-rot
- https://learn.chatgpt.com/docs/pricing

Прибор и таблицы: `projects/cache-ttl-article/` и `projects/session-context-audit/` в AI-Workspace. Статья: prompt-cache-lifetime-codex-vs-claude-code-ru и prompt-cache-lifetime-codex-vs-claude-code-en. Нога GLM собрана после синтеза (гейт ✅, 70 источников), в сравнение не вошла.


## Check it yourself: prompt, sources, raw reports

- Prompt: [DR26-11-20-MACSTUDIOM4-01-0251-srok-zhizni-kesha-prompta-u-codex-claude-code-i-gemini-sintez-dr-i-nas/prompt.md](DR26-11-20-MACSTUDIOM4-01-0251-srok-zhizni-kesha-prompta-u-codex-claude-code-i-gemini-sintez-dr-i-nas/prompt.md)
- All sources (496 links): [DR26-11-20-MACSTUDIOM4-01-0251-srok-zhizni-kesha-prompta-u-codex-claude-code-i-gemini-sintez-dr-i-nas/sources.md](DR26-11-20-MACSTUDIOM4-01-0251-srok-zhizni-kesha-prompta-u-codex-claude-code-i-gemini-sintez-dr-i-nas/sources.md)

## Raw model reports

5 reports delivered, synthesis used 5 of 6. [DR26-11-20-MACSTUDIOM4-01-0251-srok-zhizni-kesha-prompta-u-codex-claude-code-i-gemini-sintez-dr-i-nas/](DR26-11-20-MACSTUDIOM4-01-0251-srok-zhizni-kesha-prompta-u-codex-claude-code-i-gemini-sintez-dr-i-nas/README.md)
