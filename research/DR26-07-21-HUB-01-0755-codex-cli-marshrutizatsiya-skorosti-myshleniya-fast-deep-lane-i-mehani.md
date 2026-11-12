---
dr_id: DR26-07-21-HUB-01-0755
title: "Codex CLI — маршрутизация скорости мышления (fast/deep lane) и механика квот Max-подписки"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# DR26-07-21-HUB-01-0755 — синтез: как гонять Codex быстро и вдумчиво с одной Max-подписки

## Консенсус вендоров (Grok ∩ Gemini, high confidence)
1. **Квота = токены/кредиты, не запросы.** Подписка меряется кредитами (API-эквивалентные токены; reasoning-токены = output) внутри **5-часовых окон** + независимый **жёсткий недельный кап**. «Сообщения в 5ч» — адаптивная витрина: floor≈300 / ceiling≈1800 для sol на Pro 20× (Gemini, [reported]).
2. **xhigh жжёт 3-5× против medium** [reported, оба]; фиксированного официального множителя нет. Дефолт Codex = medium.
3. **Профили — штатный механизм двух полос**: `[profiles.<name>]` в `%USERPROFILE%\.codex\config.toml`, вызов `codex exec --profile <name>`; прецедент CLI `-c` выше профиля; секьюрные ключи в проектном `.codex/` молча игнорируются.
4. **Гочи headless**: stdin читается только с явным `-` (позиционный аргумент молча побеждает пайп); `-c` строки нужно двойно квотить (`'"xhigh"'`); `--json` = JSONL-поток для оркестратора; `codex exec resume --last` восстанавливает упавшую сессию; `--ephemeral` **запрещён для deep-полосы** (убивает resume).
5. **Долгие прогоны падают не от «зависания модели», а от remote compaction timeout** (~300с, Cloudflare idle cap; GitHub issues) → лечится внешним watchdog + resume-loop, не внутренним таймаутом Codex (его нет).

## ⚔️ Ключевое расхождение: unattended cron на подписке
- **Grok**: официальные доки прямо поддерживают `codex exec` для CI/scheduled jobs, есть auth-refresh guidance; риск «низкий для intended use» [established по докам].
- **Gemini**: подписочный OAuth в always-on cron = нарушение «Strictly Personal» ToS; бан-волна OpenCode/OpenHands (нач. 2026), фингерпринт = параллельные headless-сессии/непрерывный поток; для честного crona — API-key (`env_key = "<key-var>"`) [reported + ToS-чтение].
- **Наша трактовка (синтез)**: расхождение о granular-политике, обе стороны сходятся в главном — **human-initiated headless безопасен**. Наш паттерн (t1/t2/t3 из живой сессии, /tt, ретро по команде Антона) = human-in-the-loop → ок в обеих трактовках. ⛔ Чистые ночные кроны на Codex-подписке НЕ заводим; если когда-то захочется — отдельное решение + API-key-лента (Tier-2 spend gate). Совместимо с prefer-included-limits-before-paid-api.

## Правки моей матрицы v1 (что DR поменял)
| Пункт v1 | Было | Стало (по DR) |
|---|---|---|
| Deep-полоса | xhigh | **high** — Gemini [speculative→recommended]: на ревью xhigh даёт «практически идентичное качество», но 30+ мин, компакшн-крэши и 3-5× квоты; Grok: «xhigh only when evals prove benefit». xhigh остаётся только для системных миграций/сложнейших архитектур |
| Fast-полоса | medium (sol) | medium на sol ИЛИ low на **terra** (Gemini: terra 70% vs sol 73% на agentic-бенче при цене ~×0.6) — решает вкус; старт = medium/sol, эскалация вниз по цене после наблюдения за квотой |
| Механизм | `-c model_reasoning_effort=...` | **профили** `fastlane`/`deeplane` (чище, один файл, меньше квотинг-ловушек PowerShell) |
| Таймаут deep | «ждём терпеливо» | **resume-loop**: внешний watchdog ловит exit → `codex exec resume --last --profile deeplane "continue"`; + `model_reasoning_summary` экономит контекст |
| Cron-гейты на Codex | не оговаривал | ⛔ не на подписке; только human-triggered |

## Действия (кому/что)
1. **~/.codex/config.toml**: добавить `[profiles.fastlane]` (medium, summary=none, approval=never) + `[profiles.deeplane]` (high, summary=concise, approval=never) — по шаблону Gemini §7 (оригинал). Владелец: хаб; раскатка пирам с Codex через improvement-rollout.
2. **secondop.py**: t1/t2/t3 → `--profile fastlane` (90с бюджет остаётся); будущий deep-режим → `--profile deeplane --json` + resume-loop + внешний таймаут ~40 мин. Связка с decision-2026-07-20-codex-second-opinion-hub-cron-reorg.
3. **Квота**: перед deep-прогоном проверять `/status` (или dashboard chatgpt.com/codex/settings/usage); недельный кап не тратить фоновым шумом.
4. **Память**: codex-cli-install дополнена ссылкой сюда; правило «Codex = только human-initiated» — след в этой заметке + декстоп-решении.

## Вес ChatGPT-прогона
Deep Research формально завершился (7 мин), но **0 citations · 0 searches** — писал из параметрической памяти, требования брифа нарушены. Текст забран аддендумом в оригинал (см. _originals), в синтезе НЕ использован как источник — только как третий голос «по общим местам» (совпадает с консенсусом, противоречий не внёс). Урок для /dr-fanout: «completed» ≠ «искал» — чек «citations>0» добавить в критерии приёмки сборщика.

> 🧒 Простыми словами: спросили трёх роботов, как правильно пользоваться платным помощником-программистом. Двое хорошо покопались и сошлись: у помощника есть кошелёк, который тратится тем быстрее, чем глубже он думает; думать «на максимум» почти никогда не выгодно — хватает «сильно»; и нельзя оставлять его работать совсем без человека — за это банят. Третий робот схалтурил (ничего не искал) — его ответ мы положили на полку с пометкой «не проверено».
