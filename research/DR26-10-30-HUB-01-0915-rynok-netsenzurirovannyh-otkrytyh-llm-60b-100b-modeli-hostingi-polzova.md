---
dr_id: DR26-10-30-HUB-01-0915
title: "Рынок нецензурированных открытых LLM 60B-100B+: модели, хостинги, пользователи, регулятори"
date: 2026-10-30
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-10-30-HUB-01-0915): Рынок нецензурированных открытых LLM 60B-100B+: модели, хостинги, пользователи, регуляторика

> Какие abliterated/uncensored открытые LLM класса 60B-100B+ существуют на 30.10.2026, как их снимают, где взять платный доступ без своей GPU и что с этим по рискам и регуляции.

## Ключевые выводы
- Технического дефицита больших permissive open-weight моделей нет: доступны abliterated Llama 3.3 70B, Qwen2.5 72B, gpt-oss-120b, GLM, DeepSeek, Qwen3-235B, а также Hermes 4 (70B/405B) и Dolphin Mixtral 8x22B (141B MoE). Китайские базы (Qwen, GLM, DeepSeek, Kimi) служат сырьём для самых больших сборок, сами derivative чаще делает западное или международное HF-сообщество.
- Метод: Arditi et al. (arXiv:2406.11717) показали на 13 моделях до 72B, что отказ опосредован одним направлением в активациях; его удаление из весов снимает отказы без переобучения. Поздние работы (EMNLP 2026) уточняют, что отказ не сводится к одной оси. Heretic автоматизирует подбор: на gemma-3-12b отказы 97/100 → 3/100 при KL 0.16 против 0.45 у huihui-ai и 1.04 у mlabonne (замер автора инструмента).
- Потеря качества на больших моделях обычно невелика, но независимого мета-бенчмарка нет. Страдают математика и правдивость: на 16 моделях 7B-14B GSM8K от +1.51 до -18.81 п.п. в зависимости от инструмента; у Heretic в среднем -7.81 п.п.; у карточки DeepSeek V4.1 Flash MMLU 83.77% против 87.63% (-3.86 п.п., цифры авторов); по Cracken AI (24 модели 0.6B-1T) MMLU сохраняется, деградация максимум 0.028.
- Масштаб: Tech Against Terrorism (CBS, 09.10.2026) насчитала более 29 000 репозиториев на HF, рекламирующих uncensored; Lin et al. - более 11 000 моделей; 10a Labs (arXiv:2609.05241) - 3 471 модель, каждая перепакована в среднем 2.4 раза, перераспространение делает удаление с HF малоэффективным. Реальное использование заметно ниже видимости: abliterated Llama 3.3 70B - 20 117 скачиваний в месяц, Dolphin Mixtral 8x22B - 8 901, Hermes 4 405B - 2 011, против 197 874 у стоковой Qwen2.5-72B.
- Скорость появления: DeepSeek V4.1 Flash 1017 вышла 17.10.2026, первая сборка без отказов через ~9 часов, к 28.10 77 репозиториев и ~204 000 скачиваний. HF точечно модерирует: penclaw-GLM-5.3-abliterated-for-offensive-cyber отключена в октябре 2026, Kimi K2.7 Code abliterated исчезла 23.10.2026, тысячи остальных остаются.
- Доступ без GPU: OpenRouter (Hermes 4 70B $0.11/$0.38, 405B $1/$3 за 1M, режим ZDR); Venice.ai (своя uncensored-линия 24B, Venice Uncensored 1.2 $0.20/$0.90, заявлено отсутствие хранения, есть TEE/E2EE); Featherless (Premium $25/мес любой размер, Llama-3.3-70B abliterated $2.88/$2.88); Infermatic (от $9/мес); NanoGPT; аренда GPU (RunPod, Lambda, Vast.ai, H100 примерно $1.73-3.99/ч). Together и Fireworks дают в основном стоковые модели. Самый предсказуемый путь без провайдерских отказов - свои веса на арендованной GPU.
- Слово uncensored не означает отсутствия ограничений на уровне сервиса: у всех провайдеров есть ToS/AUP (незаконный контент, CSAM, мошенничество), право отозвать ключ или аккаунт; OpenRouter запрещает unapproved red-teaming/jailbreaking; у Llama 3.3 AUP запрещает снимать защиты, лицензия наследуется производными. Заявления о no-log у Venice, Featherless, Infermatic - собственные декларации без внешнего аудита.
- Пользователи и риск: по OpenRouter/a16z (arXiv:2601.10088) около 52% токенов открытых моделей уходит на ролевые игры и сторителлинг; задокументированы WormGPT-клоны на Mixtral и Grok, 25% из 1 643 GitHub-приложений с uncensored-моделями признаны explicitly malicious (10a Labs). Опубликованные оценки: реальный uplift в катастрофических доменах пока ограничен; UK AISI отмечает, что после публикации весов защиты снимаются необратимо.
- Регуляция почти не затрагивает конечного пользователя: EU AI Act освобождает открытые GPAI от части документационных обязанностей, но не модели с системным риском (>10^25 FLOP); в США CAISI/NIST оценивает китайские модели без запрета. FT с Alice (май 2026): защиты Llama 3.3 снимаются за минуты, Gemma 4 - за 90 минут после релиза.

