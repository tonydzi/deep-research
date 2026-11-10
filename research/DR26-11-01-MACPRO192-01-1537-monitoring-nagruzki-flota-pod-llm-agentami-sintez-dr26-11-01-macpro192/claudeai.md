# DR26-11-01-MACPRO192-01-1537 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: claude.ai Research (Opus 5.5, effort Extra), chat https://приватный чат
topic: DR26-11-01-MACPRO192-01-1537 Load monitoring for a fleet of developer machines running many LLM coding agents (macOS + Windows)
date: 2026-11-01

proof: UI "Research complete · 272 sources · 8m 20s"
---
# Load monitoring for a fleet of LLM-coding-agent machines (macOS Intel + Apple silicon, Windows 11, Linux) � November 2026

**Bottom line:** For your fleet, the simplest setup that both shows the load and names which process tree caused it is **Netdata**. Install the agent on all six machines, stream them to a Netdata Parent on the Debian VPS, and send alerts from that Parent to Telegram. Netdata is the only tool I found that groups CPU/RAM by process tree on Linux, macOS *and* Windows (apps.plugin + apps_groups.conf) and alerts on those groups. Beszel is lighter and easier to repair, but in late 2026 it still has no per-process data, so it can tell you a machine is overloaded but not which agent or job is responsible. Prometheus' process-exporter only works on Linux. My confidence is **[emerging]**: Netdata's macOS support is officially "Community" tier and has not been field-tested on an Intel Mac Pro running ~70 `claude` processes or a Windows box with 2,400 processes. Week 1 has to measure the agent's own overhead.

