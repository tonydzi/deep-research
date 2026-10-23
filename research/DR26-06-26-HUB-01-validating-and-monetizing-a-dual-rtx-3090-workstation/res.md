# DR26-06-26-HUB-01 — raw report from res

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Dual RTX 3090 Validation and Monetization Research

## Bottom line

For your use case, the strongest setup is to treat the machine as a **dual-GPU workstation with two separate 24 GB devices**, not as one magical 48 GB card. The MSI GeForce RTX 3090 GAMING X TRIO 24G is a 24 GB GDDR6X card with 370 W board power, three 8-pin power connectors, PCIe Gen 4, and physical support for the RTX 3090 generation’s NVLink bridge. NVIDIA’s own GeForce RTX 3090 spec page also confirms 24 GB VRAM, NVLink support, and a maximum GPU temperature of 93°C for the reference-class 3090. citeturn4search0turn4search10turn4search3turn30search0

For **automation**, a Linux-first validator is the cleanest path. The reason is simple: NVIDIA’s `nvidia-smi`, CUDA Samples, `memtest_vulkan`, `gpu-burn`, NCCL tests, and PyTorch all run well from the shell and are scriptable. On Windows, the best extra diagnostics are HWiNFO, GPU-Z, OCCT, and MSI Afterburner, but fully unattended OCCT automation is gated behind OCCT’s separate CommandLine edition, which is not the normal free desktop build. citeturn32view0turn11search8turn9view0turn9view1turn35view0turn8search0turn31view0turn3search1turn29view1turn1search2turn1search4

For **money-making**, the practical answer in mid-2026 is: start by listing the cards as **two independent RTX 3090s** on a marketplace that actually accepts consumer GPUs and single-card supply. Vast.ai, io.net, and Clore are the most realistic starting points. Gonka is not a fit for two 3090s because its current supported host reference configurations are in the **~320 GB per ML node** class for MiniMax M2.7 and **640 GB per ML node** for Kimi K2.6. Runpod is no longer accepting new Community Cloud hosts, Salad will not utilize multiple GPUs in one machine simultaneously, Nosana is explicitly **one GPU per PC**, and TensorDock expects datacenter-style hosting with strict uptime, bandwidth, static IP, and hardware requirements plus a 25% revenue share. citeturn14search0turn23search5turn25search2turn26search0turn19view2turn16search4turn36view0turn36view1turn28view0

## Validation workflow for Codex or Claude

The right way to hand this off to an agent is to give it a **toolchain, a test order, and hard failure conditions**. The toolchain below is built from official vendor or project pages for NVIDIA, PyTorch, HWiNFO, OCCT, MSI Afterburner, and the upstream GitHub repositories for CUDA Samples, `memtest_vulkan`, `gpu-burn`, and NCCL tests. GPU-Z is published by TechPowerUp. citeturn32view0turn11search8turn8search0turn31view0turn1search2turn1search4turn3search1turn9view0turn9view1turn35view0

```text
Official downloads and docs for the agent

NVIDIA SMI manual:
https://docs.nvidia.com/deploy/nvidia-smi/index.html

CUDA Samples docs:
https://docs.nvidia.com/cuda/cuda-samples/

CUDA Samples repo:
https://github.com/NVIDIA/cuda-samples

memtest_vulkan releases:
https://github.com/GpuZelenograd/memtest_vulkan/releases

gpu-burn repo:
https://github.com/wilicc/gpu-burn

NCCL tests repo:
https://github.com/NVIDIA/nccl-tests

PyTorch install guide:
https://pytorch.org/get-started/locally/

HWiNFO download:
https://www.hwinfo.com/download/

OCCT download:
https://www.ocbase.com/download

MSI Afterburner:
https://www.msi.com/Landing/afterburner

GPU-Z:
https://www.techpowerup.com/gpuz/
```

The agent should run the checks in this order: **inventory and topology first**, **VRAM test second**, **sustained compute third**, **multi-GPU communication fourth**, and only then a **framework-level or LLM-level smoke test**. That ordering follows the capabilities of NVIDIA’s management tools, CUDA Samples, `memtest_vulkan`, `gpu-burn`, NCCL tests, and PyTorch. `deviceQuery` enumerates CUDA device properties; `bandwidthTest` measures host↔device and device↔device memory copy bandwidth; `busGrind` reports peer-to-peer bandwidth statistics; `memtest_vulkan` is explicitly built to stress video memory stability; `gpu-burn` is a multi-GPU CUDA stress test; and NCCL tests are specifically for correctness and performance of NCCL multi-GPU operations such as all-reduce. citeturn10view0turn9view0turn9view1turn13search3turn35view0turn8search0

