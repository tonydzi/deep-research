---
dr_id: DR26-11-01-MACPRO192-01-1537
title: "Мониторинг нагрузки флота под LLM-агентами: синтез DR26-11-01-MACPRO192-01-1537"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Мониторинг нагрузки флота под LLM-агентами (Mac + Windows): синтез ДР

Повод: 01.11.2026 MacPro192 «жёстко тормозит»: load 24 на 32 ядра (пик 47), виноваты два кодирования x265 без приоритета (860% CPU), забытый `evidence.py` из сессии 27.10 (112 CPU-часов) и scp 2,2 ГБ индекса книги чатов через Tailscale (расширение сети 264% CPU). Screenpipe в топе не было. Память не проблема (95% свободно). Связь: hub-infra-ops, perf (скилл), mcp-server-costs-ram-in-every-session, never-archive-sessions-stop-them.

## Где все четыре отчёта сошлись (established)
1. **Атрибуция «какой агент жрёт» на Mac и Windows есть только у Netdata** (apps.plugin + `apps_groups.conf`: обходит дерево процессов, ненайденный наследует группу родителя/ребёнка, считает CPU уже умерших детей). Prometheus `process-exporter` только Linux (читает /proc); Beszel, Glances, btop, osquery, Uptime Kuma дерево не сворачивают.
2. **Beszel = самый лёгкий дашборд флота** (агент 10–25 МБ, хаб 10–50 МБ, SQLite, тревоги в Telegram через Shoutrrr), но **без процессов вовсе** (issue #971 открыт; PR с подсчётом процессов отложен из-за машин с 2000+ процессами, то есть наш хаб).
3. **Prometheus+Grafana, Zabbix, Telegraf+Influx = слишком тяжело для 6 узлов**, Uptime Kuma не про нагрузку вообще.
4. **Сигналы «почему тормозит сейчас»:** Mac: давление памяти (не свободная RAM), рост свопа, `kernel_task` (тепловой регулятор или компрессор памяти), `WindowServer`, `pmset -g therm`/`powermetrics --samplers thermal` (на Apple Silicon `pmset` врёт, верить `powermetrics`), load ≫ ядер. Windows: DPC/ISR латентность (LatencyMon), hard faults/с, % Privileged Time, очередь диска. Прибора задержки ввода на Mac нет ни у кого.
5. **GPU:** Win NVIDIA = nvidia-smi / windows_exporter / HWiNFO; Mac AMD 580X = `ioreg -c IOAccelerator` «Device Utilization %» без root; Apple Silicon = `macmon` без sudo. Тревога «тяжёлое на CPU при спящем GPU» = связка «группа python > N ядер M минут И GPU < 5%».
6. **Корень большинства наших инцидентов не ёмкость, а утечки агентов:** сироты MCP/сессий (claude-code #45507, #1935, #40706, #79567, #82952, #91879; codex #32797, #13928), на Mac они переподчиняются launchd (PPID 1), поэтому жнец по дереву их не видит: **ловить по имени группы и по числу процессов**, тревога «процессов `node`/`claude` > K», и убивать группу процессов, а не прямого ребёнка.
7. Тревоги Telegram «из коробки»: Netdata (health.d + health_alarm_notify.conf), Beszel (Shoutrrr), Grafana. Glances и штатные средства ОС: только скриптом.

## Где разошлись (2 против 2)
- Grok и Gemini: **Beszel первичный** (одна панель флота, 15 минут, дешёвые тревоги) + Netdata локально на тяжёлых узлах без Cloud.
- Claude.ai и Mistral: **Netdata первичный**: агент на каждом узле → свой Parent на Linux-VPS (Маяк) → Telegram; Beszel только как запасной.
- Netdata Cloud: бесплатно 5 узлов, у нас 6; Homelab $90/год только для некоммерческого; Windows-панель то запирали, то отпирали (PR #24327, 27.03.2026). Все четверо: **self-hosted Parent обходит лимит**, Cloud не нужен.
- macOS у Netdata «Community»-тир, Intel-Mac в CI не проверяют (claude.ai): риск для MacPro192, проверяется первой неделей.

## Решение (кофаундер, АК-47: одна система, а не две)
**Netdata parent-child.** Parent на Маяке (Маяк, Debian), агенты на MacPro192 и хабе первыми (канарейка + узел другого типа), потом Mac16 и ZBook; `apps_groups.conf` с группами claude/codex/cursor/chrome/firefox/syncthing/tailscale/screenpipe/python/ffmpeg/node (готовые конфиги в оригиналах Mistral и Grok); тревоги в чат 03: load > 0.7×ядер 5 мин · группа > 4 ядер 10 мин · процессов группы > K · своп растёт · CPU_Speed_Limit < 90. Netdata Cloud не подключаем.
Гейт первой недели: агент на хабе ест > 5% ядра или apps.plugin ломается на Intel-Mac → на этот узел Beszel + наш `perf_snapshot.py` как тревога по группам (10 строк, уже есть половина).
Что НЕ делаем: Prometheus-стек, Zabbix, Influx; Uptime Kuma остаётся только «жив ли Маяк».

## Что уже починено 01.11 (до ДР, та же сессия)
- `fit_upload.py`: ffmpeg под `nice -n 15` (Windows: BELOW_NORMAL) + `pools=ядра/2`; тест 27 проверок PASS; бэкап `.bak-P21597-20261101`.
- Сирота `evidence.py` (PID 2083, 4,5 суток, 112 CPU-ч) остановлен; класс в selfheal `orphan-scratch-loop-burns-cpu-for-days`.
- Жнец сессий не сломан: код 2 = «слепой» при покрытии 76% (17 процессов без `--resume` не сцепить с карточками).

## Открытые вопросы
- Реальный оверхед Netdata на хабе с 2400 процессами и на Intel-Mac с 70 claude: замер, а не вера.
- Кто и зачем тянет 2,2 ГБ `_brain_sessions.npy` с хаба по Tailscale (в скриптах `scp hub:` не найден; 01.11 файл обновлялся в момент замера).
- Поздние ноги: GLM (делает docx 4 агентами), ChatGPT (Deep Research, эффорт Medium): подхватит ночной `dr_collect` или эта сессия.

## Дочитка 02.11 (P21597): 6 из 6 ног + панель проверки
Поздние ноги: **ChatGPT** (Deep Research, 68k символов) = Netdata на каждом узле + свой Parent на Debian-VPS + маленькая «session-shim» (Windows Job Objects; на Mac леджер pid+start_time), `bottom` как локальный запасной, Telegram-тревоги с Parent; **GLM-5.3** (51 стр docx, 135 источников) = Telegraf на всех узлах → Prometheus + Grafana на VPS → Telegram через Grafana; Netdata «runner-up», потому что, по его мнению, на Windows apps.plugin даёт только счётчики. Счёт первичных стеков: **Netdata 3** (claude.ai, mistral, chatgpt) · **Beszel 2** (grok, gemini) · **Telegraf+Prometheus 1** (glm).

Панель (workflow wf_0ed3931b-a52, 12 агентов: 6 читателей, 4 проверяющих по первоисточникам через WebFetch, критик, судья):
1. **Netdata группирует процессы на Windows: TRUE.** В репо есть `apps_os_windows.c` (Perflib «Process»: % User Time, % Privileged Time, Working Set - Private, Creating Process ID), README apps.plugin описывает Windows-правила `apps_groups.conf` (с v2.5.3 по comm/cmdline/name, имена без `.exe`, сервисы svchost по display name). GLM спутал `windows.plugin` System Statistics (только счётчики) с apps.plugin. Живого прогона на Windows никто не видел: гейт первой недели.
2. **Свой Parent обходит лимит 5 узлов: PARTLY (FALSE в главном).** По странице access-control и PR #24327 (слит 15.10.2026) лимит 5 узлов действует и на ЛОКАЛЬНУЮ панель Parent без Cloud; стриминг и сбор не ограничены, тревоги и API не блокируются, лишние узлы показываются как Locked. Windows-панель standalone с 27.03.2026 открыта на бесплатном тарифе. У нас 6 машин: в общей панели видны 5, шестую смотреть на её локальной панели; Homelab $90/год (только некоммерческое) = вопрос Антону (деньги, Tier-2).
3. **Telegraf procstat сворачивает дерево на Mac/Win: PARTLY.** В README `recursion_depth`/`parent_pid`/`child_level` без ограничения ОС (тег «all»), но прямо не сказано; Linux-only помечены отдельно. Не аргумент против Netdata.
4. **Beszel без процессов: TRUE.** Только хост и контейнеры; issue #971 открыт с 16.07.2025, мейнтейнер 20.01.2026: отказался от per-process из-за ~3× нагрузки агента. На Mac ставится `brew tap henrygd/beszel; brew install beszel-agent`.

Критик, чего не поднял ни один отчёт: Parent не умеет тревожить о собственной смерти («stream disconnected» живёт на нём же) → внешний dead-man (наш deadman или одна проверка Uptime Kuma); Маяк уже несёт RAG/LLM, Parent никто не отсайзил (диск под tier-0, RAM); общий аккаунт MacPro192: apps.plugin без setuid не прочтёт cmdline чужих процессов (процессы Артура уйдут в «other»); cmdline с путями репо и кусками промптов уезжают на VPS → Parent только на tailnet-интерфейсе; Kaspersky на хабе может сканировать Perflib-опрос и dbengine; kickstart включает автообновление → на Intel-Mac пинить версию; ноутбуки спят → дыры в репликации и дрейф часов; имена health-контекстов на Windows-детях могут отличаться от Linux.

**Решение после панели (судья, confidence emerging): стоит, с тремя поправками.** (1) Атрибуция на Windows = сам Netdata apps.plugin, второй коллектор не нужен; гейт = живой прогон на хабе в первую неделю (установить MSI standalone, `claude: claude`, `codex: codex`, `node: node` в apps_groups.conf, сутки замера оверхеда и состава групп). (2) Лимит 5 узлов принимаем: 5 предпочтительных узлов в общей панели, шестой на своей локальной, тревоги и API не ограничены; $90/год Homelab отдельным вопросом Антону; два Parent отвергнуты (две системы). (3) Версию пиним, автообновление kickstart на Intel-Mac выключаем; на Parent внешний dead-man; Parent слушает только tailnet. Beszel остаётся запасным только если демон Netdata не проходит гейт на ОБОИХ тяжёлых узлах.
Отвергнуто: Beszel первичным (нет процессов вовсе; цифра 1.3% CPU у Gemini взята из чужого репо siltop, cask назван с ошибкой «bezel»); Telegraf+Prometheus+Grafana (три сервиса на VPS, второй движок тревог, дерево на Mac/Win только подразумевается).
Пробелы, которые ни один отчёт не закрыл: атрибуция по СЕССИИ (все отдают это ненаписанной «прокладке»; идентификатор сессии в окружении детей Claude Code не найден, только `CLAUDECODE=1`); сбор GPU Radeon 580X в сам мониторинг (только `ioreg -c IOAccelerator` руками); история температуры на Apple Silicon без sudo; задержка ввода на Mac; тихая установка на Windows (`msiexec /qn /i`, `stream.conf` на Windows-ребёнке).
