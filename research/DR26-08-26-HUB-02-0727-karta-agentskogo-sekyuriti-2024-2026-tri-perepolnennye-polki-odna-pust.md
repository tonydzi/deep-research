---
dr_id: DR26-08-26-HUB-02-0727
title: "Карта агентского секюрити 2024-2026: три переполненные полки + одна пустая (fleet control plane) — наш клин"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Карта агентского секюрити 2024-2026, 27.08.2026

Потребитель: слайд «карта рынка и почему мы» + инвест-мемо HUB-03.
Связано: decision-DR26-08-25-HUB-03-control-plane-raise-memo, insight-DR-DR26-08-26-HUB-01-0727-smb-market-penetration, zero-trust-product-line, two-storefronts-raise-and-acquihire, fake-it-courage-not-fake-numbers.

## 🎙 Вердикт
Место для no-name OSS-first команды с работающим флотом **ЕСТЬ** — но НЕ в «security/control plane for AI agents» вообще (там Onyx $40M, Geordie $36.5M, Neo $100M, Runlayer $42M) и НЕ в «AI-SPM / prompt firewall / защита одной модели» (там раздавят платформы). **Пусто — «пустая клетка №12»:** нейтральная диспетчерская для ГЕТЕРОГЕННОГО флота (разные модели/фреймворки/машины/облака), единый causal-журнал действий и handoff'ов, бюджеты (capabilities/деньги/риск, не только токены), fleet-wide pause/kill/rollback, provenance и откат памяти, в **OSS-first self-hosted** слое. Такого продукта у ближайших конкурентов в публичных описаниях **ни один вендор не нашёл**.

