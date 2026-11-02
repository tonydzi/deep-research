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
