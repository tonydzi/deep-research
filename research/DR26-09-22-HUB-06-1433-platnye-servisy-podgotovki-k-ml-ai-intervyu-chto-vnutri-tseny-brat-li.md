---
dr_id: DR26-09-22-HUB-06-1433
title: "Платные сервисы подготовки к ML/AI-интервью: что внутри, цены, брать ли для research-трека"
date: 2026-09-22
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

> ⚠️ Исправлено 2026-09-29 (dr-judge): первая версия была собрана по мусорной/слабой рельсе и расходилась с живыми отчётами. Переписано по chatgpt.md (56K) и grok.md (51K) из results DR26-09-22-HUB-06-1433. Класс дефекта: synthesis-read-only-the-garbage-rail.

# Insight (DR DR26-09-22-HUB-06-1433): Платные сервисы подготовки к ML/AI-интервью: что внутри, цены, брать ли для research-трека

> Отчёт разбирает по цене и содержанию платные сервисы подготовки к ML/AI-интервью и отвечает, какая доля их программ покрывает research-лупы лабораторий и что бесплатно воспроизводимо.

## Ключевые выводы
- Interview Kickstart, Formation, Pathrise, Educative и основной продукт interviewing.io собраны вокруг кода, system design и поведения software-инженера — под research-луп не адаптированы.
- Hello Interview прямо пишет о своём ML-гайде: «This guide is not useful for AI/ML research interviews» и «This guide is not useful for AI/ML research engineering interviews» — их живые моки и менторство закрыты 31 мая 2026, остался только текстовый гайд ($47/мес, $79/год, $279 навсегда).
- interviewing.io для research-специализации НЕ доказан: FAQ перечисляет Google, Facebook, Amazon, Microsoft, Stripe, Uber, Dropbox, Netflix, LinkedIn, Slack — frontier-лаборатории в списке нет; про frontier labs пишут только в блоге для работодателей. Мок «от $179», цена растёт при запросе интервьюера конкретной компании.
- Rora (teamrora.com) — лучшее найденное соответствие AI-research треку: сайт прямо называет аудиторией «Top AI Researchers», предлагает mock research talks, ML fundamentals, ML system design/architecture и behavioral с подбором интервьюера «in a similar role, level, and company»; отзывы подписаны именами вроде Avi Singh и Avner May с пометкой Google DeepMind/Research Scientist. Фиксированный гонорар подготовки и процент за переговоры на сайте не опубликованы — по договору 2023 года с Reddit это был ретейнер $1,000 + 10% от дельты двухлетнего пакета.
- Interview Kickstart: официальной цены на прайсе нет («узнать на бесплатном вебинаре»). Из жалоб: Trustpilot-отзыв «I paid $6,888.33, with another $2,633.33 still pending» с блокировкой доступа; BBB (Санта-Клара, не аккредитован, рейтинг F, 31 жалоба, 30 без ответа) описывает схему как «a $10k scam / rip-off via Klarna/Affirm».
- Formation: Diagnostic $299 (невозвратный), Monthly $2,500/мес, Fellowship $5,000 сразу + outcome fee, итоговая заявленная стоимость по фейс-странице «$7,500 – $20,000». Продукт нацелен на senior/staff software engineers, dedicated RS-curriculum на открытой странице не виден.
- Dr. Sundeep Teki — единственный открытый прайс, собранный прямо под RS в OpenAI/Anthropic/DeepMind: Гайд $79, Стратегия $499, Sprint $1,799 (3 мока по 60 минут + research talk), Intensive $3,799 (6 недель), Accelerator $6,499 (12 недель). Не действующий член hiring loop лабораторий.
- Levels.fyi Negotiation: Standard $1,250 (гарантия +$10k TC), Premium $2,450 (+$15k), Leadership $5,000 (+$40k) — плоский гонорар с частичным возвратом, если порог не взят; уровень роли не гарантируют.

## Рекомендации / решения
- Полный буткемп (IK/Formation/Pathrise/Educative-курс целиком) не покупать под research-луп — не адаптирован и дорог.
- Главный кандидат на точечную покупку мока под research-специфику — Rora; перед оплатой добиться письменного fixed fee и точного процента переговоров, а не полагаться на «no increase, no fee».
- Dr. Sundeep Teki и/или 2-4 мока у людей, реально проводивших research talk в целевой лаборатории — второй кандидат наравне с Rora.
- interviewing.io и IGotAnOffer ($100–250/час) брать только под прикладной/кодовый раунд, не как замену research-мока.
- Переговоры после оффера почти всегда выгоднее платной подготовки до оффера; Levels.fyi — при одном оффере и малом торге, Rora — при нескольких офферах и нестандартном пакете; обоим одновременно не платить.
- Karat кандидату не покупается — это B2B-скрининг, который оплачивает лаборатория-работодатель, а не соискатель.

## Сущности
- **Люди:** Anton Dziatkovskii, Dr. Sundeep Teki, Avi Singh, Yulia Rubanova, Avner May
- **Компании:** Interview Kickstart, interviewing.io, Exponent, Educative, Tech Interview Handbook, Hello Interview, IGotAnOffer, Karat, Formation, Pathrise, Rora, Levels.fyi, Google, Meta, OpenAI, DeepMind, FAIR, Anthropic
- **Продукты/инструменты:** Grokking the Machine Learning Interview, ML System Design in a Hurry (Hello Interview), Rora Sprint/Intensive/Accelerator, Levels.fyi Negotiation Standard/Premium/Leadership, LeetCode

## Открытые вопросы
- Официальная текущая цена Interview Kickstart в США закрыта за вебинаром — известны только цифры из жалоб Trustpilot/BBB, не прайс-лист.
- Точный процент Rora по текущему договору не опубликован; договор 2023 года с Reddit (ретейнер $1,000 + 10%) не подтверждён как ещё живой.
- Не найдено сервиса, который бы публично называл конкретного действующего research scientist лаборатории интервьюером с именем и числом проведённых лупов.
- Независимая верификация громких цифр компаний (IK «$300,000+/60%+», Formation «over $89k», Rora «$1.2B/$221M») отсутствует — это self-reported маркетинг, не аудированная статистика.

## Источник
- DR-ID `DR26-09-22-HUB-06-1433` · реестр _DR-Registry
- живые отчёты: «внутренний архив лаборатории» (56K), `results/grok.md` (51K)
- оригинал: «внутренний архив лаборатории»

## Связано
- insight-DR-DR26-09-22-HUB-03-1433-интервью-лупы-топ-лабораторий-ии-для-research-scie