**5 proof-points на seed (консенсус):**
1. **Production dogfood с цифрами** — N машин, M агентов, млн tool calls, реально сработавший kill-switch (не демо), MTTK < T сек, $ срезанные budget-cap'ом. ⚠️ Само «мы сами этим пользуемся» — НЕ moat.
2. **2–3 enterprise design-партнёра с частотой** — платный пилот $15–40k сильнее, чем 10–20 F500-intro (F500 без пилота = анти-сигнал).
3. **Публичный research/OSS-артефакт, который пересылают CISO** (CVE, класс атаки MCP, воспроизводимый бенчмарк). ⭐ У нас уже есть семя — [github.com/tonydzi/leash-poc](https://github.com/tonydzi/leash-poc).
4. **Измеримый контроль на ОПАСНЫХ действиях** (отправка наружу, удаление, расход $, эскалация прав) — precision/recall, не «jailbreak % на HarmBench».
5. **Слайд non-overlap** vs PANW Prisma AIRS / Cisco AI Defense / Zenity / WitnessAI / Entra Agent ID / Invariant / Operant.

## Блок 1. Карта компаний (ключевые, с раундами)
> Цифры — из chatgpt (SEC-первоисточники); стадия на авг.2026. Спорные атрибуции помечены.

| Компания | Фаундеры | Что защищает | Раунды · оценка | Exit |
|---|---|---|---|---|
| **CodeIntegrity** | Rose-Hulman (НЕ 8200) | Детерминированный runtime-посредник, MCP integrity | seed $5M SYN+Antler+Boost (27.05.26) | — |
| **Runlayer** | ex-Zapier/Nanit | MCP gateway + agent platform | seed $11M Khosla/Rabois (17.11.25) → A $30M → $42M | — |
| **Onyx Security** | Unit 8200 + Nvidia | Самый буквальный «secure AI control plane» | $40M Conviction+Cyberstarts (12.03.26) | — |
| **Geordie AI** | Darktrace/Snyk alumni | Fleet discovery+posture+runtime (Beam) | seed $6.5M → A $30M Balderton → $36.5M | — |
| **Neo** | ex-SentinelOne execs | Agent/MCP inventory+attribution+blocking | seed $25M → A $75M (20.07.26) → $100M | — |
| **Glow** | ex-Meta/Snowflake/Claroty | Prevention-first endpoint (agents/MCP) | $180M, **оценка $1.2B** (22.07.26) | — |
| **Zenity** | Unit 8200 (Copilot-research) | SSPM+runtime для Copilot/Agentforce | до C $125M Norwest (03.08.26) → ≥$184.5M | — |
| **Noma Security** | Unit 8200 | End-to-end AI-SPM | seed→A→B $100M Evolution → $132M | — |
| **WitnessAI** | ex-PANW/Google | Network-level policy для employee AI | A $27.5M → +$58M → $85.5M | — |
| **Archestra** | Grafana/OSS alumni | **OSS control layer** для MCP/CLI/APIs | pre-seed $3.3M → seed $10M 20VC → $13.3M | — |
| **Trent** | AWS/Cambridge academics | **Security-агенты над агентами (multi-agent)** | seed $13M LocalGlobe (07.04.26) | — |
| **Willow** | ex-Wix | Agent identities+scoped access+gateway | seed $7M Hetz (04.06.26); Wix-нагрузка | — |
| **Invariant Labs** | ETH Zürich (research) | MCP tool-poisoning research, mcp-scan — **создали повестку MCP-security** | не раскрыто | **Snyk 24.06.25** |

**M&A-волна (платформы купили «галочку CISO»):** PANW→Protect AI **$634.5M** · Cyera→Oasis LOI **~$1B** · CrowdStrike→Pangea $212.1M · Check Point→Lakera $201.8M · SentinelOne→Prompt Security ~$180M · F5→CalypsoAI $145.2M · Tenable→Apex $47.8M · Cisco→Robust Intelligence + Cisco→Astrix (~$400M) · OpenAI→Promptfoo · Fortinet→Virtue AI.

## Блок 2. Деньги категории
Единого тотала нет (категория размазана). chatgpt по датированным equity-траншам (нижний предел, без M&A): **2024 ≥$370M · 2025 ≥$329.7M · 2026 YTD ≥$592M** (с announcement-flow ≥$778.5M). Живая карта Prompt Security (21.08.26): **398 вендоров, $11.2 млрд капитала, 35 приобретений (19 из них в 2026)**. Наблюдаемый объём ~удвоился 2025→2026.

## Блок 3. Один агент vs ФЛОТ — ядро вердикта
Безопасность **одного** агента/модели (проверить prompt, скан MCP, выдать credential) — **перенаселена**, все M&A 2024-25 сюда. Безопасность **флота** должна понимать: кто породил агента и от чьего имени · delegation/handoff lineage · общий capability/spend/risk budget · причинную цепочку через машины/фреймворки · cross-agent propagation атак/poisoned memory · fleet-wide pause/rollback/replay · жизненный цикл общей памяти.
Это **уже не полный greenfield** (Trent, Geordie, Onyx, Neo рядом), но никто не продаёт **ПАКЕТОМ**: аудит MCP + runtime kill + per-agent budget + human approval + immutable log + multi-machine — для **гетерогенного** флота. Щель жива, потому что A2A (апр.2025) и MCP (стандарт 2025) создали объект, которого капитал не успел занять специализированным Series A. Окно = **18–36 мес**.
⚠️ Угроза «занятого языка диспетчерской» — hyperscaler-control-towers (Microsoft Agent 365/Entra Agent ID, ServiceNow AI Control Tower, Salesforce Agentforce, Google Agentspace, AWS Bedrock AgentCore), но все закрывают СВОИ агенты в СВОЁМ облаке → наш анти-колок: «мы сидим на флоте, который их инструменты не видят: cross-model, cross-machine, tool-using с бюджетами и human gates».

## Блок 4. Наш клин + позиционирование
**Ближе всех:** Onyx ($40M, но без OSS/cross-machine replay/memory provenance), Geordie ($36.5M, без OSS data plane/spend budgets/rollback), Archestra ($13.3M, ближайший OSS-first, без execution lineage/OS-kill), CodeIntegrity ($5.25M, deterministic pre-execution, без fleet inventory/budgets/multi-machine graph), Trent ($13M, явный multi-agent, без универсального executor/rollback).

**Формулировка (консенсус):** «Open-source flight recorder + policy runtime для гетерогенного self-hosted мультиагентного флота: детерминированный pre-execution контроль, делегированные capability/spend бюджеты, fleet-wide kill/rollback, cross-agent lineage, memory provenance.» MVP grok = **4 кнопки: inventory · policy · kill · log**; бюджеты вторыми; knowledge — мост, не ядро.
**Buyer на seed = CTO / Head of AI Platform / SRE**, НЕ CISO и НЕ банк (банк = серия B как sidecar к PANW).
⚠️ Knowledge/memory подавать как НОВЫЙ security-object («на основании каких данных агент решил действовать»), НЕ как RAG-permissions (иначе лоб в лоб с Knostic).
**Где раздавят:** лобовой «AI security platform» (Noma/Zenity/Neo) · «MCP gateway» (Runlayer/Archestra коммодитизируется) · generic red-teaming (Promptfoo/Lakera) · identity (Oasis/Astrix/Entra).

## Блок 5. Инвесторы — кому НЕ слать / кому слать
**⛔ НЕ первыми:** IL cyber-фонды (8200-driven) — YL Ventures, Cyberstarts, Team8 (для no-name без pedigree конверсия низкая).
**✅ Слать:** **Ballistic Ventures** (grok: «лучший кибер-фонд под вас»; Noma/WitnessAI/Operant), Work-Bench, Boldstart, **YC** (инвестирует до enterprise GTM), EU deep-tech (redalpine-тип), ангелы-CISO из Protect-AI/HiddenLayer-сети. **a16z-seed** — только при вирусном MCP-артефакте (прецедент: Promptfoo seed — OSS-команде НЕ из 8200). **Insight** — после traction (у них тезис + прямое предупреждение против слишком широкого «all agent security» pitch). **SYN** — CodeIntegrity (два инженера Rose-Hulman с exploit proof, НЕ CISO).
**Доказанные пути «замены pedigree»:** Protect AI (Сиэтл, не 8200 → OSS Huntr → exit PANW), Lakera (швейц. research + Gandalf), Robust Intelligence (Harvard prof + бенчмарки → Cisco), Promptfoo/CodeIntegrity/Archestra/Trent/AgentOps (OSS/research/community).

## Блок 6. Консенсус vs расхождение
**Сошлись (на слайд):** место есть только на pre-seed/seed $2–6M · три полки заняты + одна пустая (fleet control plane) · нельзя называться AI-SPM/firewall/IAM/observability · buyer = platform/SRE не CISO · 5 proof-points (dogfood-числа, платные пилоты, MCP-артефакт, контроль на опасных действиях, non-overlap слайд) · dogfood ≠ moat без публичных данных · окно 18–36 мес · избегать IL-cyber, идти Ballistic/YC/a16z-seed.

**Разошлись:** chatgpt сильнее по ФАКТУРЕ (~116 ссылок, SEC-филинги, нашёл всю 2026-волну Zenity/Straiker/Onyx/Neo/Oasis/Glow); **grok слабее по цифрам** (не делал live-выгрузки, по ~40% «не раскрыто», перепутал фаундеров Noma/HiddenLayer, пропустил 2026-раунды — сам признал «дыра прогона без Crunchbase»), но **сильнее по стратегии** (hyperscaler-угрозы, «пустая клетка №12», MVP-4-кнопки, инвестор-логистика).
**Рабочая связка для дека: цифры/раунды — из chatgpt (спорные сверять по первоисточнику); стратегическую рамку и анти-колок — из grok.**

---
*Провенанс: синтез из 2 DR-легов (chatgpt/codex ~116 ссылок + grok), субагент 27.08.2026. Кворум 2/6, браузерные рельсы не добраны. Оригиналы: «внутренний архив лаборатории».*