## Рекомендации / решения
- Для журналиста-исследователя брать нейтральный тюн (Hermes 4 70B/405B через OpenRouter с zdr:true) или сильную стоковую открытую модель у провайдера с нулевым хранением, а не максимально вседозволенную сборку.
- Если нужны гарантированно предсказуемые ответы без провайдерского слоя - арендовать GPU (RunPod, Lambda, Vast.ai) и запускать выбранные веса самому; 70B в 4 битах часто влезает на одну карту 80 ГБ.
- Для широкого перебора abliterated-сборок использовать Featherless Premium ($25/мес), для компактного перебора ролевых тюнов - Infermatic; перед оплатой сверять цены и ToS на страницах самих хостеров, так как сводки противоречат друг другу.
- Перед публикацией читать ToS/AUP конкретного сервиса (у OpenRouter запрет unapproved red-teaming) и цепочку лицензий производной модели; не путать сервисы WormGPT/FraudGPT с исследовательским хостингом.
- Цифры об уровне деградации качества и размере рынка подавать как наблюдаемые прокси с указанием источника, а не как измеренный размер рынка; различать политическую цензуру КНР (R1 1776) и общие safety-отказы.

## Сущности
- **Люди:** Andy Arditi, Eric Hartford, Maxime Labonne (mlabonne), Philipp Emanuel Weidmann, FailSpy, Erik Voorhees, Jesse Proudman, Sao10K, sophosympatheia
- **Компании:** Nous Research, Cognitive Computations / dphn, huihui-ai, Meta, Mistral AI, OpenAI, Alibaba Qwen, Z.ai / Zhipu, DeepSeek, Moonshot AI, Cohere, OpenRouter, Venice.ai, Featherless, Infermatic, NanoGPT, Together AI, Fireworks AI, Hugging Face, RunPod, Lambda, Vast.ai, Perplexity, UK AI Security Institute, Tech Against Terrorism, 10a Labs, Recorded Future, Cracken AI, RAND, Financial Times, Alice
- **Продукты/инструменты:** Hermes 4 70B/405B, Llama-3.3-70B-Instruct-abliterated, Dolphin-2.9.2-Mixtral-8x22B, gpt-oss-120b abliterated, Qwen2.5-72B abliterated, Qwen3-235B abliterated, GLM-4.5-Air / GLM-5.x abliterated, DeepSeek V4.1 Flash uncensored, Perplexity R1 1776, Midnight-Miqu-70B, Euryale, Heretic, DECCP, ErisForge, abliteration, RefusalBench, Chutes, Ollama, Venice Uncensored 1.2

## Открытые вопросы
- Нет публичного аудита размера рынка в деньгах или уникальных пользователях; HF downloads не равны пользователям, данные OpenRouter отражают только его трафик.
- Нет независимого мета-бенчмарка потери качества при abliteration моделей 70B-500B; опубликованные цифры - в основном заявления авторов или замеры на 7B-14B.
- Заявления о no-log у Venice, Featherless, Infermatic не подтверждены внешним аудитом.
- Актуальные цены и ToS ряда хостеров (Together, Fireworks, Groq, RunPod, Venice) не открывались первоисточником; сводки противоречат друг другу.
- Не подтверждены надёжные крупные uncensored-деривативы Command R+ и Kimi K2 на дату среза; данные о Llama 4 Scout abliterated только из одного источника.
- Причина исчезновения Kimi K2.7 Code abliterated с HF 23.10.2026 не установлена.
- Реальная доля uncensored-моделей в inference-трафике и характер основной массы пользователей по независимым опросам не измерены.
- Юрисдикции и условия многих узких хостеров (Infermatic, NanoGPT, децентрализованные сети вроде Chutes/Akash/Bittensor) не установлены.

## Источник
- DR-ID `DR26-10-30-HUB-01-0915` · реестр _DR-Registry
- оригинал: «внутренний архив лаборатории»
- оригинал: «внутренний архив лаборатории»
- оригинал: «внутренний архив лаборатории»
- оригинал: «внутренний архив лаборатории»

## Связано
- abliteration / directional ablation
- refusal direction
- open-weight safety limits
- composable alignment
- zero data retention
- EU AI Act GPAI
- AISI оценки открытых моделей
- redistribution as persistence layer

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-11-01 через gh: 0 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 0 · 🤷 0.
