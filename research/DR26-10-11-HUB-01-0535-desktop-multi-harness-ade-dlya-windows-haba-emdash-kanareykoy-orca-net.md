---
dr_id: DR26-10-11-HUB-01-0535
title: "desktop multi-harness ADE для Windows-хаба: Emdash канарейкой, Orca нет, bb не трогаем"
date: 2026-10-11
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

Machine: хаб (Хаб) · Agent: Claude Code (Fable 5.1) · Operator: Anton · 2026-10-11

# Insight (DR DR26-10-11-HUB-01-0535): одно окно для многих harness на Windows-хабе

> Заказ: голосовая Антона 10.10.2026 «Изучить. Провести ДР» по отчёту ChatGPT `deep-research-report (3).md` (рейтинг Orca → Superset → Emdash → ccgui → bb → OpenChamber → Proliferate, CC Switch компаньон). Решение, которое кормит отчёт: менять ли пульт bb (выбран 03.10, bb-scout-and-harness-recall-2026-10-03) на Orca или Emdash одной обратимой канарейкой, или no-go. Промпт написал Codex 10.10 (adversarial: все имена, звёзды, лицензии и Windows-фит считать непроверенными), сессия Codex оборвалась, веер довёл хаб 11.10. Поверх четырёх ног: проверка канонических репо по рельсе web (`queue/DR26-10-11-HUB-01-0535/verify-web-claude-2026-10-11.md`).

## Вердикт

**Канарейка: Emdash (generalaction/emdash) portable `.exe` в одноразовой папке. Запасной: Orca только с Agent Permissions = Manual и без account switcher. bb остаётся как есть. ccgui, CC Switch-прокси, Superset, Proliferate: no-go.** Сам рейтинг исходного отчёта на Windows-хабе не выживает: второе место без Windows-сборки, четвёртое с телеметрией в Baidu и без LICENSE.

## На чём вендоры сошлись (4/4 плюс web-проверка)

| Факт | Кто подтвердил | Первоисточник |
|---|---|---|
| Категория реальна, все 8 продуктов существуют; «ADE» = термин самих вендоров (Orca, Emdash, ZCode, OpenChamber) | claude, gemini, mistral; glm частично | README репозиториев |
| Superset: «Windows: not yet available», Elastic 2.0 | все 4 + web | README superset-sh/superset |
| OpenChamber = один harness OpenCode, Claude через плагин Agent SDK | claude, mistral, web; gemini «ambiguous» | README openchamber/openchamber |
| Orca: MIT, канонический репо stablyai/orca, Windows-инсталлятор с подписью SignPath, 89k звёзд | claude, mistral, web; gemini дала 4.1k звёзд (ошибка) | README, GitHub API |
| Orca по умолчанию ставит агентам флаги обхода разрешений (`--dangerously-skip-permissions`, `--dangerously-bypass-approvals-and-sandbox`, `--mode yolo`), worktree не является песочницей процесса | mistral, web, Codex 10.10 | onorca.dev/docs/agents, tui-agent-permissions.ts |
| Emdash: Apache-2.0, MSI + portable exe, local-first SQLite, `TELEMETRY_ENABLED=false`, хуки с маркером в user-level конфиг агентов | claude, gemini, mistral, web; glm не нашёл | README generalaction/emdash |
| CC Switch = роутер конфигов и провайдеров, не воркспейс; OAuth-прокси «may violate OpenAI ToS» по их же FAQ | claude, mistral, web | FAQ farion1231/cc-switch |
| bb: Windows native alpha «expect problems», рекомендуют WSL2 | claude, gemini, mistral, web | README get-bb/bb |
| Все first-party harness существуют: Grok Build (Apache-2.0 после июля 2026), DeepSeek Harness dsh (MIT, developer preview), ZCode (репо 20.09.2026, 3 недели), Antigravity, Kimi Code, Qwen Code, Mistral Vibe | claude, gemini, mistral | репо вендоров |

## Расхождения с критерием каждого вендора (не усредняю)

