# DR26-10-24-HUB-01 — raw report from counters

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Continuous Monitoring (Built‐in Counters)  
Use Windows Performance Counters (PerfMon/PDH via Get-Counter, typeperf or logman) for ongoing low‑overhead metrics. PerfMon can record CPU, memory, disk, network, etc., with minimal impact. Counter data is provided by the OS kernel, so overhead is usually a few percent CPU at most. **Limitations:** counter values can “wrap” or be missing if a process exists between samples. For example, PDH’s PdhCollectQueryData may return `PDH_NO_DATA` if a counter instance did not exist at sample time.  Wildcard queries (e.g. “\Process(*)\% Processor Time”) may show duplicates or reindexed instance names when processes restart. A common workaround is to also collect **“ID Process”** counters and map names to PIDs offline. Note that many counters are protected by ACL; querying **all** instances typically requires an elevated session. In practice run PerfMon/logman as admin to see full data.  

- **CPU (%) normalization:** PerfMon’s “% Processor Time” is relative to one logical CPU, so a fully busy 28-core system shows ~2800%. Divide by core count if you want 0–100%.  
- **Instance identity:** The legacy “Process” performance object uses names (e.g. `chrome#3`), which shift as processes exit/launch. Windows 10+ introduced ETW “Process_V2” events for stable IDs, but PerfMon itself has no easy PID tagging. Always collect the Process\ID Process counter to resolve names→PIDs.  
- **High churn:** If many short-lived processes spawn faster than your sample interval, PerfMon will miss them (“lost fast processes”). Only kernel tracing (WPR/Sysmon) can catch 10–100 ms tasks. Similarly, PDH may mark samples invalid under saturation. Use conservative intervals (e.g. ≥1 sec) to limit PDH overhead; verify PDH returns success (not `NO_DATA`).  
- **Protected processes:** User-mode counters typically cannot see “protected” processes (like some AV or DRM processes). Only high‑privilege or kernel tools (Driver-based) can measure those. PerfMon and Get-Counter run in user context and omit protected processes.  

**Elevation required:** Most PerfMon counters and logman data-collector sets require admin privileges to enumerate all objects and instances (e.g. `Get-Counter -ListSet *` needs elevation). Some basic CPU/memory counters work without it, but plan to run as admin for completeness.  

# On-Demand Deep Trace (ETW/WPR)  
For detailed diagnosis use Windows Performance Recorder (WPR) with an appropriate profile (e.g. **“CPU Samples”** or **“GeneralProfile”**). WPR uses ETW to capture kernel‐level data (CPU stacks, DPCs/ISRs, process create/exit, disk I/O latency, etc.). Overhead depends on profile: a *light* trace (CPU sampling at low frequency, kernel symbols off) is modest, but full tracing can be heavy (multi‐percent CPU). Store data in a circular memory buffer or small file to avoid saturating disk. WPR can be run from the command line (e.g. `wpr -start generalprofile -onoffscenario`) or via PowerShell. The trace file can be opened in WPA for analysis.  

- **Memory circular traces:** To avoid huge log files, use in-memory ETW buffers or limited-duration captures. For example, WPR’s “MemoryLight” profile uses minimal disk I/O. Always specify a reasonable buffer size (e.g. 128 MB) and stop/flush before full.  
- **DPC/ISR:** WPR can measure deferred procedure calls and interrupts (via *Kernel\Interrupt* and *Kernel\DPC* ETW). PerfMon also has `\Processor(_Total)\DPCs Queued/sec` and `% DPC Time`, but WPR provides call stacks.  
- **Disk physical vs logical I/O:** PerfMon distinguishes Logical I/O (Process\IO Data Bytes/sec, counting all virtual file I/O by a process) vs Physical I/O (PhysicalDisk counters measuring actual HDD/SSD transfers and latencies). To attribute disk latency to a process, use WPR with Disk IO tracing – PerfMon alone can report avg latency (Disk\Avg Disk sec/Read) but not per-process. 

