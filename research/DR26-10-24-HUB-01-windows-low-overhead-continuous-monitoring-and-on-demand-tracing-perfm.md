---
dr_id: DR26-10-24-HUB-01
title: "Windows low-overhead continuous monitoring and on-demand tracing (PerfMon, ETW/WPR, Sysmon"
date: 2026-10-24
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-10-24-HUB-01): Windows low-overhead continuous monitoring and on-demand tracing (PerfMon, ETW/WPR, Sysmon)

> How to monitor Windows CPU, memory, disk and process activity continuously with built-in counters plus on-demand deep traces, without the monitoring itself slowing the system, and how to validate that.

## Ключевые выводы
- PerfMon/PDH counters (Get-Counter, typeperf, logman) are the low-overhead baseline for ongoing metrics, usually a few percent CPU at most. They can miss processes that start and exit between samples and can return PDH_NO_DATA when an instance did not exist at sample time.
- '% Processor Time' is relative to one logical CPU, so a fully busy 28-core system shows about 2800%. Divide by core count to get 0-100%.
- The legacy Process counter object names instances like chrome#3, and these names shift as processes exit and launch. Also collecting 'ID Process' and mapping names to PIDs offline is the workaround. There is no PerfMon 'Process v2' counter object; Process_V2 exists only as an ETW provider.
- PerfMon polling or typeperf cannot catch tasks lasting 10-100 ms. Sysmon (Event ID 1 process create, Event ID 5 process terminate) records every start and exit with command line, parent PID and timestamp, and WPR/ETW kernel tracing also captures them.
- WPR (ETW) is the on-demand deep-diagnosis tool for CPU stacks, DPC/ISR, process create/exit and disk I/O latency. Overhead depends on the profile: light CPU sampling is modest, full tracing can cost multiple percent CPU. Use circular in-memory buffers (e.g. 128 MB) or short captures, and the MemoryLight profile to limit disk I/O.
- Attribution has limits. PerfMon gives disk latency (Avg Disk sec/Read) but not per process, so per-process disk latency needs WPR disk I/O tracing. GPU Engine counters are per engine, so summing 3D and video engines can exceed 100% and there is no simple GPU % by process. User-mode tools cannot see protected processes.
- Elevation: enumerating all counters and instances, logman data collector sets, WPR and Sysmon installation all need admin. Basic CPU and memory counters work without it. Without admin the report recommends PerfMon via Get-Counter, optionally Glances and Process Explorer, and Sysmon only after one-time admin install.
- Process Monitor has extremely high overhead and must not run continuously. Process Explorer is interactive only. Glances is cross-platform but needs Python/psutil, keeps about 1200 in-memory samples per metric and is not a guaranteed history. The report says no official overhead benchmarks exist and assumes a few percent CPU.
- Validation is a before/after test with the same workload, including a loop of fast tasks such as 'ping -n 1 localhost'. Heuristic acceptance: combined monitor overhead below 5-10% CPU and below 50 MB RAM, at least 95% of processes captured, no PDH_NO_DATA beyond transient. Rollback: logman stop, wpr -stop, Sysmon.exe -u, kill Glances/Process Explorer.

## Рекомендации / решения
- Run a low-overhead logman PerfMon collector (1-5 s samples of key CPU, memory, disk, network counters, plus Process\ID Process) permanently with circular logging, and sample no faster than about 1 s.
- Keep WPR profiles ready for on-demand deep dives (CPU, disk I/O, DPC) and start them only when counters show trouble. Use New-MpPerformanceRecording for antivirus scan investigations.
- Deploy Sysmon, filtered to process create and terminate events only, where admin is available, to capture short-lived processes and reliable parent attribution.
- Collect '\Processor(_Total)\% DPC Time' and 'Interrupts/sec' to watch kernel overhead, and trigger a deeper trace when PerfMon queue length exceeds 5 or context switches are extremely high.
- Run the before/after validation: measure the monitors' own CPU and RAM, confirm known short tasks appear in Sysmon or WPR, cross-check PerfMon against Task Manager, and run a 30-60 min stability test checking log sizes.
- Do a disable test: if the slowdown disappears when all monitors are off, one monitor was the culprit, so reduce counters or lengthen intervals.
- Write the rollback checklist (logman stop, wpr -stop, Sysmon -u, kill Glances) before deploying anything.

## Сущности
- **Люди:** —
- **Компании:** Microsoft, Sysinternals, NVIDIA, Windows Defender
- **Продукты/инструменты:** PerfMon, PDH, Get-Counter, typeperf, logman, WPR, WPA, ETW, Sysmon, Process Explorer, Process Monitor, Glances, psutil, Task Manager, New-MpPerformanceRecording, NVML

## Открытые вопросы
- No official overhead benchmarks exist for Glances, and the 'few percent CPU' figures for PerfMon, Sysmon and Glances are assumptions to be measured on the actual machine.
- The 5-10% CPU, 50 MB RAM and 95% process-capture acceptance thresholds are only heuristics and need calibrating on the real 28-core workload.
- How to measure input latency (keystroke to response) is unresolved; the report says it needs targeted ETW tracing or loopback tests.
- There is no reliable per-process GPU % without double-counting; NVML detail on 3D versus compute needs verification.
- How to attribute protected processes (AV, DRM) is unresolved; it would need driver or kernel-level tools.
- The exact Sysmon rule set and WPR profile settings (buffer sizes, frequency) that give acceptable overhead are not specified.

## Источник
- DR-ID `DR26-10-24-HUB-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- Windows performance diagnostics
- ETW kernel tracing
- Process telemetry and short-lived process detection
- Monitoring overhead measurement
- Admin elevation requirements
- Before/after validation testing
- Slowdown and thermal diagnosis (/perf)

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-10-25 через gh: 0 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 0 · 🤷 0.
