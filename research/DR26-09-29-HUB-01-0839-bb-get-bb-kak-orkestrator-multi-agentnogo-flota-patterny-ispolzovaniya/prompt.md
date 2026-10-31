# DR26-09-29-HUB-01-0839 — the exact prompt sent to every model

# Deep Research: как мир использует bb (get-bb, getbb.app) как оркестратор агентов, и какие интерфейсы к нему реально работают

## Контекст заказчика
У нас bb-app 0.41.0 уже работает на Linux-VPS (Hetzner, tailnet) как systemd-сервис, web UI доступен по tailnet на :8443. Латест 0.44.0. Флот: Windows-хаб с 2 GPU, Windows/Mac ноутбуки, Linux-VPS-анкер. Агенты на подписках: Claude Code (основной), Codex, Cursor, Grok CLI, Gemini CLI, GLM, Mistral. 100+ рутин в Windows Task Scheduler, межмашинный синк Syncthing, шина Telegram.

## Вопросы (по убыванию важности)
1. КТО И КАК реально использует bb (github.com/get-bb/bb) в проде/ежедневно: найди живые кейсы (блоги, X/Twitter, HN, Reddit, YouTube, GitHub discussions/issues, плагины сообщества). Что за паттерны: bb как ГЛАВНЫЙ оркестратор всего? Или LLM-агент ПОВЕРХ bb, который сам менеджирует остальные (manager thread, агент спавнит агентов в child threads, workflow plugin)?
2. Как люди подключают к bb НЕСКОЛЬКО разных агентов (Claude Code + Codex + Cursor + Grok + OpenCode + Hermes + ACP-агенты) и как делят между ними работу? Роутинг задач: кто планирует, кто исполняет, кто ревьюит?
3. Интерфейсы: web app, desktop (macOS/Linux AppImage), CLI, HTTP API, iOS app / mobile PWA (TestFlight), bb connect relay (yourhandle.getbb.app, до 20 серверов/20 машин). Что из этого люди реально используют для headless-сервера на VPS? Best practice: tailnet-only vs bb connect relay — плюсы/риски (API без аутентификации!).
4. Automations в bb (cron/one-shot) против внешнего планировщика (systemd timers, Task Scheduler): кто как решает конфликт «два планировщика»? Есть ли кейсы миграции существующего парка cron-рутин внутрь bb?
5. Ограничения и грабли 0.42–0.44: workflow plugin, dispatch queue, concurrency limit, split views, plugin safe mode — что из нового реально про мульти-агентную оркестрацию, что сырое (experimental_)?
6. Альтернативы для сравнения одним абзацем каждая: OpenClaw, Conductor, Vibe Kanban, omp, claude-squad и т.п. — есть ли у кого web-UI-поверх-CLI-агентов лучше, чем у bb, для нашего сценария (VPS + флот + свои подписки)?

## Формат ответа
Конкретика с URL и датами. Кейсы поимённо. Отдельным блоком: рекомендованная целевая схема для нашего флота (что оркестратор, где UI, как подключить 4-6 агентов) + риски.