**Elevation required:** WPR must be run as admin (it uses kernel tracing). For example, the Defender perf tools (`New-MpPerformanceRecording`) internally invoke WPR. Without admin you cannot start an ETW session.  

# Process Tracking (Sysmon)  
Install **Sysmon** (System Monitor from Sysinternals) as an agent for granular process audit. Sysmon logs all process creation/termination events to the Event Log (Event ID 1 and 5) with full command lines, parent PID, etc. This ensures *no* process start or exit goes unrecorded, even for very short-lived tasks. You can filter Sysmon rules to only record process events to minimize load.   

- **Parent/start-time:** Sysmon’s “Process Create” event includes the parent process and precise timestamp, solving the unreliable parent attribution of plain PerfMon. The Process GUID and ID let you tie lifetime across events.  
- **Short-lived processes:** The “Process Terminated” event explicitly logs when a process exits, so you catch any 1–100 ms processes. PerfMon polling or typeperf would completely miss them.  

**Elevation required:** Sysmon must be installed as a Windows service (requires admin) and remains resident. It consumes a small but non-zero amount of CPU/RAM (typically a few percent on busy systems). Without admin or policy rights, Sysmon cannot be used.  

# Other Tools  
- **Process Explorer (Sysinternals):** An interactive GUI showing per-process CPU/GPU/disk usage live. Useful ad hoc but not for logging history. It requires elevation to see all processes/handles. Overhead is low (it polls counters periodically), but it’s not suited for scripting or long runs.  
- **Process Monitor:** Logs all filesystem/registry operations. Extremely high overhead – do *not* run continuously for performance analysis. Use only for targeted troubleshooting.  
- **PerfMon PerfKit (logman/typeperf):** You can script `logman` or `typeperf` for lightweight logging to CSV. Overhead is negligible for a few counters. Beware PDH errors if you hammer too fast – always verify return codes (see PDH docs).  
- **Glances (HTTP/MCP):** A Python-based cross-platform monitor. It provides current system and process stats via a REST/MCP API. *Pros:* easy setup, cross-OS; *Cons:* not native (needs Python/psutil) and may have more overhead. Glances maintains in-memory history (default ~1200 samples per metric), but it is designed as a real-time display, not a guaranteed historical log. It will miss extremely short tasks like PerfMon. As a userland process, it cannot see protected processes either. No official benchmarks of its overhead exist; assume a few percent CPU on busy systems. Glances requires no elevation for basic stats, but certain info (e.g. full command-line of other users) may be limited.  

# Key Issues and Pitfalls  
- **Instance Renaming:** When PerfMon reports `process.exe#2`, then `#1` goes away, matching counters to PIDs is tricky. Always record “ID Process” counter to disambiguate. Or switch to WPR ETW trace, which uses GUIDs.  
- **“Process” vs “Process v2”:** There is no PerfMon “Process v2” counter object. The ETW Process_V2 provider exists (for kernel traces), but get-counter only has “Process”. In Windows 10+, the Process counter values match ETW; use Sysmon or WPR to get richer fields.  
- **Invalid PDH Samples:** If system is very busy or a process vanishes between samples, PDH may report 0 or stale values. Always check `PdhGetFormattedCounterValue` return codes or use `PdhGetRawCounterArray` after a wildcard query to see which instances are valid.  
- **Disk vs CPU Attribution:** Process I/O counters count *logical* bytes issued; actual physical latency comes from underlying disk counters. Likewise, GPU usage is reported per engine: one process using both 3D and video engines can appear to use >100% of the GPU when summed, due to engine overlap. There is no simple “GPU % by process” counter that avoids double-counting. (NVIDIA’s NVML can break out 3D vs compute, but PerfMon’s GPU Engine counters are coarse.)  
- **Kernel Time (DPC/ISR):** To check kernel overhead, include PerfMon “Processor\% DPC Time” and “\Processor(_Total)\Interrupts/sec”. WPR can capture stacks of DPC routines. There is no trivial user-mode counter for “input latency” – measuring how long a keystroke takes to process usually requires targeted ETW tracing or loopback tests.  