```text
Codex/Claude prompt to validate two RTX 3090 cards

Goal:
Validate two installed NVIDIA GeForce RTX 3090 cards and produce a machine-readable health report.
Do not update drivers or flash BIOS unless explicitly asked.
Prefer native Linux for full unattended validation. On Windows, still collect inventory and run command-line-safe tests, but note that OCCT headless automation may not be available.

Required outputs:
1. report.json
2. summary.md
3. raw logs directory
4. csv files for telemetry where possible

Steps:
A. Inventory
- Run nvidia-smi --list-gpus
- Run nvidia-smi topo -m
- Run nvidia-smi topo -p2p n
- Run nvidia-smi nvlink --checkBridge
- Run nvidia-smi nvlink --info
- Capture nvidia-smi --query-gpu=index,uuid,pci.bus_id,name,memory.total,driver_version,power.limit,temperature.gpu,clocks.current.graphics,clocks.current.memory,pcie.link.gen.current,pcie.link.width.current --format=csv

B. CUDA sanity
- Build CUDA Samples if needed
- Run deviceQuery on each GPU
- Run bandwidthTest
- Run p2pBandwidthLatencyTest when both GPUs are visible
- Save outputs

C. VRAM integrity
- Run memtest_vulkan and select each discrete RTX 3090 individually
- Minimum quick pass: 30 minutes per card
- Preferred pass: 2 hours per card
- Record any errors; any VRAM error is a hard fail

D. Sustained compute
- Run gpu-burn on each GPU individually for 30 minutes
- Then run both GPUs together for 60 minutes
- Monitor nvidia-smi once per second during the burn
- Record throttling, clock collapse, Xid, hangs, or driver resets

E. Multi-GPU communication
- Build NCCL tests
- Run all_reduce_perf with both GPUs
- Verify correctness and capture bus bandwidth

F. Framework smoke test
- Install CUDA-enabled PyTorch from the official PyTorch selector
- Verify torch.cuda.is_available()
- Verify both GPUs enumerate
- Run a simple CUDA tensor workload on each GPU
- If both cards pass everything above, optionally run a multi-GPU smoke test

Hard fail conditions:
- wrong GPU model or wrong VRAM size
- any memtest_vulkan error
- deviceQuery failure
- NCCL correctness failure
- Xid / driver reset / GPU lost
- system black-screen reset under load

Soft fail / investigate:
- one card consistently much hotter or slower than the other
- PCIe link width/gen lower than expected under load
- NVLink bridge missing or inactive when installed
- fan instability, excessive noise, or thermal throttling

Decision rule:
Pass only if both cards clear all hard-fail gates.
If one card fails, mark the machine as mixed health and identify the failing GPU by UUID and PCI bus ID.
```

The quickest **on-site purchase sequence** is smaller: boot, check `nvidia-smi`, check basic sensors in HWiNFO or GPU-Z, run `memtest_vulkan`, then do a shorter burn. That is enough to reject the obviously bad cards before you spend hours. `memtest_vulkan` is intentionally lightweight to start: on Windows it runs without install or admin rights, and on Linux it lets you select the discrete GPU to test. The project’s own README says to let it run for **at least six minutes**, though for a used 3090 purchase I would treat that as only a very first screen, not a final certification. citeturn9view0turn34view4turn31view0turn3search1

## Failure modes and pass criteria

The most important fact is that you usually **cannot prove “this card mined” or “did not mine” with software alone**. NVIDIA’s standard tooling, GPU-Z, and HWiNFO expose model identity, clocks, buses, temperatures, utilization, sensors, and reporting, but they do not expose a “mining odometer.” In practice, prior abuse is inferred from current symptoms: unstable VRAM, fan wear, thermal throttling, suspicious BIOS behavior, PCIe anomalies, crashes under sustained load, or bad multi-GPU communication. That is an inference from what these tools actually surface. citeturn32view0turn3search1turn31view0

A **hard fail** is straightforward: wrong model name, wrong 24 GB memory size, device enumeration errors, any `memtest_vulkan` error, CUDA sample failure, NCCL correctness failure, or a driver/device-lost event during burn. NCCL tests are explicitly designed to validate both correctness and performance of collectives; `memtest_vulkan` explicitly reports video-memory errors during the run; OCCT likewise positions its VRAM test as one of the few tests that can thoroughly test all VRAM chips and report issues. citeturn35view0turn9view0turn29view1

