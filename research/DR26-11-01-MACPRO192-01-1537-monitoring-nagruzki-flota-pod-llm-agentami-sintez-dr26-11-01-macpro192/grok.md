# DR26-11-01-MACPRO192-01-1537 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: grok.com (Expert + skill deep-research), chat https://приватный чат
topic: DR26-11-01-MACPRO192-01-1537 Load monitoring for a fleet of developer machines running many LLM coding agents (macOS + Windows)
date: 2026-11-01

proof: "Worked for 54s", "129 sources" shown in UI; Expert mode, model not displayed
---
# Load Monitoring for a Fleet of Developer Machines Running Many LLM Coding Agents — Deep Research Report

Research questions: What is the simplest robust ("repairable with a hammer and an axe") way to continuously see and alert on resource load across a mixed macOS (Intel + Apple Silicon) + Windows 11 fleet of developer machines that host many concurrent LLM coding agents (Claude Code/Desktop, Codex, Cursor, etc.), with attribution to which agent/session/background job is responsible, ideally via one shared dashboard?

Scope / Date: November 2026; prioritizes 2025–2026 sources.

Overall confidence: Medium-High on tool landscapes and measured overheads; Medium on exact process-tree attribution reliability under heavy Electron/MCP loads; Lower on long-term production overhead numbers specific to 70+ Claude CLI processes.

## Executive Summary
- Beszel is the closest match to "simplest robust" for a small fleet: hub + tiny agents (~10–15 MB RSS, near-zero CPU), native multi-host dashboard, history, threshold alerts (Telegram/webhook via Shoutrrr), GPU/temp/load/CPU/RAM/disk/network, Docker/systemd views. It lacks deep native process-tree attribution. [established] Beszel
- Netdata (agent + optional Parent or Cloud) is the strongest for process-group attribution (apps.plugin + apps_groups.conf automatically rolls up trees; works on macOS/Windows/Linux) and 1-second metrics + built-in alerts. Overhead is higher (~1–5% of one core, 100–200 MB RAM; lower when ML/alerts offloaded). [established] Netdata
- Process-tree attribution is imperfect on macOS/Windows (no cgroups). Netdata apps.plugin and Prometheus process-exporter (-children) do the best native roll-up; macOS "responsible PID" (launchctl procinfo / Activity Monitor hierarchy) and Windows Job Objects are the underlying OS mechanisms. [established]
- Interactive lag is best predicted by load vs. core count, memory pressure (not free RAM), WindowServer/kernel_task on macOS, and DPC/ISR + privileged time + hard faults on Windows. Direct input latency tools exist mainly on Windows (LatencyMon). [established]
- Recommendation: Beszel as the primary fleet stack (one dashboard + cheap alerts) + Netdata agents (or local Stats/btop/Process Explorer) on the heavy machines for process attribution when something is lagging. Confidence: Medium. Main assumption: process-level grouping is needed only for diagnosis, not continuous high-cardinality alerting.