# Experiment and Validation  
**Before/After Test:** Baseline the system performance *without* monitors (e.g. run a workload that spawns many quick child processes, GPU tasks, disk I/O, etc.). Record key metrics (CPU%, queue length, latency). Then enable your monitor stack (PerfMon, Sysmon, etc.) and re-run the same workload. Compare:  

- **Overhead:** Use Task Manager or PerfMon to watch the monitor tools themselves (e.g. Sysmon service, WPR service, glances process). Their CPU and RAM should be minimal (e.g. <5% CPU in total). If overhead is high, scale back (reduce counters, increase intervals).  
- **Detection:** Verify short-lived processes were recorded. E.g. have a known fast task (`ping -n 1 localhost` in a loop). Sysmon’s Terminated events or WPR’s process events should see it; PerfMon alone will not.  
- **Accuracy:** Compare PerfMon CPU totals with taskmgr. With 28 cores, ensure 100%×28 is consistent. If sums differ, check if counter aggregation is correct.  
- **Stability:** Run 30–60 min with logging on. Ensure no data collector failures or “heartbeat lost” alerts. Check log file sizes: PerfMon can be set to overwrite (circular buffer) so it does not run out of disk.  

**Acceptance Criteria (heuristic):** Monitor stack adds <5–10% CPU (combined) and <50 MB RAM. It should capture ≥95% of actual processes (none of the known short tasks should be missed). It must not drop samples (“PDH_NO_DATA”) except transiently – if so, tighten the config. Alerts: if PerfMon queue length > 5 or context switch rate extremely high, trigger deeper trace.  

**Rollback:** Be ready to disable each tool if it degrades performance. For example, `logman stop` to stop PerfMon data collection, `wpr -stop` to end WPR traces, `Sysmon.exe -u` to uninstall Sysmon, and simply kill the Glances/ProcessExplorer processes. Include these steps in a recovery checklist.  

# Recommended Stack (Non-Elevated Fallback)  
- **Standard user:** Use PerfMon counters (via Task Manager’s Performance Monitor or PowerShell `Get-Counter`) for basic CPU, memory, disk/I/O stats. These require no new installs. Add Sysmon only if you can get one-time admin to install it; otherwise skip it. You can run Glances (Python) as user for a live dashboard, but it’s optional. For ad hoc detail, use Process Explorer (as user, then approve UAC if needed).  

- **Elevated/admin:** In addition to above, deploy Sysmon for process logging. Set up a low-overhead PerfMon logman Data Collector (1–5 sec samples of key counters) and keep it running. Prepare a WPR profile to run on demand for deep dives (covering CPU, disk I/O, DPC, etc.). Use Defender’s `New-MpPerformanceRecording` if investigating antivirus scans specifically.  

# Checklist to Verify Non-Slowdown  
- **Monitor resource use:** With monitoring on idle, PerfMon and Sysmon CPU% should be near zero. Spike CPU loads to ensure monitor processes are not competing with work.  
- **Data consistency:** Cross-check monitors. If PerfMon shows 20% idle CPU, and TaskMgr shows 20% idle, counters are sane. If glances shows wildly different CPU usage than PerfMon, investigate (perhaps glances not polling fast enough).  
- **Disk usage:** Ensure PerfMon’s log files or WPR ETLs aren’t filling disk unexpectedly. Use circular logging or separate drive.  
- **No missing data:** Introduce a known short task; confirm it appears in Sysmon log or WPR trace. If not, maybe sampling is too coarse.  
- **Disable test:** Briefly disable all monitors and rerun workload; performance should not improve significantly. If “slowdown” disappears, one of the monitors was the culprit.  

By combining lightweight PerfMon counter logging (for ongoing trends) with targeted ETW traces on demand, and validating via a before/after test, you can diagnose slowdowns without the monitor itself inducing delays. 

**Sources:** Microsoft PerfMon/PDH docs, Sysmon official guide, Defend­er cmdlets docs, and glances documentation. These confirm behavior of each tool and required privileges.