A **soft fail** is when the card works, but the information suggests you should demand a deeper discount or skip it unless you want a repair project. Examples include one card consistently running much hotter than the other, one fan that hunts or rattles, a PCIe link negotiating badly under load, a missing bridge detection when a bridge is installed, or sustained operation too close to throttle territory. NVIDIA’s own 3090 spec puts the maximum GPU temperature at 93°C; operationally, for a used card you want real stress tests with clear headroom below that ceiling, because once you are testing near the top of the allowed envelope you are no longer learning much about long-run reliability. That last sentence is an engineering inference, but it is grounded in NVIDIA’s published thermal ceiling. citeturn30search0turn32view0

For a dual-MSI-3090 machine, power and physical integration are part of validation, not an afterthought. MSI rates the GAMING X TRIO 24G at **370 W** and **three 8-pin connectors per card**, with a recommended single-card PSU of **750 W**. Two such cards therefore bring an upper-bound GPU board-power budget of **740 W before CPU, motherboard, drives, fans, and transient spikes**, which is why flaky PSUs and marginal power leads are a real source of false negatives during buying tests. citeturn4search0turn4search10

That power budget matters financially too. If you assume the GPUs alone average their 740 W board-power ceiling while rented, then at **50% utilization** and **€0.25/kWh** you are already at roughly **€66.6/month** in GPU-only electricity; at **100% utilization** that becomes roughly **€133.2/month**, before CPU and the rest of the system. Those numbers come from MSI’s board-power spec plus a simple energy calculation, and they are why occupancy matters much more than theoretical hourly rates. citeturn4search0turn33calculator0turn33calculator1

## NVLink SLI and dual GPU LLM reality

For **games**, the story is bleak. NVIDIA states that for GeForce RTX 3090 and future SLI-capable GPUs, SLI is only supported where the game implements it natively; NVIDIA is no longer adding new traditional SLI driver profiles. In other words, yes, the RTX 3090 is physically NVLink-capable, but the number of modern games where this matters is small enough that I would not buy an NVLink bridge for gaming as a primary motive. citeturn5search0turn4search3

For **LLM work**, the situation is better, but still more limited than many buyers hope. vLLM supports both tensor parallelism and pipeline parallelism across multiple GPUs. It also says, explicitly, that if GPUs on a node **do not have NVLink interconnect**, pipeline parallelism can be preferable due to lower communication overhead. Tensor parallelism is the communication-heavy option and benefits most from fast GPU↔GPU links. So, on a dual-3090 system, an NVLink bridge can help the small class of workloads where you truly want communication-heavy multi-GPU inference or training on both cards. But it still does **not** convert the setup into one monolithic 48 GB GeForce device; the software sees and manages multiple GPUs. citeturn7view0turn7view1turn13search3

Commercially, that means your best default posture is still **two independent 24 GB supply units**, not one permanently bridged pair. If a model fits one 3090, you can serve two jobs, or one on each card, with less coordination overhead. If a model does not fit one 3090, then you reach for explicit multi-GPU software and decide between tensor parallelism and pipeline parallelism based on the framework and interconnect. That recommendation is an inference from NVIDIA’s SLI policy and vLLM’s multi-GPU guidance. citeturn5search0turn7view0turn7view1

## Where two RTX 3090 cards can earn money today

**Vast.ai** is the clearest first experiment. Its official docs describe it as a marketplace where hosts retain control over pricing and rental types, and its host/market docs support on-demand, interruptible, and reserved models. The current live Vast pricing page shows the RTX 3090 at about **$0.13/hr** with a platform-displayed **$0.07–$1.33/hr** range, and Vast’s docs include CLI/API support for searching by exact GPU type and GPU count. That combination makes Vast the most practical place to test whether your 3090s can get rented at all. citeturn14search0turn24search3turn24search4turn23search5turn23search10