## Detailed Findings
### (1) Landscape of Candidate Tools
Tool | OS Support | Install | Own Overhead (sources) | Process / Tree Views | History | Multi-host Dashboard | Alerting | License / Price
Netdata | Linux, macOS (Intel/arm64), Windows, FreeBSD | kickstart script, brew install netdata, Windows installer / winget (in progress) | <5% one core, ~150–200 MB RAM (standalone, ML on); ~1–2% / 100–150 MB when streaming to Parent and ML/alerts offloaded. University ICSOC 2023 study called it most energy-efficient. [established] Netdata | Excellent: apps.plugin groups by name/cmdline/tree; apps_groups.conf; automatic .app grouping on macOS; service grouping on Windows. Per-process live view. | Tiered local (1s → years); Parent for longer | Parent-child or Netdata Cloud (free tier limited) | Built-in, Telegram, webhook, email, Slack, etc. | Agent GPL-3; UI free with agent/Cloud (NCUL)
Beszel | Linux, FreeBSD, macOS (brew), Windows (winget/Scoop) | Hub Docker or binary; agent curl script / brew / winget | Agent ~10–15 MB RSS, near-0% CPU; Hub ~11–45 MB. Measured 2026. [established] Beszel | System + Docker/Podman + systemd services + GPU. No native deep process-tree views (feature request exists). | Yes (up to ~30 days typical) | Yes (single hub dashboard) | Yes, many channels incl. Telegram via Shoutrrr | MIT, free
Glances | Linux, macOS, Windows, FreeBSD | pip / brew / packages | Low–moderate (one process); export mode adds some CPU | Process list; export of selected processes possible (recent) | In-memory short; InfluxDB/CSV export for longer | Via InfluxDB + Grafana or multi-client | Limited (via export + external) | LGPL-3
btop / htop / bottom | Linux, macOS, Windows (btop) | brew / packages / single binary | Negligible when open | Process + tree | None (live only) | No | No | Various open
Prometheus node_exporter + windows_exporter + process-exporter + Grafana | All (process-exporter Linux-native; macOS community exporter exists) | Packages / binaries | Low per exporter; total depends on scrape interval & cardinality | process-exporter groups + -children tree roll-up; windows_exporter process collector | Configurable retention | Yes (central Prometheus/Grafana) | Alertmanager (Telegram/webhook/email) | Apache-2
Telegraf + InfluxDB | All | Packages / binaries | Moderate | Process input available | Configurable | Yes | Kapacitor / external | MIT / Influx
Zabbix agent | All | Packages | Higher (5–10% / 1 GB+ typical comparisons) | Yes | Yes | Yes | Yes | Open / enterprise
osquery | All | Packages | Variable (query-driven) | SQL over processes | Scheduled queries | Yes (Fleet/Kolide) | Via external | Apache-2
Uptime Kuma | Any (external checks) | Docker / binary | Low for checks | None (external HTTP/ping/TCP) | Yes for checks | Yes | Yes | MIT, free
macOS built-ins | macOS | Built-in | Zero | Activity Monitor (responsible-PID hierarchy), top, ps | Limited (log stream, power logs) | No | No | —
Windows built-ins | Windows | Built-in | Zero | Task Manager, Resource Monitor, Process Explorer, PerfMon, WPR | Data Collector Sets | Limited | PerfMon alerts | —
macOS menu-bar (Stats, iStat Menus, MenuMeters) | macOS Intel + arm64 | brew install stats; iStat paid | Very low | Live process lists in some | Short graphs | No | Threshold notifications (Stats/iStat) | Stats MIT free; iStat ~$12

Why Uptime Kuma is not enough: It is an external reachability checker. An HTTP 200 or ping success says nothing about CPU load, memory pressure, a single process at 800% CPU, swap growth, or thermal throttling. It cannot attribute load to an agent session. [established] Alertkick

### (2) Attribution Problem — Process Trees
cgroups do not exist on macOS or Windows.
- macOS: Most processes are children of launchd (PID 1). The useful parent is the "responsible PID" exposed by sudo launchctl procinfo <pid> and used by Activity Monitor's hierarchical view. Private API responsibility_get_pid_responsible_for_pid underlies it. [established] Themittenmac
- Windows: Job Objects group processes and accumulate CPU/memory/IO for the tree (including exited children). Process Explorer colors jobs; WMI classes expose accounting. Nested jobs exist since Windows 8. [established] Learn.microsoft

Tools that do native roll-up:
- Netdata apps.plugin: matches name/cmdline, then includes the whole tree; unmatched processes inherit from parent; automatic .app / framework grouping on macOS; service grouping on Windows. [established] GitHub
- Prometheus process-exporter (-children=true default): subprocesses roll up to the first matching group when walking the tree. [established] GitHub

Concrete Netdata apps_groups.conf fragment (first match wins; trees included automatically):
```
# Agents & Electron apps
claude: claude *claude*
codex: codex *codex*
cursor: cursor *cursor* "Cursor Helper*"
electron-agents: "Claude" "ChatGPT" "Codex"
# Browsers / helpers
chrome: chrome "Google Chrome*" chromium
firefox: firefox
# Sync / network / audio
syncthing: syncthing
tailscale: tailscale
screenpipe: screenpipe *whisper*
# Background jobs
python: python python3
ffmpeg: ffmpeg
node: node
# Catch-all interpreters if needed
interpreters: python python3 node bash
```
process-exporter YAML equivalent (Linux; use community macos-process-exporter on macOS):
```yaml
process_names:
- name: claude
  cmdline: ['.*claude.*']
- name: codex
  cmdline: ['.*codex.*']
- name: cursor
  cmdline: ['.*[Cc]ursor.*']
- name: chrome
  comm: ['chrome', 'chromium']
- name: syncthing
  comm: ['syncthing']
- name: tailscale
  comm: ['tailscale']
- name: screenpipe
  cmdline: ['.*screenpipe.*', '.*whisper.*']
- name: python
  comm: ['python', 'python3']
- name: ffmpeg
  comm: ['ffmpeg']
- name: node
  comm: ['node']
```
Run with -children (default).