| Вендор | Канарейка | Запасной | Критерий, по которому судил |
|---|---|---|---|
| claude.ai | **Orca** | Emdash | ширина Windows + remote + mobile, подписанный инсталлятор; сам же перечисляет red flags Orca (перезапись «внутренний путь лаборатории» #9582/#13165, pairing-токены #17397-17399) и требует изолировать креды |
| Gemini a@ | **Emdash** | Orca | безопасность subprocess: Orca на Windows гоняет `powershell -ExecutionPolicy Bypass -EncodedCommand`, ловится AhnLab/Kaspersky как PDM:Exploit (issue #14233); Emdash использует execFile |
| Mistral | **Emdash** 4.35 | Orca 4.15 | дефолты безопасности (yolo-флаги Orca) + обратимость portable exe без установки |
| GLM | **Orca** | CC Switch | популярность + Windows; но Emdash, bb, ccgui, OpenChamber пометил NOT VERIFIED, то есть альтернативу не видел; CC Switch как «fallback» = ошибка категории |

Счёт 2:2 по голосам. Поправка после панели ломателей 14.10: взвешивать голоса «по уликам» в 3:1 было нечестно, потому что Gemini (за Emdash) опровергнута внутри веера дважды (звёзды Orca, природа ccgui), а GLM (за Orca) не проверил Emdash вовсе; симметричная скидка даёт 1:1. Вердикт держится не на счёте голосов, а на фактах, подтверждённых независимо web-проверкой и Codex: yolo-дефолт Orca, перезапись credentials (#9582, #13165), AV на PowerShell-обёртке (#14233), открытые Windows-крэши; ни один из них ни одна нога не опровергла. Критерий, который разделяет лагеря, один: что важнее, ширина каталога или безопасность дефолтов на машине с секретами. Для хаба хаб (secrets\, 232 скилла, хуки в settings.json, логины уже горели 02.10) ответ однозначный.

## Прямые опровержения внутри веера

1. Gemini: «Orca ~4.1k звёзд» и «ccgui = плагин JetBrains/VS Code». Опровергнуто Mistral (GitHub API: 89 285) и web-проверкой (desktop-cc-gui = Tauri, NSIS `CC.GUI_1.1.3_x64-setup.exe`). Gemini смотрела на `vscode-cc-gui` того же автора.
2. GLM: «CC Switch 67.2K звёзд», «у Orca телеметрии не найдено». Опровергнуто: 142 533 звезды (API), телеметрия документирована с opt-out `ORCA_TELEMETRY_DISABLED=1` (claude, mistral).
3. Mistral: «в последнем релизе Orca только macOS/Linux ассеты». Опровергнуто web-проверкой: `orca-windows-setup.exe` есть, подпись SignPath. Mistral сама пометила это как single-source conflict.
4. Исходный отчёт ChatGPT: «ccgui 8.5, трест под вопросом». Уточнено: Baidu Tongji до сих пор в коде (`src/lib/analytics.ts`, `src-tauri/src/baidu_tongji.rs`), файла LICENSE в корне нет.

## Уникальное у каждого

- **claude.ai**: единственная нога с научными уликами про «harness важнее модели»: Vats & Golev «Scaffold Effect» (arXiv 2607.22585): 3 harness × 2 модели × 50 задач Terminal-Bench Pro, разрыв до 40× в токенах на решённую задачу при разнице pass-rate 0–8 п.п.; Tessl position paper ICML 2026 (Opus 4.6: 79.8% под ForgeCode против 58.0% под Claude Code на TerminalBench). Вывод для нас: держать Claude в Claude Code, GPT в Codex подтверждается экономикой токенов, не точностью.
- **Gemini**: инцидент Grok Build июль 2026: CLI молча выгружал целые git-репозитории с `.env` и SSH-ключами в GCS, переключатель приватности не помогал; xAI открыл исходники (~840k строк Rust) после скандала. Плюс Antigravity: Strict Mode sandbox «not supported on Windows».
- **Mistral**: брендовые коллизии как класс риска (Emdash CMS, Orca MSI editor, Apache Superset, @jun-a/ccgui); ZCode слишком молод для канарейки; самый полный 60-минутный план с portable exe без инсталлятора.
- **GLM**: ничего уникального, слабейшая нога: 71 URL, из них половина домены общего назначения.

## Что это меняет у нас

1. Пульт bb в WSL2 (решение 03.10) не трогаем: ни одна нога не нашла продукт, который на Windows даёт больше при меньшем риске прямо сейчас.
2. Канарейка Emdash: отдельная задача task-2026-10-11-emdash-canary-windows-hub, 60 минут, portable exe, одноразовый репо, бэкап и diff «внутренний путь лаборатории» (Emdash пишет туда хуки с маркером). Решение decision-2026-10-11-ade-canary-emdash-not-orca.
3. Grok Build у нас стоит как CLI-рельса (`grok --always-approve`, dr_rails.json). Инцидент выгрузки репо июль 2026 требует проверить версию и егресс нашей установки. Отдельная строка в задаче.
4. CC Switch не ставить: наш `llm_routing.json` уже делает маршрутизацию, а OAuth-прокси CC Switch режет условия OpenAI.

## Не проверено

Hands-on ни одного продукта (ни одна нога не ставила софт). Стабильность Emdash на Windows известна только из changelog вендора. Открытые Windows-крэши Orca (#16759, #19891, #16797, #25270) не проверены на текущей версии 1.4.224. ChatGPT и Grok в веере не участвовали.

рельса: ноги claudeai (bb@, Fable 5.1 Max + Research), gemini a@ (Deep Research), glm (Deep Think Max + Advanced Search), mistral (Deep research) через Chrome; web-проверка `llm_ask --class web` (gemini мимо, ответил claude); синтез Fable 5.1 на хабе.


## Raw model reports

4 reports delivered, synthesis used 4 of 6: [DR26-10-11-HUB-01-0535-desktop-multi-harness-ade-dlya-windows-haba-emdash-kanareykoy-orca-net/](DR26-10-11-HUB-01-0535-desktop-multi-harness-ade-dlya-windows-haba-emdash-kanareykoy-orca-net/README.md)
