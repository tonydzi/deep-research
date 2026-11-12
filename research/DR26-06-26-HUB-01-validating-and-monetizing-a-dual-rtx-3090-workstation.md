---
dr_id: DR26-06-26-HUB-01
title: "Validating and monetizing a dual RTX 3090 workstation"
date: 2026-06-26
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-26-HUB-01): Validating and monetizing a dual RTX 3090 workstation

> How to validate the hardware health of two RTX 3090 GPUs with a scriptable toolchain, and which GPU-rental platforms realistically accept them for monetization.

## Ключевые выводы
- Best mental model is two independent 24GB GPUs, not one 48GB device — NVLink physically supported (RTX 3090, 24GB GDDR6X, NVIDIA confirms NVLink + 93°C max temp) but SLI is dead for gaming (NVIDIA no longer adds SLI profiles; only native game support remains).
- For LLM workloads, vLLM supports tensor parallelism and pipeline parallelism; pipeline parallelism is preferable without NVLink due to lower communication overhead — NVLink only helps communication-heavy tensor-parallel workloads, doesn't merge VRAM into one pool.
- Linux is the clean automation path: nvidia-smi, CUDA Samples (deviceQuery, bandwidthTest, p2pBandwidthLatencyTest), memtest_vulkan, gpu-burn, NCCL tests, and PyTorch are fully scriptable; on Windows, OCCT's headless/unattended automation requires a separate paid CommandLine edition (not the free desktop build).
- Recommended validation order: inventory/topology → VRAM integrity (memtest_vulkan) → sustained compute (gpu-burn, per-GPU then both together) → multi-GPU communication (NCCL all_reduce_perf) → framework smoke test (PyTorch).
- Software cannot prove prior mining/abuse history directly — no tooling exposes a 'mining odometer'; prior abuse must be inferred from symptoms (VRAM instability, fan wear, throttling, PCIe anomalies, crashes under load, bad multi-GPU comms).
- MSI RTX 3090 GAMING X TRIO 24G draws 370W board power with 3× 8-pin connectors (750W recommended single-card PSU); two cards = ~740W GPU-only ceiling before rest of system, driving real electricity cost: ~€66.6/month at 50% utilization and ~€133.2/month at 100% utilization (€0.25/kWh).
- Monetization platform fit varies sharply: Vast.ai (~$0.13/hr RTX 3090, $0.07–$1.33/hr range) and io.net (officially supports 3090, pays in IO Coin, 0.25% fee, 14-day unstake cooldown, ~$0.18–0.27/hr across modes) are realistic; Clore.ai is a solid secondary option (2.5% spot / 10% on-demand fee, ~$0.18/hr spot floor, promotional claim of ~$165/month per 3090 treated as upside not guarantee).
- Several platforms are poor/no fits for a dual-3090 home rig: Gonka requires ~320–640GB VRAM per ML node (way beyond two 3090s), Runpod no longer accepts new Community Cloud hosts, Salad can't utilize multiple GPUs in one machine simultaneously, Nosana requires 1 GPU per PC, TensorDock demands datacenter-grade hosting (static IP, 1Gbps, 25% revenue share).
- Golem is technically viable (accepts 30xx-series, 8GB+ VRAM, pays in GLM, host sets own rate) but is more operationally specialized (IOMMU, dedicated SSD image, stable 100Mbps) than a typical home-rig fit.

## Рекомендации / решения
- Validate both cards using a Linux-first scripted toolchain (nvidia-smi, CUDA Samples, memtest_vulkan, gpu-burn, NCCL tests, PyTorch) with defined hard-fail conditions (wrong VRAM/model, any memtest_vulkan error, deviceQuery/NCCL failure, Xid/driver reset) before making any purchase or deployment decision.
- Don't buy an NVLink bridge by default — only add it if a specific workload demonstrably needs communication-heavy tensor parallelism across both cards, or a real customer explicitly requests it.
- Start monetization by listing the two GPUs as separate 24GB units on Vast.ai first, then try io.net or Clore.ai as a second platform — avoid Gonka, Runpod (new hosts), Salad, Nosana, and TensorDock given their structural mismatch with a dual-GPU home box.
- Treat the rig as a dedicated, isolated host for rental workloads (separate wallet hygiene, sandboxed environment, not the daily-driver PC) rather than folding rental hosting into personal use.
- Decide card quality by present-condition stress testing (VRAM integrity, sustained burn, multi-GPU correctness), not by claimed mining history — a passing used card beats a 'never mined, trust me' card with weak thermals.

## Сущности
- **Люди:** —
- **Компании:** NVIDIA, MSI, Vast.ai, io.net, Clore.ai, Golem, Gonka, Runpod, Salad, Nosana, TensorDock, TechPowerUp
- **Продукты/инструменты:** RTX 3090 GAMING X TRIO 24G, NVLink, nvidia-smi, CUDA Samples, deviceQuery, bandwidthTest, p2pBandwidthLatencyTest, memtest_vulkan, gpu-burn, NCCL tests, PyTorch, HWiNFO, GPU-Z, OCCT, MSI Afterburner, vLLM

## Открытые вопросы
- No software method exists to definitively prove or disprove a used GPU's mining history — reliance is on inferred symptoms only.
- Whether an NVLink bridge purchase is worthwhile remains contingent on future workload needs (communication-heavy tensor-parallel jobs) not yet confirmed.
- Actual sustained rental occupancy/pricing on Vast.ai/io.net/Clore for this specific hardware is untested — quoted rates are marketplace snapshots, not guaranteed earnings.
- OCCT's paid CommandLine edition (needed for full Windows automation) wasn't evaluated in depth — cost/fit unresolved if Windows-based automation is required.

## Источник
- DR-ID `DR26-06-26-HUB-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- gpu-rental-monetization
- dual-gpu-llm-inference
- home-lab-hosting-security
- vllm-multi-gpu-parallelism
- gpu-hardware-validation-toolchain