### (3) "Why is it lagging RIGHT NOW" Signals
macOS (best predictors): Load average ≫ logical cores (your measured 24 on 32 cores). kernel_task high % (often thermal/CPU speed limiting). WindowServer % high. Memory pressure (not free RAM) via memory_pressure or Activity Monitor. Thermal pressure (sudo powermetrics --samplers thermal -n 1 or pmset -g therm). Disk I/O wait.
Windows: High % Processor Time + high % Privileged Time. DPC/ISR latency (LatencyMon / LatencyTest / xperf). Hard faults/s and disk queue length. UI thread stalls (WPR / Performance Monitor).
Direct input/UI latency tools: LatencyMon (Windows, free, measures interrupt-to-process, DPC, ISR, hard pagefaults) is the standard. No equally simple always-on equivalent is widely used on macOS. [established] Latencymon

### (4) GPU Monitoring
- Windows NVIDIA: nvidia-smi (live), nvidia_gpu_exporter or windows_exporter GPU collector (Prometheus), HWiNFO, Task Manager. Detects "CPU-only torch while GPU idle". [established]
- macOS AMD / Apple: sudo powermetrics --samplers gpu_power, ioreg Device Utilization, Stats / asitop / macmon. [established] Tendmac

### (5) Thermal Throttling with History
- macOS: powermetrics --samplers thermal, pmset -g therm, Stats/iStat Menus sensors. History via continuous logger or Netdata/Beszel temperature charts.
- Windows: PerfMon thermal counters, HWiNFO logs, LatencyMon correlation.
- Netdata and Beszel both record temperature history out of the box where sensors are exposed.

### (6) Alerting Designs That Are Cheap
Threshold examples that map to your incidents: load > cores × 0.7 for 5 min; swap growth; single process/group > N cores for M minutes; process count of a given name > K.
Out-of-the-box support: Netdata: yes (hundreds of built-in + custom, Telegram/webhook). Beszel: yes (threshold alerts on the metrics it collects, Telegram via Shoutrrr). Prometheus + Alertmanager: yes. Glances / menu-bar tools: limited or none.

### (7) Recommendation — One Primary Stack + Lightweight Fallbacks
Primary stack: Beszel. Why: lowest overhead, true single dashboard across the fleet, alerts that are cheap to set up, covers the "is the machine hot/loaded right now?" question, GPU/temp/load, and is easy to repair.
Install (approximate): Hub (on one always-on machine, e.g. the Linux VPS or Windows hub): Docker or binary from get.beszel.dev. Agents: brew install beszel-agent (macOS) or the winget/PowerShell script (Windows). Setup time: 30–90 minutes for the fleet. Overhead: ~10–15 MB RAM / near-zero CPU per agent. Main risk: limited process-tree attribution (you will still need to SSH or open a local tool when an alert fires). First week: alert on load > 0.7×cores and memory pressure; correlate with your existing session reaper and CSV logger.
Lightweight local fallbacks: macOS: Stats (brew install stats) + Activity Monitor hierarchical view + occasional powermetrics. Windows: Process Explorer + Resource Monitor + LatencyMon when audio/UI feels bad. All: keep your existing one-shot snapshot script and 5-minute CSV logger as the "hammer".
Alternative if process grouping is non-negotiable: Netdata agents everywhere, one Parent (or free Cloud for ≤5 nodes), custom apps_groups.conf as above, alerts on the app groups. Higher overhead and slightly more moving parts.