## TL;DR
- **Primary stack: Netdata Agent on every node ? Netdata Parent on the Linux VPS ? Telegram alerts from the Parent.** Use a custom apps_groups.conf for claude/codex/cursor/MCP/python/ffmpeg. This is the only candidate that does native process-tree rollup on macOS and Windows with per-group history and alerts. The open-source Agent is GPLv3+; Cloud/dashboard access is free for 5 nodes and you have 6, so plan on the Homelab plan if your use is personal/non-commercial. Netdata Learn's "Complete Product Description" lists it at "$90/year or $10/month� Personal, non-commercial use� Unlimited nodes (fair usage policy)", and offline or stale nodes are not billed. [emerging]
- **Fallback / "hammer and axe" option: Beszel.** ComputingForGeeks measured Beszel 0.18.7 in July 2026: the systemd agent "held around 14 MB of resident memory", and the hub container "idled around 10 MB of memory in our lab". It has brew + winget packages and Telegram alerts via Shoutrrr. Pair it with your existing snapshot script turned into a per-group Telegram alerter. It covers fleet-wide load/RAM/temperature/GPU history and alerts, but has **no per-process attribution** (Beszel issue #971 is still open). [established]
- **Most of your incidents are attribution and leak problems, not capacity problems.** 2025�26 GitHub issues show Claude Code, Codex and Cursor leaking stdio-MCP children and idle sessions: hundreds of orphaned processes, 7�37 GB, ~90% of a core each. On macOS these orphans get re-parented to launchd (PPID 1), which breaks naive parent-PID rollup. So alert on **process count per group** and **per-group CPU over time**, not just on load. [established]

## Key Findings

1. **Process-tree attribution without cgroups is only native in Netdata.** Netdata's apps.plugin "traverses the entire process tree", groups it with apps_groups.conf, and also counts CPU of children that already exited. Netdata documents this on Linux, FreeBSD, macOS and Windows (Netdata Learn, "Fleet Deployment" and "Function: Top/Processes", 2026). A July 2026 Netdata release added path-derived grouping on macOS, with stock groups for windowserver, spotlight, coreaudio and timemachine (Netdata blog, "Native macOS Monitoring", Jul 2026). [emerging]: vendor documentation only, no independent tests on macOS/Windows. [single-source]
2. **process-exporter does not work on macOS or Windows.** Its README says it "mines /proc to report on selected processes" (ncabatoff/process-exporter README). On Windows the equivalent is windows_exporter's `process` collector. It is off by default, reads all processes with one `NtQuerySystemInformation` call, and exposes `windows_process_info` with `creating_process_id`, `process_group_id`, `owner` and `cmdline` labels (windows_exporter docs/collector.process.md, current main). That is enough to rebuild the tree in PromQL or in a script. [established]
3. **Netdata's licensing and Windows status changed several times, and sources disagree.** In 2024 Netdata said Windows nodes would be "UI-locked on free plans" (GitHub discussion #18422, Nov 2024). Some third-party reviews in 2026 still say Windows needs a paid tier (openmsp.ai; cubeapm.com). But Netdata PR #24327 (2026) says the "standalone Windows local dashboard is not locked. The lock was removed from the dashboard on 2026-03-27", and the current Windows install doc says the local dashboard "works on Windows as it does on Linux, on every plan, including the free Community tier." Netdata's own docs still contradict each other: the "Complete Product Description" page says "only the local dashboard of a standalone Windows Agent is unlocked on paid plans, and Windows Children streaming to a Linux Parent are shown on the Parent's dashboard on any plan." The recommended setup (Windows Children ? Linux Parent) is unaffected either way. The 5-node limit on dashboards still applies to every OS. [emerging]
4. **macOS is officially a "Community" (lowest) support tier for Netdata.** Its platform policy lists macOS 14/15/26 with "No" official native packages and says "Apple Silicon builds are validated in CI" (Netdata Learn, "Platform support policy", 2026). Intel macOS is not mentioned as CI-validated, which is a real risk for the Mac Pro. [established that the tier is Community; speculative whether Intel breaks]
5. **Beszel is the lightest and easiest fleet dashboard with real alerts, but it cannot attribute load.** Install is `brew tap henrygd/beszel && brew install beszel-agent` on macOS and `winget install henrygd.beszel-agent` (v0.21.1) on Windows (beszel.dev Agent Installation; winstall.app). Alerts cover CPU, memory, disk, temperature, load average, GPU and status, with threshold + duration, sent through Shoutrrr to Telegram, ntfy, webhook or email (shekse.com; rdp.sh; denshub.com, 2025�26). Asking for top-N processes is still open feature request #971 (retitled Jan 21, 2026). A 2026 PR adding only process *state counts* was held back because the maintainer was "worried about performance� on systems with lots of processes", and a 2,000-process cutoff was suggested (Beszel PR #2008). That cutoff would exclude your Windows hub. [established]
6. **The agent stack itself is a documented load source.** Examples from 2026 GitHub issues:
   - Orphaned MCP servers each pinning ~90% of a core after a Claude Code session exits, reproduced on macOS, Linux and Windows (anthropics/claude-code #79567, Jul 2026).
   - 106 leaked `claude` processes / 26 GB in 25 h from scheduled tasks, plus 284 Playwright-MCP processes / 10.9 GB (claude-code #91879, Sep 2026).
   - Idle sessions growing to ~15 GB each over 18 h, ending in a jetsam kill (claude-code #18859, Jan 2026).
   - Codex Desktop holding 147 node.exe / 13.9 GiB of MCP processes about 1 h after launch (openai/codex #32797, Jul 2026).
   - Cursor at ~21�22 GB across dozens of helper processes (forum.cursor.com, Apr 2026).

   [established for "this happens"; magnitude varies by version, and every report is user-submitted]
7. **No tool I found measures UI/input latency directly on macOS.** On Windows, LatencyMon measures kernel timer latency, ISR/DPC execution time and hard page faults per driver (Resplendence). It is an audio/real-time suitability checker and "not a general slowdown finder" (windowsforum.com, 2026). [established for Windows; macOS gap: no source found]

## (1) Tool landscape

Overhead figures are the vendor's or a third party's unless marked otherwise. "n/s" = no source found in this research.

| Tool | macOS Intel | macOS arm64 | Win 11 | Linux | Install | Own overhead (sourced) | Per-process / tree | History | Multi-host | Alerting ? Telegram/webhook | Licence / price |
|---|---|---|---|---|---|---|---|---|---|---|---|
| **Netdata Agent (+Parent/Cloud)** | Yes, Community tier, not CI-validated | Yes, Community tier, CI-validated | Yes, native MSI (x64), Win10/11 baseline | Yes, Core tier | install script / brew; `netdata-x64.msi` | Standalone, per Netdata Learn "Complete Product Description": "1�5% CPU and 100�200 MB RAM on an idle system; 5�20% of one CPU core and 250�350 MB RAM in production workloads". As child: "2%�10%, 100�300 MiB" (Netdata Enterprise Evaluation Guide). Marketing: "<5% CPU, 150MB RAM". apps.plugin alone hit 8�9% CPU on a busy VPS (Netdata forum, 2021, older) | **Yes: apps.plugin process-tree groups, incl. exited children; Top/Processes function** | Tiered DB, "3GB disk, >1 year" (vendor) | Parent�child streaming; Cloud (5 nodes free) | Built-in health engine; Telegram/webhook/email via alarm-notify (n/s in this research for Windows-agent notifications) | Agent GPLv3+; UI NCUL1; Cloud Community $0 / 5 nodes; Homelab "$90/year or $10/month", non-commercial, unlimited nodes under fair usage (Netdata Learn "Complete Product Description"); Business $7/node/mo (official page; third parties say $4.50, see Caveats) |
| **Beszel** | Yes (brew) | Yes (brew) | Yes (winget/scoop + NSSM service) | Yes | brew tap; winget; script | ComputingForGeeks, Jul 2026, v0.18.7: agent is "a ~9 MB Go binary"; systemd agent "around 14 MB of resident memory"; hub container "around 10 MB" | **No** (issue #971 open) | Yes (hub DB) | **Yes, purpose-built** | **Yes**: CPU/mem/disk/temp/load/GPU with duration ? Shoutrrr (Telegram, ntfy, webhook, email) | MIT ("Beszel is licensed under the MIT License", henrygd/beszel README); free |
| **Glances (+web, InfluxDB/Prometheus export)** | Yes (brew) | Yes | Yes (Python) | Yes | `brew install glances`; `pip install glances[web,export]` | n/s; `--disable-process` documented to "reduce Glances CPU consumption" | Per-process live; no tree rollup | Only via export (InfluxDB/Prometheus/CSV) | Via TSDB + Grafana | Local threshold colouring only; real alerting needs Grafana | LGPL-3.0; free; v4.5.x in 2026; patch CVE-2026-32632 if using `-w` |
| **btop / htop / bottom** | Yes | Yes | btop/bottom yes (n/s) | Yes | brew / winget | n/s (negligible, interactive) | Live tree view (htop/btop) | No | No | No | Apache/GPL/MIT (n/s); free |
| **node_exporter + windows_exporter + process-exporter + Prometheus + Grafana** | node_exporter yes (n/s) | yes (n/s) | windows_exporter (MSI/winget) | yes | brew / MSI / apt | n/s | windows_exporter process collector (opt-in; parent PID label); **process-exporter is Linux-only (/proc)**; on macOS node_exporter has no per-process collector (n/s) | Prometheus TSDB | Yes | **Best-in-class** (Alertmanager ? Telegram/webhook) | Apache-2.0/MIT; free; most moving parts |
| **Telegraf + InfluxDB (+Grafana/Kapacitor)** | Yes | Yes | Yes | Yes | brew / MSI | n/s | `procstat` with `pattern` (pgrep -f); Windows support "preliminary" (Telegraf README) | InfluxDB | Yes | Via Grafana/Influx tasks | MIT; free OSS |
| **Zabbix agent 2** | Yes (agent) | Yes (n/s) | Yes | Yes (server) | pkg/MSI | n/s | `proc.cpu.util`/`proc.mem` by name (n/s) | Server DB | Yes | Yes (Telegram media type, n/s) | AGPLv3 since 7.0 (Wikipedia/Zabbix); free; heavy server |
| **osquery** | Yes (n/s) | Yes (n/s) | Yes (n/s) | Yes | pkg/MSI | n/s | SQL `processes` table incl. `parent` (n/s) | No (needs a fleet manager) | Needs Fleet/Kolide | No (not a metrics/alerting tool) | Apache-2.0/GPL (n/s) |
| **Uptime Kuma** | � | � | � | Docker | Docker | �30�50 MB (instapods.com, 2026) | **No** | Up/down only | Yes | Yes | MIT; free. **Not enough:** it does HTTP/ping/keyword checks, not host CPU/RAM/process metrics (devtoolbox.blog) |
| **macmon** | **No** | Yes, sudoless, M1�M5 | No | No | `brew install macmon` | n/s (Rust, small) | Process list with per-process CPU/mem/power/GPU | TUI history only | No (but has JSON/Prometheus server) | No | MIT; free |
| **nvidia_gpu_exporter** | � | � | Yes (winget/scoop/service) | Yes | winget / binary | n/s | Per-GPU, not per-process | Prometheus | Yes | Via Prometheus | MIT; free |
| **macOS built-ins** (Activity Monitor, `top`, `powermetrics`, `pmset -g therm`, `memory_pressure`, `log stream`) | Yes | Yes | � | � | built-in | negligible | Activity Monitor groups by *responsible* process | Activity Monitor: none; `pmset -g thermlog` streams | No | No | free |
| **Windows built-ins** (Task Manager, Resource Monitor, PerfMon Data Collector Sets, Process Explorer, WPR/WPA, `Get-Counter`) | � | � | Yes | � | built-in / Sysinternals | negligible (WPR heavier while tracing) (n/s) | Process Explorer tree; Task Manager groups apps | PerfMon DCS ? .blg/CSV on disk | No | PerfMon alerts can run a task (n/s) | free |
| **Stats / iStat Menus / MenuMeters** | Yes (n/s) | Yes (n/s) | � | � | brew cask | n/s | top-N per sensor (n/s) | short | No | iStat has local notifications (n/s) | Stats MIT free; iStat paid; MenuMeters GPL (n/s) |

**Reading the table:** only two rows give you both *fleet* and *per-tree* data out of the box: Netdata, and the Prometheus + windows_exporter route. The Prometheus route needs per-OS work: no tree rollup on macOS, and on Windows you do the rollup yourself in PromQL.

## (2) The attribution problem

**Why it is hard.** Without cgroups there is no kernel "bag" that follows a session. Three facts make PPID-walking unreliable for your stack:
- **Orphans lose their parent.** Leaked MCP servers and task workers get re-parented to launchd (PPID=1) on macOS. Codex #12491 reports 1,319 zombie processes "reparented to launchd (PPID=1)", and Claude Code #33947 is titled "orphan accumulation (macOS PPID=1)". Once orphaned, a pure parent walk files them under "launchd/other".
- **macOS keeps a "responsible PID" that survives this.** `sudo launchctl procinfo <pid>` shows the parent as `1` but still reports `responsible pid` / `responsible path` (TrueTree write-up; Qt blog). Activity Monitor uses the same data. There is no public API: Apple's forum says only a "private responsibility API" exists, with `responsible_audit_token` available through EndpointSecurity (Apple Developer Forums). Terminal-launched CLIs are usually attributed to the terminal app (iTerm2, VS Code's Electron, Ghostty) unless the launcher sheds responsibility (Ghostty #9263). So `claude` started inside Claude Desktop most likely reports Claude Desktop as responsible: useful for "which app", not for "which session". [emerging]
- **Windows parent PIDs are point-in-time.** windows_exporter's `creating_process_id` label gives the parent. **Job Objects** are the only Windows mechanism that tracks a whole tree, including terminated processes: `QueryInformationJobObject` with `JobObjectBasicAccountingInformation` returns TotalUserTime/TotalKernelTime "for all active processes associated with the job, as well as all terminated processes", plus ActiveProcesses/TotalProcesses (Microsoft Learn, JOBOBJECT_BASIC_ACCOUNTING_INFORMATION). Accounting only works if the *launcher* puts children in a job. No monitoring tool does this for you (n/s for any that does).

**Practical design (what I recommend):**
1. **Group by "what" with Netdata apps.plugin**, covering the stack in the config below. This answers "is it claude, codex, ffmpeg or python?" with per-second history.
2. **Get "which session" with a 60-second side-car script**, which is your existing snapshot script extended:
   - On macOS: `ps -axo pid,ppid,pgid,%cpu,rss,etime,command`, plus `launchctl procinfo` for PPID=1 processes in the heavy groups.
   - On Windows: `Get-CimInstance Win32_Process | select ProcessId,ParentProcessId,CommandLine,CreationDate`, joined with `Get-Counter '\Process(*)\% Processor Time'`.

   Walk up to the nearest `claude`/`codex`/`cursor` ancestor. If it is orphaned, label it `orphan:<responsible app>` and record the session's cwd/argv. Append to a CSV, and push the top offender as a Netdata custom chart (statsd) or Prometheus textfile.
3. **Tag heavy jobs when you launch them.** Run ffmpeg/indexer/whisper through a 5-line wrapper (`run-job <session-id> <cmd>`) that writes `pid,session,start` to a ledger file. On Windows the wrapper can also create a Job Object and log its accounting at exit. This is the cheapest way to make "forgotten Python loop, 112 CPU-hours" impossible to miss.

**apps_groups.conf (Netdata; `/etc/netdata/apps_groups.conf` or `edit-config apps_groups.conf`; Windows: in the install dir's `etc\netdata`)**

Format, from the stock file header: `group: process1 process2`. `name*` = prefix, `*name` = suffix, `*name*` = substring of the full command line. Substring patterns make apps.plugin read the cmdline of every process, so use them sparingly. "For each process given, its whole process tree will be grouped" (stock apps_groups.conf header). **Order matters:** put specific agent groups *before* generic interpreters. A python child of claude goes to `python` if `python` matches first, so choose between the two variants below. On Windows, names appear without `.exe` in Netdata's process view, but test both forms (n/s).

```conf
# ---- Variant A: "who" (session attribution) � agents first, NO generic python/node/ffmpeg groups
#      ? python/ffmpeg/node spawned by an agent are charged to that agent's tree
claude:      claude claude.exe *claude-code* *@anthropic-ai/claude-code*
claude-mcp:  *mcp-server* *-mcp* *mcp_* *modelcontextprotocol*
codex:       codex codex.exe *codex*app-server* *codex-cli*
cursor:      Cursor *Cursor\ Helper* cursor.exe
claude-desktop: Claude *Claude\ Helper*
chatgpt:     ChatGPT *ChatGPT\ Helper*
browsers:    *Google\ Chrome* chrome chrome.exe firefox firefox.exe *Firefox*
syncthing:   syncthing syncthing.exe
tailscale:   tailscaled tailscale *io.tailscale.ipn.macsys* *IPNExtension* tailscale-ipn.exe
screenpipe:  screenpipe screenpipe.exe *whisper*
ffmpeg:      ffmpeg ffmpeg.exe ffprobe
# ---- Variant B: "what" (job type) � append these AFTER the agent lines if you prefer job-type view
python:      python python3 python3.* python.exe pythonw.exe *uv\ run* *torch*
node:        node node.exe npx npm
```

Recommendation: run **Variant A + ffmpeg** (ffmpeg is unambiguous and is your #1 CPU hog) and let the side-car script answer "which python". Cmdline substrings (`*codex*app-server*`, `*whisper*`) are best-guess patterns. Verify them on each host with `ps -axo comm,command | sort | uniq -c | sort -rn | head -50` (macOS) or `Get-CimInstance Win32_Process | group Name | sort Count -desc` (Windows). [speculative until verified]

**process-exporter YAML (Linux VPS only, because it reads /proc)**

```yaml
# /etc/process-exporter/config.yml   run: process-exporter -config.path config.yml -children=true
process_names:
  - name: "claude"
    cmdline: ['(^|/)claude(\s|$)', '@anthropic-ai/claude-code']
  - name: "codex"
    cmdline: ['(^|/)codex(\s|$)', 'codex.*app-server']
  - name: "cursor"
    comm: [cursor, Cursor]
  - name: "browsers"
    comm: [chrome, chromium, firefox]
  - name: "syncthing"
    comm: [syncthing]
  - name: "tailscale"
    comm: [tailscaled, tailscale]
  - name: "screenpipe"
    comm: [screenpipe]
  - name: "ffmpeg"
    comm: [ffmpeg, ffprobe]
  - name: "mcp"
    cmdline: ['mcp']
  - name: "python"
    comm: [python, python3]
  - name: "node"
    comm: [node, npm, npx]
  - name: "other:{{.Comm}}"
    cmdline: ['.+']
```

`-children=true` (the default) charges unmatched children to the matched ancestor. `comm` is truncated at 15 characters, so use `cmdline` regexes for anything longer (process-exporter README).

**windows_exporter equivalent:** `--collectors.enabled "[defaults],process" --collector.process.include "^(claude|codex|node|python|pythonw|ffmpeg|Cursor|chrome|firefox|syncthing|tailscaled|screenpipe).*"`, then aggregate in PromQL:

```promql
sum by (group) (
  label_replace(rate(windows_process_cpu_time_total[5m]), "group", "$1", "process", "^(claude|codex|node|python|ffmpeg|Cursor|chrome|syncthing|tailscaled|screenpipe).*")
)
```

Collect `owner`/`cmdline` labels only with admin rights, because they are empty for other users' or protected processes (windows_exporter docs). Expect high label cardinality with 2,400 processes, so keep the include regex tight.

## (3) "Why is it lagging RIGHT NOW" � a 60-second triage

**macOS (in order of predictive value, my ranking [emerging]):**
1. **Memory pressure, not free RAM.** Run `memory_pressure` and check Activity Monitor's pressure graph, swap used and compressed memory. Many 15 GB idle sessions plus jetsam events (claude-code #18859) are the typical failure. Free RAM on macOS is meaningless because the file cache fills it.
2. **Load vs cores.** Load > ~0.7?logical cores sustained (22 on the Mac Pro) predicts UI lag once the work isn't purely low-priority. Your load-24 incident fits.
3. **WindowServer % CPU and `kernel_task`.** High `kernel_task` usually means thermal management stealing CPU (vitalsmac.com glossary, 2026). Check with `pmset -g therm` (Intel: `CPU_Speed_Limit` < 100 = throttled).
4. **Network-extension CPU.** Tailscale's system extension at 264% during scp is the "invisible" one. Group it explicitly (done above).
5. **Disk I/O.** Use `sudo fs_usage -w -f filesys` or `iostat 1`. Syncthing scanning 300k files plus x265 temp writes.
6. **GPU busy.** Covered in section (4).
7. **Logs:** `log stream --predicate 'eventMessage CONTAINS[c] "jetsam" OR subsystem == "com.apple.thermal"'` (n/s for the exact subsystem name; verify locally).

No tool directly measures input-to-photon/keystroke latency on macOS [no source found]. Closest proxy: `spindump`/hang reports in `/Library/Logs/DiagnosticReports` (n/s).

**Windows 11:**
1. **% Processor Time vs % Privileged Time.** A high privileged share points at drivers/AV (Kaspersky) rather than your jobs. windows_exporter exposes `windows_cpu_processor_privileged_utility_total`, and its docs recommend the "utility" counters to match Task Manager (windows_exporter collector.cpu docs).
2. **DPC/ISR time:** `\Processor(_Total)\% DPC Time`, `% Interrupt Time`, then LatencyMon to name the driver. LatencyMon itself runs DPCs and skews "total time in DPCs" (windowsforum.com, 2026).
3. **Hard faults/s** (`\Memory\Pages Input/sec`) and **commit charge**. Hard faults, not "available MB", predict stalls.
4. **Disk queue** (`\PhysicalDisk(*)\Avg. Disk Queue Length`, `% Idle Time`).
5. **Process count / handle count.** Codex issue #44683 notes that Windows WMI/security-tool cost "scales linearly with process count". 2,400 processes plus Kaspersky is a plausible cause of UI stalls on its own. [single-source]
6. **UI thread stalls:** Windows Performance Recorder (`wpr -start GeneralProfile -start CPU` � `wpr -stop lag.etl`) + WPA "UI Delays" / Window-in-focus analysis (n/s in this research).

One-liner snapshot: `Get-Counter '\Processor(_Total)\% Processor Time','\Processor(_Total)\% Privileged Time','\Processor(_Total)\% DPC Time','\Memory\Pages Input/sec','\PhysicalDisk(_Total)\Avg. Disk Queue Length','\System\Processor Queue Length' -SampleInterval 2 -MaxSamples 15`.

## (4) GPU: catching "heavy job on CPU while GPU idles"

- **Windows/NVIDIA:** nvidia_gpu_exporter (Go, wraps `nvidia-smi.exe`, installable via winget/scoop/Windows service, default port 9835, ships a Grafana dashboard) (utkuozdemir/nvidia_gpu_exporter README/INSTALL.md). Netdata also has an nvidia-smi collector (n/s in this research). Beszel shows GPU usage and can alert on it (shekse.com). HWiNFO logs to CSV and has a shared-memory/Prometheus bridge (n/s).
- **Detector rule:** `group_cpu(python) > 4 cores for 15 min AND gpu_util < 10%` ? "CPU-only torch job". This would have caught the 13-hour indexer. Quick confirmation: `nvidia-smi --query-compute-apps=pid,process_name,used_memory --format=csv`. If the python PID is missing, it is on CPU.
- **Apple silicon:** macmon is sudoless, reads "the same data powermetrics exposes", lists per-process GPU usage, and has a JSON/Prometheus server mode. It is **Apple silicon only** (Byron/macmon README; crates.io v0.8.x). Netdata's July 2026 macOS release added "Apple-silicon GPU" metrics (Netdata blog). asitop wraps `powermetrics` and needs sudo.
- **Intel Mac Pro + Radeon Pro 580X:** neither macmon nor asitop applies. Use `ioreg -r -d 1 -c IOAccelerator | grep -o '"Device Utilization %"=[0-9]*'` and `sudo powermetrics --samplers gpu_power -i 5000 -n 1` in the side-car script [no source found in this research for the exact ioreg key on AMD; verify locally].

## (5) Thermal throttling with history

- **macOS Intel:** `pmset -g therm` prints `CPU_Speed_Limit`, where "100 means no limit and a lower number means macOS is capping CPU speed" (vitalsmac.com, 2026). `pmset -g thermlog` streams changes. Log it every 5 min into your existing CSV. On Apple silicon, `pmset -g therm` often prints only "No thermal warning level has been recorded" (AppleToolBox reader comment, M1) [low-authority]. Use macmon's "OS thermal state", or `powermetrics --samplers thermal` (pressure level), instead.
- **Windows:** HWiNFO sensor logging (CSV), or `\Processor Information(_Total)\% Processor Performance` vs `% Processor Utility` from windows_exporter. Sustained performance < 100% under load suggests throttling (inference from the windows_exporter cpu collector docs). ZBooks also have HP's firmware thermal policy (n/s).
- **History:** Netdata stores sensor/thermal charts per second with tiered retention. Beszel stores temperatures and can alert (e.g. 80 �C for 10 min, shekse.com). Either one replaces the "no history" gap.

## (6) Cheap alerting designs

| Rule | Netdata (native) | Beszel (native) | Prometheus | Script + Telegram |
|---|---|---|---|---|
| load > cores?0.7 for 5 min | Yes | Yes (Load Avg alert, fixed value) | Yes | Yes |
| swap growth | Yes (rate of swap used) | Partial (memory % only, n/s for swap) | Yes | Yes |
| one process/group > N cores for > M min | **Yes (app groups)** | **No** | Yes (Linux/Win only) | Yes |
| process count of a name > K | **Yes (app.processes)** | No | Yes | Yes |
| GPU idle while python CPU high | Yes (two-chart expression, n/s) | No | Yes | Yes |
| Telegram/webhook out of box | Yes (alarm-notify; Cloud integrations) | Yes (Shoutrrr) | Via Alertmanager webhook/telegram receiver | curl Bot API |

**Netdata health config (on the Parent, `health.d/agents.conf`).** Context names are from Netdata v2 apps charts: verify with `curl localhost:19999/api/v2/contexts | grep app.` [speculative until verified]

```conf
 template: host_load_high
       on: system.load
   lookup: average -5m unaligned of load1
    every: 1m
     warn: $this > (($system.cpu.processors) * 0.7)
    delay: down 10m
     info: load above 0.7 x cores for 5 minutes
       to: sysadmin

 template: app_group_cpu_hog
       on: app.cpu_utilization
   lookup: average -15m unaligned
    units: %
    every: 1m
     warn: $this > 400
     crit: $this > 800
     info: one app group used more than 4 (warn) / 8 (crit) cores for 15 minutes
       to: sysadmin

 template: app_group_proc_explosion
       on: app.processes
   lookup: max -5m unaligned
    every: 1m
     warn: $this > 150
     info: process count of a group (e.g. claude-mcp) above 150 � likely MCP/session leak
       to: sysadmin

 template: swap_growth
       on: mem.swap
   lookup: average -30m unaligned of used
    every: 5m
     warn: $this > 8192
     info: swap used above 8 GiB (30 min average)
       to: sysadmin
```

Telegram (`health_alarm_notify.conf` on the Parent): `SEND_TELEGRAM="YES"`, `TELEGRAM_BOT_TOKEN="�"`, `DEFAULT_RECIPIENT_TELEGRAM="<chat_id>"`. These keys are confirmed by Netdata Learn's Telegram integration doc: set the recipient to "your Telegram chat ID (find it with служебный бот)", use `chatID:topicID` for topics, and test with `alarm-notify.sh test`. Evaluating health on the Linux Parent avoids depending on Windows/macOS notification scripts.

**Prometheus/Alertmanager equivalents (if you choose the Prometheus route):**

```yaml
groups:
- name: agents
  rules:
  - alert: HostLoadHigh
    expr: node_load5 > 0.7 * count without(cpu,mode) (node_cpu_seconds_total{mode="idle"})
    for: 5m
  - alert: GroupCpuHog
    expr: sum by (instance, groupname) (rate(namedprocess_namegroup_cpu_seconds_total[5m])) > 4
    for: 15m
  - alert: GroupProcExplosion
    expr: namedprocess_namegroup_num_procs{groupname=~"mcp|claude|node"} > 150
    for: 5m
  - alert: WinCpuOnlyTorch
    expr: (sum by (instance) (rate(windows_process_cpu_time_total{process=~"python.*"}[10m])) > 4)
          and on(instance) (avg by (instance) (nvidia_smi_utilization_gpu_ratio) < 0.10)
    for: 15m
  - alert: SwapGrowth
    expr: deriv(node_memory_swap_used_bytes[30m]) > 0 and node_memory_swap_used_bytes > 8e9
    for: 30m
```

(The darwin swap metric name differs by node_exporter version; check yours. Alertmanager has a native `telegram_configs` receiver [n/s in this research].)

**Beszel settings:** per system ? CPU 70% for 5 min, Load Avg 5m > 22 (Mac Pro) for 5 min, Memory 90% for 10 min, Temperature 85 �C for 10 min, GPU usage, Status 2 min. Notifications: `telegram://<token>@telegram?chats=<chat_id>` (Shoutrrr URL format, n/s). Beszel has no default cooldown, so a CPU hovering near the threshold can spam you (devtoolbox.blog) [single-source].

## (7) Recommendation, install commands, decision table

**Primary: Netdata Agents ? Parent on the Debian VPS (over Tailscale) ? Telegram.**

- **macOS (both Macs):**
  ```bash
  curl https://get.netdata.cloud/kickstart.sh > /tmp/netdata-kickstart.sh && sh /tmp/netdata-kickstart.sh
  # or: brew install netdata && brew services start netdata   (brew formula availability: n/s)
  sudo /usr/local/netdata/etc/netdata/edit-config apps_groups.conf   # path varies by install type
  ```
  Then configure `stream.conf` with `[stream] enabled = yes`, `destination = <vps-tailscale-ip>:19999` and `api key = <uuid>`.
- **Windows (hub + 2 ZBooks), admin PowerShell:**
  ```powershell
  Invoke-WebRequest https://github.com/netdata/netdata/releases/latest/download/netdata-x64.msi -OutFile $env:TEMP\netdata-x64.msi
  msiexec /qn /i $env:TEMP\netdata-x64.msi TOKEN="<claim-token>" ROOMS="<room-id>"
  ```
  The MSI is x64-only and needs admin; local dashboard at http://localhost:19999 (Netdata Windows install doc).
- **VPS Parent:** `wget -O /tmp/k.sh https://get.netdata.cloud/kickstart.sh && sh /tmp/k.sh --stable-channel`. Then set up `stream.conf` `[<api-key>] enabled = yes`, the health.d files above, and Telegram keys.
- **Setup time:** ~2�3 h for all six nodes plus ~2 h tuning groups and alerts (my estimate).
- **Expected overhead:** 2�10% of one core and 100�300 MiB per child (vendor figures). Because apps.plugin cost "scales with the number of processes" (Netdata forum), expect the Windows hub (2,400 processes) at the top of that range or above. If it exceeds 5% of a core, raise `[plugin:apps] update every = 5`.
- **Main risk:** macOS (especially Intel) is a Community-tier platform with no official packages. A broken update could silently stop data. Secondary risk: Netdata's free-plan rules keep changing (2024 Windows lock, 2026 removal), and 6 nodes exceeds the 5-node free limit.

**Local fallback per OS (survives any central failure):**
- **macOS:** your snapshot script + `brew install btop macmon` (macmon on the MacBook only) + Stats (`brew install --cask stats`, n/s).
- **Windows:** Sysinternals Process Explorer (`winget install Microsoft.Sysinternals.ProcessExplorer`, n/s for the ID), a PerfMon Data Collector Set started with `logman create counter LagDCS -c "\Processor(_Total)\% Processor Time" "\Processor(_Total)\% Privileged Time" "\Processor(_Total)\% DPC Time" "\Memory\Pages Input/sec" "\PhysicalDisk(_Total)\Avg. Disk Queue Length" -si 15 -f csv -o C:\perflogs\lag && logman start LagDCS`, and LatencyMon for driver-level stalls.

**Decision table**

| Option | Attribution (macOS/Win) | Fleet view | Alerts ? Telegram | Repairability ("hammer & axe") | Own overhead | Cost | Main risk | Verdict |
|---|---|---|---|---|---|---|---|---|
| **A. Netdata Agents + VPS Parent** | **Native tree groups both OSes** | Yes | Yes | Medium (one daemon per host, big codebase) | Medium (2�10% core, 100�300 MiB) | $0 self-hosted; $90/yr Homelab for Cloud >5 nodes | macOS Community tier; licensing churn | **Recommended [emerging]** |
| B. Beszel + extended snapshot script | Script only (DIY) | Yes, excellent | Yes (host-level); script for groups | **High** (single Go binary, PocketBase) | **Lowest** (~10�25 MB) | $0 | You maintain attribution code; no per-process history in the UI | Best fallback / if A disappoints |
| C. Prometheus + node/windows/process exporters + Grafana + Alertmanager | Linux full; Windows via PromQL; **macOS none** | Yes | Best | Low (5+ components) | Medium | $0 | Time sink; macOS attribution gap | Only if you already run Prometheus |
| D. Telegraf + InfluxDB + Grafana | procstat by pattern (Windows "preliminary") | Yes | Via Grafana | Low�medium | Medium | $0 | Same as C, plus Windows procstat maturity | No |
| E. Glances (-w) + InfluxDB | Per-process, no rollup | Via Grafana | Via Grafana | Medium (Python) | n/s | $0 | Python dependency; web-mode CVE history | Use as an ad-hoc TUI only |
| F. Zabbix | Per-name items | Yes | Yes | Low (server + DB + PHP) | n/s | $0 | Heavy for 6 hosts | No |

**What to measure in week 1 (go/no-go):**
1. **Agent overhead:** Netdata's own CPU-seconds/hour and RSS per host, read from its own `netdata`/`apps.plugin` group. Keep A if it stays under 5% of one core on the Windows hub, otherwise tune or fall back to B.
2. **Attribution coverage:** % of total CPU-seconds landing in "other". Target < 15% after group tuning.
3. **Alert precision:** count Telegram alerts and mark each as useful or noise. Target � 70% useful.
4. **Time-to-culprit:** in the next lag episode, minutes from "it's lagging" to the named group or session. Baseline today is manual.
5. **Leak catch:** whether `app_group_proc_explosion` fires before the machine becomes unusable (compare against reaper logs).

## (8) Electron-heavy agent setups: known costs (2025�2026)

All of these are self-reported in GitHub issues or forums. Many are closed as fixed or duplicates, so treat them as *failure modes to monitor*, not current baselines. [established that the failure modes exist; magnitudes single-reporter]

- **Claude Code:**
  - Orphaned MCP servers after session exit, each "~90% of a CPU core indefinitely", e.g. single PIDs with 5,600+ CPU-minutes. Also seen on Linux and Windows 11. Killing the leaked processes took one machine "from 3.0 GB to 14 GB free" (#79567, Jul 2026).
  - Scheduled-task runs leaked 106 resident processes holding ~11.4 GB, and per-session Playwright MCP added "284 extra processes� 10.9 GB" (#91879, Sep 2026).
  - 172 orphaned `mcp-postgres` processes at ~44 MB each = 1.09 GB (#40667, Mar 2026).
  - Heroku MCP `--repl` processes at 90�140% CPU even when never called (#36730).
  - Single `claude.exe` at 21.7 GB on Windows (#32304, Mar 2026) and 18 GB private memory with 7�8 concurrent sessions, while 61 node.exe MCP processes totalled only 0.6 GB (#33735, Mar 2026). The memory is in the CLI, not the MCP children.
  - Idle sessions growing to ~15 GB each over 18 h and getting jetsam-killed (#18859, Jan 2026).
  - Claude Desktop "Cowork" sessions leaking 50+ node processes (#71424).
  - 15 concurrent sessions multiplying MCP servers into a kernel panic on 64 GB (#45880).
- **Codex (desktop / app-server):**
  - macOS app-server at "~7.0�7.3 GB RSS with 160�200% CPU", plus a 5.5 GB task worker (#36971, 2026). A commenter saw 21.4 GB physical footprint and 44.5 GB swap after ~4 days.
  - Windows app-server private memory reached 23.2 GB shortly after restart (#32886, Jul 2026).
  - 147 node.exe / 13.9 GiB / 339 descendants about 1 h after launch with 23 stdio MCP servers (#32797, Jul 2026).
  - Codex CLI on Windows: "464 leaked processes consuming 6.9 GB" in 4.7 h idle, with system process count going from ~500 to 967 (#44683).
  - macOS: "1,319 zombie Codex processes and 1,537 node processes consuming ~37GB RSS + 40GB swap" (#12491, ~Feb 2026; fix announced by a maintainer).
- **Cursor:**
  - ~21�22 GB across renderer (5.14 GB), GPU (3.51 GB), per-workspace extension hosts (417�566 MB each) and ~15 more renderers (forum, Apr 23 2026). Staff: "Renderer 5+ GB: yep, that's a leak"; one extension host per workspace is expected.
  - 8�10 extension hosts at ~4 GB and 97�99% CPU each (forum, Feb 11 2026).
  - Electron renderers have a ~4 GB per-process cap, leading to OOM crashes on Windows (forum, 2026).
  - Staff advise against running many concurrent agent loops in one window.

**Practical advice for your fleet:**
1. **Count stdio MCP servers ? sessions.** Your 176 processes / 7 GB across 56 sessions matches the published pattern (�3 MCP processes/session). Prefer one shared HTTP/SSE MCP server per host over per-session stdio servers where the tool allows it, and disable globally enabled plugins you rarely use (#91879's Playwright case).
2. **Make the reaper leak-aware, not just idle-aware.** Every 15 min, kill MCP/node processes whose PPID is 1 (macOS) or whose parent `claude`/`codex` PID no longer exists (Windows). Feed the counts into the `app_group_proc_explosion` alert.
3. **Cap per-session memory where supported.** For Node-based CLIs, `NODE_OPTIONS=--max-old-space-size=4096` limits V8 heap growth (Claude Code #27788 discusses an unbounded V8 heap reservation) [single-source; test, since some builds are native binaries].
4. **Restart Electron apps daily** (Claude Desktop, Codex, Cursor) using a launchd/Task Scheduler timer at night. Renderer leaks are the dominant cost in the Cursor and Codex reports.
5. **Lower the priority of batch work.** Use `taskpolicy -b` / `nice -n 19` for ffmpeg/indexers on macOS, and `start /low` or a Job Object with a CPU rate cap on Windows, so a forgotten loop costs throughput rather than interactivity.

## Caveats

- **Pricing conflict:** Netdata's own pricing page (2026) says Business "starts at $7/node/month". Several 2026 third-party reviews (cubeapm.com, monitoringcost.com, gappsy.com) say $4.50/node/month annual, and one (makerstack.co) wrongly calls Homelab "$4.50 per node". I trust the vendor page. Homelab ($90/yr) is explicitly "personal, non-commercial". If these machines do paid work, Business pricing applies.
- **Netdata overhead claims** ("<5% CPU", and the University of Amsterdam study "An Empirical Evaluation of the Energy and Performance Overhead of Monitoring Tools on Docker-Based Systems", ICSOC 2023, DOI 10.1007/978-3-031-48421-6_13, cited in Netdata Learn's "Scalability" page) come from the vendor or vendor-cited sources and were measured on Linux/Docker. No independent macOS/Windows measurements exist [no source found]. The experiment that settles it: one week of Netdata's self-reported CPU-seconds on the Windows hub at `update every = 1` vs `5`.
- **Steelman for Beszel over Netdata:** it is a single tiny binary with a hub you can rebuild in minutes, and it covers ~80% of "is the box OK" alerts. Your per-group attribution already exists as a script you understand. The disagreement comes down to whether per-process *history* in a UI is worth a heavier, licence-churning dependency. If week 1 shows Netdata over 5% CPU on the hub, or macOS breakage, switch to B.
- **Everything about agent memory/CPU is user-reported GitHub/forum data** (many reports AI-assisted), version-specific and often already fixed. Your own measurements beat them.
- Items marked n/s / [no source found] (osquery, Zabbix details, menu-bar apps, some exact command keys) come from general knowledge and should be checked before you rely on them.