**io.net** is also realistic for consumer 3090s. Its supported-device list explicitly includes the RTX 3090, and its rewards docs say suppliers are paid in IO Coin, that block rewards are distributed hourly, and that worker earnings incur a 0.25% fee. io.net’s own support docs also say staking is required for a worker to become “Cluster Ready,” and that unstaking enters a **14-day cooldown**. A current explorer snapshot exposed by search shows GeForce RTX 3090 supply with prices around **$0.18/hr (Ray)**, **$0.20/hr (bare metal)**, **$0.25/hr (VM)**, and **$0.27/hr (container)**, with **10** 3090 devices visible in that snapshot. The upside is that 3090 is officially supported there; the downside is that token-linked economics and staking make the earnings model less straightforward than a plain fiat GPU marketplace. citeturn37view0turn37view1turn37view2turn25search2

**Clore.ai** looks like the best secondary experiment after Vast. Its official docs say spot orders carry a **2.5%** fee and on-demand orders a **10%** fee, and its live 3090 rental page shows a current floor around **$0.18/hr spot** and **$0.27/hr on-demand**. Clore’s own marketing for hosts claims roughly **$165/month per 3090**, but I would treat that as promotional upside rather than as a dependable forecast. The live marketplace floor and fee structure are the parts I would trust. citeturn15search0turn26search0turn26search4

**Golem** is technically viable, but I view it more as an experiment than as the first monetization path. Golem’s GPU-provider guide accepts NVIDIA **30xx series or newer** with **8 GB+ VRAM**, requires IOMMU support, asks for at least as much system RAM as GPU VRAM, wants a stable **100 Mbps** connection, and even boots from a dedicated SSD image. It pays providers in **GLM** and lets you set your own hourly rate. That is workable, but more operationally specialized than a typical home Windows/Ubuntu box owner usually wants. citeturn27view0

Several platforms are poor fits for your exact dual-3090 situation. **Gonka** is effectively out, because the current host quickstart’s reference classes for the active models are roughly **~320 GB VRAM per ML node** for MiniMax M2.7 and **640 GB** for Kimi K2.6. **Runpod** is out for new supply because it says it is **no longer accepting new hosts for Community Cloud**. **Salad** is a bad fit because its own docs say **multiple GPUs in the same machine cannot be utilized at the same time**. **Nosana** is a constrained fit because it warns hosts to use a sandboxed environment and explicitly states **1 private key per GPU, 1 GPU per PC**. **TensorDock** is the most operationally demanding: it requires datacenter hosting for new hardware, 1 Gbps internet, static IP, power/internet redundancy, PCIe bandwidth of at least **16 GB/s per GPU**, and charges a **25% revenue-share fee**. citeturn19view2turn16search4turn36view0turn36view1turn28view0

## Recommended path for your setup

The best near-term plan is this: **validate both cards without buying an NVLink bridge first**, then try to monetize them as **two separate 24 GB GPUs** on Vast.ai first and either io.net or Clore second. That order matches the realities of platform fit, the limited practical value of game SLI in 2026, and the fact that LLM frameworks already treat multi-GPU inference as an explicit distributed configuration problem, not as “one bigger card.” citeturn5search0turn7view0turn14search0turn25search2turn26search0

I would only buy the **NVLink bridge** if one of two things becomes true: either you have a specific local model workflow that consistently benefits from communication-heavy tensor parallelism on the pair, or a real customer workload on your machine specifically wants both 3090s tied together. Otherwise, the bridge is a niche tool, not a default upgrade. NVIDIA’s own SLI position and vLLM’s guidance on when interconnect speed matters both point in that direction. citeturn5search0turn7view0turn13search3

For the machine build itself, assume “home-lab discipline,” not “gaming-rig improvisation.” Two MSI 3090 Gaming X Trio cards mean six PCIe power plugs, aggressive airflow requirements, and a lot of heat. For hosting untrusted workloads, follow the spirit of the stricter platforms: isolated host environment, separate wallet hygiene, and no mixing with your personal daily-driver box. Nosana explicitly recommends sandboxing its host software and a minimal-balance wallet, while TensorDock and Runpod both emphasize customer-data boundaries and host restrictions. That makes the broader security conclusion obvious even though it is partly an inference: if you are going to rent out compute, do it on a dedicated host. citeturn36view1turn28view0turn16search16

The final decision rule I would use is simple: **buy by present health, not by guessed history**. A used RTX 3090 that passes VRAM testing, sustained compute burn, topology checks, and multi-GPU correctness is far more interesting than a “never mined, trust me” card with weak thermals or unstable memory. The tools above are good at validating present-condition risk, and present condition is what decides whether the cards are worth your time. citeturn9view0turn9view1turn35view0turn31view0turn3search1