Decision table (summary):
Criterion | Beszel | Netdata | Prometheus stack
Simplicity | Highest | Medium | Lowest
Overhead | Lowest | Medium | Medium
Process attribution | Weak | Strong | Strong (Linux best)
Multi-host dashboard | Native | Parent/Cloud | Native
Alerting | Good | Excellent | Excellent
"Hammer & axe" repair | Excellent | Good | Fair

### (8) Specific Advice for Electron-Heavy Agent Setups
Published measurements and issues (2025–2026):
- ~20 concurrent Claude Code sessions ≈ 19–20 GB RAM (≈930 MB average per session including children); Codex CLI lighter (~600 MB). [single-source, 2026] GitHub
- Claude Desktop main process has been observed growing unboundedly (~3.7 GB/h) to tens of GB, triggering jetsam. [single-source] GitHub
- Code Helper (Renderer) processes hitting ~100% of one core for hours during Claude Code sessions in VS Code/Cursor. [established] GitHub
- Cursor Agents window re-reading large «внутренний путь лаборатории» transcripts can keep 4+ cores busy even when idle. [single-source, 2026] Forum.cursor
- stdio MCP servers can spawn large process trees (your 176 processes / 7 GB example is consistent with per-session duplication).
Practical mitigations that monitoring should surface: idle session reaper (you already have), limit concurrent Electron apps, watch renderer process counts, and alert on the grouped "claude" / "cursor" / "node" totals rather than individual PIDs.

## Synthesis and Implications
The fleet problem is two-layered: (1) "is any machine currently unusable?" (system load, pressure, thermal, GPU idle vs CPU busy) and (2) "which agent/session/job caused it?" Layer 1 is solved cheaply and robustly by Beszel (or Netdata). Layer 2 is best solved by Netdata's apps.plugin or a Prometheus process-exporter + careful grouping, plus the OS-specific responsible-PID / Job Object view when you need to dig. Combining a lightweight fleet dashboard with on-demand deep process tools matches the "hammer and axe" preference better than a single heavy stack.

## Limitations and Open Questions
- Exact steady-state overhead of Netdata or Beszel under 70+ Claude CLIs + Syncthing 300k files + screenpipe is not published; measure it.
- Reliability of automatic process-tree grouping when Electron spawners and stdio MCP servers create deep, short-lived trees.
- Whether Windows Job Objects can be queried continuously by existing exporters without custom code.
- Experiment that would settle the main open question: run Beszel + Netdata side-by-side on the Intel Mac Pro for one week, alert on both, and record which one first surfaces the next "machine unusable" incident and how quickly you can attribute it.

## Source Catalog
Key sources (all 2025–2026 unless noted): Netdata official docs & GitHub (overhead, apps.plugin, scalability) — multiple 2026 pages. Beszel docs & community measurements (2026). process-exporter README. LatencyMon / Windows performance literature. macOS launchctl procinfo / responsible PID write-ups (The Mitten Mac, Ask Different). Anthropic Claude Code GitHub issues #11615, #105268, #84693 (2025–2026). Cursor forum report (2026). Uptime Kuma limitation analyses (2026). All non-obvious claims above are tied to the cited web results; single-source items are flagged. No URLs were invented.
(UI: 129 sources)

## Source URLs (extracted from the chat's citation chips via DOM, same order as in UI)
- https://www.beszel.dev/guide/network-monitors
- https://www.netdata.cloud/features/architecture/zero-downtime-monitoring/
- https://alertkick.com/blog/uptime-kuma-limitations-what-it-cant-tell-you/
- https://themittenmac.com/the-truetree-concept/
- https://learn.microsoft.com/en-us/windows/win32/procthread/job-objects
- https://github.com/netdata/netdata/blob/master/src/collectors/apps.plugin/README.md
- https://github.com/ncabatoff/process-exporter/blob/master/README.md
- https://latencymon.net/
- https://tendmac.app/guides/check-mac-temperature-apple-silicon
- https://github.com/anthropics/claude-code/issues/105268
- https://github.com/anthropics/claude-code/issues/84693
- https://github.com/anthropics/claude-code/issues/11615
- https://forum.cursor.com/t/idle-agents-window-uses-4-cpu-cores-import-claude-code-conversations-keeps-re-reading-large-claude-projects-transcripts/175494
(UI counter says 129 sources; only 13 are exposed as visible link chips in the final report.)
