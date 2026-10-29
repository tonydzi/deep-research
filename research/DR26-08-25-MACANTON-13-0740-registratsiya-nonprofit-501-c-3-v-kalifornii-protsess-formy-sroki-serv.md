---
dr_id: DR26-08-25-MACANTON-13-0740
title: "Регистрация nonprofit 501(c)(3) в Калифорнии: процесс, формы, сроки, сервисы"
date: 2026-08-25
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-08-25-MACANTON-13-0740): Регистрация nonprofit 501(c)(3) в Калифорнии: процесс, формы, сроки, сервисы

> DR отвечает, есть ли аналог Stripe Atlas для регистрации non-profit в США, и вместо единого сервиса даёт полную практическую карту California + IRS процесса и сравнение специализированных филинг-сервисов.

## Ключевые выводы
- Прямого аналога Stripe Atlas для non-profit нет; процесс распадается на 4 независимых органа: CA Secretary of State (юрлицо), IRS (федеральный 501(c)(3)), California Attorney General (регистрация charity), California FTB (освобождение от налога штата) — каждый шаг нужно проходить отдельно
- Минимальные gov fees: ~$375 при праве на Form 1023-EZ ($30 Articles + $20 SI-100 + $50 CT-1 + $275 IRS) или ~$700 при full Form 1023 ($600 IRS); EIN и FTB 3500/3500A бесплатны
- 1023-EZ нельзя выбирать только по обороту <$50k — нужно пройти IRS Eligibility Worksheet целиком (доп. structural disqualifiers), assets не более $250,000
- CT-1 (регистрация у CA Attorney General) подаётся в течение 30 дней с момента ПЕРВОГО получения charitable assets, а не после IRS letter — частая ошибка ждать determination letter
- Федеральное 501(c)(3) НЕ даёт автоматом California tax exemption — нужна отдельная подача FTB 3500A (после IRS letter) или FTB 3500 (до/без letter)
- IRS benchmark обработки: ~80% 1023-EZ determinations за 22 дня (сложные случаи — до ~120 дней), 80% full Form 1023 — за 191 день; FTB ~4 месяца для 3500A, ~6 месяцев для 3500
- Из специализированных сервисов наиболее полный по подтверждённому scope — Foundation Group SureSTART (full, quote-price; Express — $1,499+fees, но БЕЗ доп. compliance filings, только для простых 1023-EZ кейсов); LegalZoom дешевле ($99 formation + $595 за 501(c)(3), итого ~$694) но не гарантирует включение CT-1/FTB 3500A; Northwest ($39+fees) и Bizee ($0+fees) закрывают только state formation, IRS/CA-compliance — отдельно
- Постоянный compliance после получения статуса: 3 параллельных годовых/двухгодичных трека — IRS (990-N/EZ/990 по порогам revenue/assets), FTB (199N/199/109), CA AG (RRF-1 $25-$1200 по revenue + CT-TR-1), плюс SOS SI-100 раз в 2 года; 3 года без required IRS filing = automatic revocation
- Организация выше $2 млн relevant revenue обязана проходить annual independent audit; unrelated business income >$1,000 требует Form 109

## Рекомендации / решения
- Для типичной charity выбрать структуру California Nonprofit Public Benefit Corporation и подавать именно форму ARTS-PB-501(c)(3) (не generic Articles), чтобы сразу удовлетворить IRS organizational test
- Не оформлять EIN заранее — только после юридического образования корпорации (IRS предупреждает против preemptive EIN)
- Строить независимый board минимум из 3 человек ради снижения риска private benefit/conflict вопросов при IRS review, даже если корпоративно достаточно меньшего числа
- Перед оплатой любого filing-сервиса присылать письменный чек-лист из 18 пунктов (что именно включено: CT-1, FTB 3500A, ежегодные 990/199/RRF-1 и т.д.) — публичная цена не гарантирует California-specific completeness
- Ranking для 'максимально под ключ': Foundation Group full SureSTART → Harbor/Labyrinth+Foundation Group (для тяжёлого multi-state compliance) → LegalZoom с отдельным контролем CA-шагов → Northwest/Bizee только если готовы сами вести IRS/CA charity filings
- Подавать IRS application не позднее 27 месяцев с момента formation, чтобы recognition ретроактивно распространилось на дату основания
- Не запускать публичный fundraising пока AG registration не в статусе good standing; для raffles и commercial fundraisers использовать отдельные CA AG режимы регистрации

## Сущности
- **Люди:** —
- **Компании:** Stripe Atlas, Foundation Group, Harbor/Labyrinth, LegalZoom, Northwest Registered Agent, Bizee
- **Продукты/инструменты:** Form 1023, Form 1023-EZ, Form CT-1, Form SI-100, FTB Form 3500, FTB Form 3500A, Form 990/990-EZ/990-N, Form 199/199N, Form RRF-1, CT-TR-1, Form 109, California bizfile Online, Pay.gov, MyFTB, SureSTART / SureSTART Express

## Открытые вопросы
- Действительно ли Foundation Group full SureSTART включает CT-1 и FTB 3500A в письменном scope у конкретного продавца-контрактора (требует прямого запроса)
- Доступность и надёжность bilingual/русскоязычной поддержки у всех 4 сравненных сервисов не подтверждена официально
- Влияние перехода California AG Registry на новую Online Filing Service (2026, поэтапный rollout) на реальные сроки CT-1/RRF-1 для данной организации
- Временное продление дедлайна AG до 31 августа 2026 — актуально ли оно всё ещё к моменту фактической регистрации
- Классификация public charity vs private foundation зависит от структуры финансирования — не решена для конкретного случая Антона (кто фондирует организацию)

## Источник
- DR-ID `DR26-08-25-MACANTON-13-0740` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- Stripe Atlas
- 501(c)(3)
- California nonprofit compliance
- IRS Form 1023
- деньги/юр.регистрация США
- founder governance non-profit
- insight-DR-DR26-08-29-MACANTON-15-0743-регистрация-nonprofit-501-c-3-в-калифорнии-сервисы — тот же тред DR по регистрации 501(c)(3) в Калифорнии, более ранняя версия
