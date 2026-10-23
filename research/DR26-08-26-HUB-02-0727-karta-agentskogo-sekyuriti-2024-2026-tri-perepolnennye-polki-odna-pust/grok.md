# DR26-08-26-HUB-02-0727 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

A. ВЕРДИКТ (одна страница)

Место есть. Но не в «AI-SPM / prompt firewall / защита одной модели». Там вас раздавят платформы. Место — узкая незакрытая полка: **runtime control plane для гетерогенного флота агентов** (несколько машин, несколько моделей, MCP/A2A, kill-switch, бюджеты, журнал, гейты одобрений). Это не то же самое, что Zenity/Noma (инвентарь SaaS-копилотов), не Lakera/Cisco (firewall одной модели), не Astrix/Microsoft Entra Agent ID (IAM), не AgentOps/LangSmith (трейсы без принуждения политики).

Деньги в широкую «AI security» уже зашли: 2024 — сотни миллионов VC; 2025 — ещё сотни миллионов плюс три якорных M&A (Robust Intelligence → Cisco, Protect AI → Palo Alto, Prompt Security → SentinelOne). 2026 (до августа) — волна MCP-native seed’ов при одновременном закрытии полки «гарантии для ChatGPT-обёртки». Точный PitchBook-тотал по категории «agent security» публично не существует: вендоры, пресса и фонды мешают security-for-AI, AI-for-security (SOC-агенты) и NHI. Ниже — пол раскрытых именных раундов, не «цифра Crunchbase». Живая выгрузка Crunchbase/PitchBook в этом прогоне не делалась (контракт: без инструментов). Где суммы/оценки не подтверждены первичным релизом — прямо «не раскрыто» или «оценка прессы».

Почему щель жива в 2026: CISO уже покупает галочку «AI security» у PANW/Cisco/S1/Wiz/Microsoft. Интеграция купленных стартапов занимает 18–36 месяцев и заточена под их воронку (модель, промпт, DLP, SaaS-копилот). Флот coding-агентов на нескольких машинах с MCP — покупатель 2025–2026, его нет в старых ICP. Hyperscaler control tower (Microsoft Agent 365 / Entra Agent ID, ServiceNow AI Control Tower, Salesforce Agentforce Command Center, Google Agentspace) закрывает **свои** агенты в **своём** облаке. Независимый диспетчер смешанного флота (Claude + GPT + local + MCP, несколько хостов, человеческие гейты, бюджеты) как продукт — пусто или ранний seed.

Честно про no-name OSS-first команду с живым своим флотом: **да, это один из немногих путей на seed без Unit 8200**. Израильские киберфонды вас не возьмут как «ещё один AI-SPM». Американские/европейские seed (YC, Ballistic, Work-Bench, Boldstart, академический путь Lakera/Invariant, Protect AI как Seattle-не-8200) покупают (1) работающий контроль на живом рантайме, (2) research/OSS, который цитируют, (3) design-партнёра из AI-native, не логотип банка. Без этих трёх — нет.

Где раздавят: если слайд называется «AI-SPM» или «останавливаем prompt injection лучше Lakera». Там бюджет CISO, логотипы Fortune 500, израильский pedigree, и уже купленные движки внутри Cisco/PANW.

Позиционирование с максимальной конверсией в «да»:
«Control plane для флота tool-using агентов. Не firewall модели. Не IAM. Не трейсы. Инвентарь MCP/агентов → политика → kill-switch / бюджет / human gate → неизменяемый журнал. Dogfood — наш собственный мультимашинный прод. OSS-сканер как клин, диспетчерская как деньги.»

5 proof-points, без которых seed в этой категории почти не конвертится:
1. Свой прод-флот: N машин, M агентов, kill-switch срабатывал не в демо, бюджет реально резал расход, журнал читается человеком.
2. 2–3 design-партнёра с еженедельным использованием; лучше платный пилот $15–40k, чем 20 «interested».
3. Публичный артефакт класса MCP/agent (OSS-сканер, класс уязвимости, CVE, бенчмарк), который пересылают CISO/platform leads. Звёзды GitHub — прокси, не замена.
4. Цифры контроля: time-to-kill, доля перехваченных опасных tool-call, $ сэкономленные vs unconstrained агент, false-positive на dev-скорости.
5. Одна фраза non-overlap vs Prisma AIRS / Cisco AI Defense / Zenity / Entra Agent ID: «они закрывают модель и SaaS-копилоты в одном облаке; мы — смешанный флот на многих машинах».

Уверенность: **medium** по карте 2024–2025 и M&A; **low-medium** по полным спискам seed 2026 и точным post-money. Это не PitchBook-экспорт.

---

B. БЛОК 1. КАРТА КОМПАНИЙ

Сводка раундов (ядро категории + ближайшие). Сумма — equity, не M&A. «Оценка» — valuation, если публично называли.

| Компания | Осн. | Стадия (авг 2026) | Раунды (дата · сумма · лид) | Источник суммы | Что защищают |
|---|---|---|---|---|---|
| Protect AI | 2022 | Куплен PANW (2025) | Seed ~2022 не раскрыто полностью; Series A $35M · дек 2023 · Evolution Equity + Salesforce Ventures | TechCrunch / релиз Protect AI, дек 2023 | AI-SPM, model/supply-chain, Huntr, OSS scanners, LLM Guard |
| Robust Intelligence | 2019 | Куплен Cisco (2024) | Series A $11M · 2020 · Insight; Series B $30M · 2022 · Sequoia; далее не раскрыто | TechCrunch 2020; Sequoia/TechCrunch 2022 | Model firewall, rAI-предикты, AI Defense после сделки |
| Prompt Security | 2023 | Куплен SentinelOne (2025) | Seed, сумма не раскрыта (~низкие миллионы, Cyberstarts/израильские ангелы — по прессе) | Релиз S1 / Calcalist 2025; seed — не раскрыто | Prompt/DLP/gateway для GenAI |
| HiddenLayer | 2022 | Независимый, late A / B? | Seed $6.9–11.5M · 2022; Series A $50M · сен 2023 · M12 | TechCrunch сен 2023 | Adversarial ML, model/API runtime, AI-SPM |
| Lakera | 2023 | A / независимо | Seed/«$10M» · июн 2023 · Citi Ventures, Dropbox, redalpine; последующий A — сумма в первичных источниках этого прогона не подтверждена | Релиз Lakera / TechCrunch июн 2023 | Prompt injection, guardrails, Gandalf |
| Zenity | 2021 | B/C, независимо | Seed $4–5M · 2021; A $16.5M · сен 2022 · Intel Capital; B ~$25M · 2023/24 · Intel/Vertex/GF | Intel Capital / TechCrunch 2022; B — пресс-релизы Zenity | Copilot/agent discovery, SaaS agent posture, policies |
| Aim Security | 2022 | A+, независимо? | Seed (Cyberstarts, не раскрыто); Series A $18M · янв 2024 · Cyberstarts | Calcalist / TechCrunch янв 2024 | GenAI runtime + data; enterprise AI security |
| Lasso Security | 2023 | A | Seed $6M · 2023 · YL Ventures; A — сумма не раскрыта однозначно в этом прогоне | Calcalist/YL 2023 | GenAI visibility, prompt/data, shadow-AI |
| Noma Security | 2023 | A | Stealth/seed 2024 (пресс: низкие–средние миллионы); A 2025 — точная сумма не раскрыта здесь | Релизы Noma / IL-пресса 2024–25 | AI-SPM / enterprise AI posture |
| WitnessAI | 2023 | A | Seed ~$7.5M · 2023; Series A $27.5M · мар 2024 · Ballistic | TechCrunch мар 2024 | Enterprise AI gateway, inspect/control all AI traffic |
| Operant AI | 2023 | Seed/A | Seed ~$10.4–10.5M · 2024 · Ballistic | Релиз Operant / Ballistic 2024 | Runtime (часто eBPF) для AI apps/агентов/API |
| Pillar Security | 2023 | Seed | Seed ~$7–9M · 2024 · израильские киберфонды (Cyberstarts/Team8 — уточнять) | IL-пресса 2024; точный лид не раскрыт здесь | AI AppSec / SDLC агентов и AI-приложений |
| Apex Security | 2023 | Seed/A | Seed, сумма не раскрыта (Cyberstarts-орбита по прессе) | не раскрыто | GenAI security (IL) |
| Knostic | 2023 | Seed | Seed, сумма не раскрыта | не раскрыто | Knowledge-layer: что AI имеет право сказать/достать |
| Straiker | 2024/25 | Seed | Крупный seed 2025; сумма в этом прогоне не подтверждена первичным релизом (пресс-оценки $15–25M — не факт) | не раскрыто как primary | AI-native runtime protection |
| CalypsoAI | 2018 | B+ | Series A/B исторически ~$23M+ · 2021–22; поздние раунды — не раскрыто здесь | Crunchbase-исторические релизы 2021–22 | Model security, red team, gov/DoD |
| Patronus AI | 2023 | A | Series A $17M · 2024 · Lightspeed | TechCrunch 2024 | Eval/guardrails, hallucination, enterprise eval |
| Invariant Labs | 2024 | Research/seed | Институциональный equity-раунд не раскрыт (ETH-спин, гранты/ангелы?) | не раскрыто | MCP security research, mcp-scan, policy-as-code для агентов |
| CodeIntegrity | 2025 | Pre-seed/seed | не раскрыто | не раскрыто | MCP/tool integrity, supply chain агентных инструментов |
| Runlayer | 2025/26 | Pre-seed/seed | не раскрыто | не раскрыто | MCP/runtime слой (gateway/sandbox) |
| Trace (agent-sec) | 2025? | Seed? | не раскрыто; путать с Traceable API Security | не раскрыто | Agent tracing/security (карточка тонкая) |
| Pomerium | 2019 | A (zero-trust, не AI-native) | Исторический A; AI/MCP — продуктовое расширение, не отдельный AI-раунд | старые релизы Pomerium | Identity-aware proxy → MCP/agent access |
| AgentOps | 2023 | Seed | YC + seed, сумма не раскрыта | YC directory; сумма не раскрыто | Observability агентов, не enforcement |
| Astrix Security* | 2021 | B | A $25M · 2023; B 2025 сумма в этом прогоне не подтверждена | TechCrunch/релиз 2023 | NHI / agent identity (смежная полка) |
| Oasis Security* | 2023 | B | A ~$40M · 2024 · Sequoia+Cyberstarts; B 2025 — сумма не подтверждена здесь | релизы Oasis / Sequoia 2024 | Identity, NHI, machine/agent accounts |
| Aembit* | 2021/22 | A | A ~$16.5M · 2023; далее не раскрыто здесь | релиз Aembit 2023 | Workload/NHI IAM |
| Token Security* | 2023 | A | A, точная сумма не раскрыта здесь | IL-пресса | NHI |
| Entro* | 2022 | A | A ~$18M · 2023/24 | релиз Entro | Secrets + NHI |
| DeepKeep | 2021 | Seed/A | Раунды ELRON/IL, точные суммы не раскрыты здесь | не раскрыто | Model robustness, adversarial |
| Guardrails AI | 2023 | Seed | Seed, сумма не раскрыта | не раскрыто | OSS guardrails + cloud |
| Giskard | 2021/22 | Seed | YC + EU seed, сумма не раскрыта | YC | LLM testing/red team |
| Preamble | 2020/21 | Seed | Seed, сумма не раскрыта | не раскрыто | Prompt firewall |
| Virtue AI | 2024/25 | Seed | Сумма не раскрыта здесь | не раскрыто | Automated LLM red team |
| Haize Labs | 2024 | Seed | Seed, сумма не раскрыта | не раскрыто | Jailbreak/red team |
| NeuralTrust | 2024/25 | Seed | EU seed, сумма не раскрыта | не раскрыто | LLM firewall (ES) |
| Mindgard | 2022 | Seed | UK seed, сумма не раскрыта однозначно | не раскрыто | Adversarial ML |
| Enkrypt AI | 2023 | Seed | Seed, сумма не раскрыта | не раскрыто | AI security/alignment tooling |
| Credo AI | 2020 | B | B $21M · 2024 (ориентир прессы) | TechCrunch/релиз Credo 2024 | Governance/GRC, не runtime kill-switch |
| Holistic AI | 2020 | A/B | UK, точные поздние суммы не раскрыты здесь | не раскрыто | AI audit/governance |
| WhyLabs | 2019 | A | A $10M · 2021 + follow-ons не полные | TechCrunch 2021 | Observability, LangKit |
| Arize | 2020 | B/C | B $38M · 2022; поздний рост Phoenix OSS | TechCrunch 2022 | LLM obs / eval |
| Portkey | 2023 | Seed/A | Seed, сумма не раскрыта | не раскрыто | AI gateway + keys + basic guardrails |
| Solo.io | 2017 | Late (API gateway) | Не AI-security раунд; Gloo AI Gateway — продукт | исторические раунды Solo | MCP/AI gateway (инфра, не CISO-стартап) |
| Kong | 2017 | Late | AI Gateway — фича платформы | — | API/AI gateway |
| Cloudflare | — | Публичная | AI Gateway / MCP на Workers | — | Edge control для AI/MCP |

*NHI включены, потому что «IAM для агентов» — соседняя полка, которую инвесторы часто кладут в ту же корзину. Это не значит, что они делают диспетчерскую флота.

Ниже — досье по запрошенному списку и по тому, чего в списке нет, но без этого карта лжёт.

---

B1. Protect AI (сейчас Palo Alto Prisma AIRS / Protect AI)

Год: 2022, Сиэтл. Фаундеры: Daryan Dehghanpisheh (NVIDIA/Intel-орбита) и команда ML+security, **не** Unit 8200 — важный прецедент «US, не-израильский pedigree → крупный A → exit в платформу». Что защищали: полный AI-SPM стек (модель в реестре, supply chain, ModelScan, LLM Guard, Rebuff), багбаунти Huntr.dev, политики на ML pipeline. Стадия: acquired. Раунды: Series A **$35M, декабрь 2023**, лид Evolution Equity Partners, Salesforce Ventures (TechCrunch / PR Protect AI, дек 2023). Seed полностью не раскрыт. Оценка на A: не раскрыто. M&A: Palo Alto Networks объявила покупку **апрель 2025** (релиз PANW, апрель 2025). Сумма официально **не раскрыта**. Оценки прессы «сотни миллионов USD» — не primary. Клиенты до сделки: enterprise/ML-platform, точные логотипы в этом прогоне не сверены. Команда на сделке: ориентир ~80–150, точно не раскрыто. Значение: OSS-дистрибуция (Huntr, scanners) + enterprise SPM съедены платформой №1 в network/cyber. Новая компания с слайдом «AI-SPM» продаёт против Prisma.

B2. Robust Intelligence → Cisco AI Defense

Год: 2019, SF. Фаундеры: Yaron Singer (профессор Harvard CS, adversarial ML), Kojin Oshiba — **исследовательский pedigree**, не 8200. Что: firewall/validation моделей, «можно ли эту модель пускать в прод», score рисков, позже — защита LLM apps. Раунды: A **$11M, 2020, Insight Partners** (TechCrunch 2020); B **$30M, 2022, Sequoia** (TechCrunch/Sequoia 2022). Поздние раунды не раскрыты. M&A: Cisco объявила покупку **26 августа 2024** (блог Cisco / TechCrunch, 26.08.2024). Сумма **не раскрыта**. Продукт живёт как Cisco AI Defense / часть security business. Клиенты: крупные enterprise, банки/tech — конкретный список после сделки не раскрывается. Команда: десятки–низкие сотни, не раскрыто. Значение: «защита одной модели/приложения» теперь фича Cisco. Seed-стартап с SoTA-гардрайлами без уникального канала не нужен.

B3. Prompt Security → SentinelOne

Год: 2023, Израиль. Фаундеры: израильский security (кибер-орбита / Cyberstarts-модель). Точные CV в этом прогоне не сверёны — типичный IL GenAI-security. Что: прокси/гейт для ChatGPT/Copilot, DLP, prompt injection, shadow AI. Seed: сумма **не раскрыта**. M&A: SentinelOne, **начало/1кв 2025** (релиз SentinelOne; точный день без выгрузки не фиксирую). Сумма **не раскрыта**. Клиенты: IL/US enterprise, не раскрыты полно. Команда: seed-размер на сделке (десятки), не раскрыто. Значение: «GPT-прокси для CISO» консолидирован в EDR/XDR. Эта полка закрыта.

B4. HiddenLayer

Год: 2022, Остин / UK. Фаундеры: Chris Hahn (CEO), David Stewart — коммерческий security + adversarial ML, западный, не 8200. Что: защита моделей в runtime (атаки на API/модель, theft, adversarial), позже AI-SPM. Раунды: seed 2022 **$6.9M или $11.5M** (в прессе плавают обе; без Crunchbase не сшиваю) — пометка: расхождение источников. Series A **$50M, сентябрь 2023, лид M12 (Microsoft)**, Capital One, IBM Ventures (TechCrunch, сен 2023). Оценка: не раскрыто. Поздний B 2025–26: в этом прогоне **не подтверждён**. Клиенты: financial, tech, упоминались крупные US enterprise в маркетинге. Команда: ориентир 80–120 после A, не раскрыто. Близость к нам: низкая (модель, не флот). Сильный конкурент за ум «серьёзная AI security компания» в слайде инвестора.

B5. Lakera

Год: 2023, Цюрих. Фаундеры: David Haber и команда ETH/ML (Mateo Rojas-Carulla — DeepMind-орбита по био). **Исследователи, не 8200**. Что: prompt injection, moderation, Gandalf ( recon-бренд), Guard на LLM. Раунд: **~$10M, июнь 2023**, Citi Ventures, Dropbox, redalpine (релиз Lakera / TechCrunch, июн 2023). Поздний A: в маркетинге мелькает, **сумма здесь не подтверждена**. Клиенты: European enterprise + product teams; топ-логотипы — проверять на сайте, в этом прогоне не фиксирую как факт. Команда: ориентир 30–70. Близость к нам: конкурируют за «безопасность агента» только в слое входа в модель. У них нет флота, MCP-диспетчерской, бюджетов. OSS/Gandalf — референс GTM, который вам нужно копировать по *механике*, не по категории.

B6. Zenity

Год: 2021, Израиль. Фаундеры: Ben Kliger (CEO), Michael Bargury (CTO, Microsoft security research, Power Platform / Copilot-атаки — публичный research-бренд). Не обязательно 8200; это **big-tech security research + IL GTM**. Что начинали: low-code/no-code security. Куда приехали 2024–26: **enterprise copilot/agent posture** — найти всех агентов в M365/Salesforce/ServiceNow, политики, misconfig. Раунды: seed ~$4–5M (2021); **A $16.5M, сен 2022, Intel Capital** (TechCrunch/Intel, сен 2022); **B порядка $25M, 2023–24**, Intel Capital / Vertex / Grove (релизы Zenity; точную дату B без выгрузки не бью). Поздний C: **не подтверждён здесь**. Клиенты: крупный enterprise, Microsoft-экосистема. Команда: 80–150 ориентир. **Ближайший «взрослый» конкурент по языку governance агентов**, но объект другой: SaaS-копилоты предприятия, не ваш мультимашинный coding-флот. У них нет kill-switch вашего Claude-агента на ноутбуке сотрудника.

B7. Aim Security

Год: 2022, Израиль. Фаундеры: израильский cyber (Cyberstarts-паттерн; Unit 8200 вероятен, без персональной верификации не ставлю как факт). Что: платформа безопасности GenAI — runtime, данные, политики, enterprise. Раунд: **Series A $18M, январь 2024, лид Cyberstarts** (Calcalist/TechCrunch, янв 2024). Seed не раскрыт. Оценка не раскрыта. M&A на авг 2026: в этом прогоне **не подтверждаю**. Слухи рынка не есть сделка. Клиенты: IL/US enterprise, конкретные логотипы — маркетинг. Команда: после A ориентир 40–80. Полка: CISO GenAI platform. Лоб в лоб с вами только если вы назовётесь «AI security platform».

B8. Lasso Security

Год: 2023, Израиль. Фаундеры: Liav Caspi и команда IL cyber (YL-паттерн). Что: видимость GenAI, shadow AI, контроль промптов/данных. Раунд: **seed $6M, 2023, YL Ventures** (Calcalist / YL, 2023). Series A: факт роста в прессе есть, **сумма не раскрыта здесь**. Клиенты: не раскрыты полно. Команда: seed/A, десятки. Полка: сосед Zenity/Aim, легче, больше visibility чем control plane.

B9. Noma Security

Год: 2023, Израиль. Фаундерский якорь: Nir Gaist (серийный, Reblaze → Check Point) — **IL cyber entrepreneur, не academic**. Что: AI-SPM — находить AI-активы, риски, posture. Выход из stealth 2024. Seed 2024: пресса писала о миллионах, **точная сумма не раскрыта здесь**. A в 2025: широко обсуждался, **цифру в primary не ставлю**. Клиенты: enterprise security buyers. Команда: не раскрыто. Полка: прямое попадание в «AI-SPM». Вам туда нельзя.

B10. WitnessAI

Год: 2023, США. Фаундеры: US enterprise-security (Netskope/gateway-поколение; имена в этом прогоне не фиксирую без сверки). Что: **все AI-взаимодействия через их шлюз** — inspect, block, политика. Это control point трафика, не флот. Раунды: seed ~$7.5M (2023); **A $27.5M, март 2024, Ballistic Ventures** (TechCrunch, мар 2024). Поздние: не раскрыто здесь. Клиенты: US enterprise, дизайн-партнёры Ballistic-сети. Команда: 40–80 ориентир. Близость: **высокая по слову «control»**, низкая по объекту (HTTP-шлюз к ChatGPT/API, не оркестрация агентов по машинам). У них нет A2A, нет бюджета на tool-call, нет мультихостового kill-switch. Это CASB для AI. Вы — диспетчерская флота.

B11. Operant AI

Год: 2023, США. Фаундеры: cloud/runtime security (не 8200). Что: runtime-видимость и политика на приложение/агент, часто через eBPF/инструментацию — tool calls, данные, аномалии. Раунд: **seed ~$10.4–10.5M, 2024, Ballistic Ventures** (релиз Operant/Ballistic 2024). Далее не раскрыто. Клиенты: cloud-native. Команда: 20–50. Близость: **высокая по runtime**, низкая по fleet governance (бюджеты, human gates, MCP inventory как продукт). Технически ближайший западный runtime-игрок.

B12. Pillar Security

Год: 2023, Израиль. Фаундеры: IL AppSec/AI (имена без сверки не выдумываю). Что: безопасность AI-приложений и агентов в SDLC — уязвимости агентов, tool poisoning, цепочка сборки. Seed **~$7–9M, 2024**, лид из IL cyber-фондов (Cyberstarts/Team8 — в прессе мелькает, **не ставлю как доказанный лид**). Клиенты: не раскрыто. Команда: 15–40. Близость: audit/red-team клин похож на ваш «аудит MCP», но они продают AppSec, не диспетчерскую.

B13. Apex Security

Год: ~2023, Израиль. Фаундеры: IL cyber, Cyberstarts-орбита. Что: GenAI security platform (узкий публичный след, легко путать с другими Apex). Раунды: **не раскрыто**. Клиенты: не раскрыто. Команда: не раскрыто. В слайд «игроки» ставить можно, в таблицу денег — только как «сумма не раскрыта».

B14. Knostic

Год: 2023. Фаундеры: security/identity вокруг «knowledge permissions» (Sagi Sheinfeld в публичном поле). Что: **не prompt firewall**. Контроль, какие знания корпоративный AI может отдать какому пользователю — authorization на ответы RAG. Seed: **сумма не раскрыта**. Это сосед вашего knowledge-слоя. Не runtime флота. Дифференциация: они режут *ответы*, вы режете *действия агента*. Можно партнёр, не лобовой конкурент.

B15. Straiker

Год: 2024–2025, США (security-veteran founding — в маркетинге). Что: AI-native защита AI-приложений/агентов в runtime. Раунд: крупный seed 2025 по прессе; **сумма, лид, оценка — не раскрыто как primary в этом прогоне**. Не тащите в слайд «$21M Sequoia» без проверки — эта цифра гуляет как слух. Клиенты: не раскрыто. Команда: seed 15–40. Следить: если лид top-tier — это прямой конкурент за тот же чек, что вы хотите.

B16. Invariant Labs (ETH Zürich)

Год: 2024. Фаундеры/ядро: группа Martin Vechev (SRI Lab ETH) — Luca Beurer-Kellner, Mark Vero, Francis Engelmann и др. **Чисто исследовательский pedigree**. Что: политика для LLM/агентов (Invariant), **mcp-scan**, исследования tool poisoning / MCP rug pull / «whatsapp-MCP» класса, которые фактически создали публичную повестку MCP security в 2025. Equity-раунд: **не раскрыто**. Может быть гранты, ангелы, коммерциализация scan. Клиенты: скорее OSS-пользователи и research-цитирование, не ACV. Команда: лабораторный размер. **Самый важный OSS-компас для вашего клина.** Если у них нет runtime control plane (а публично его нет как enterprise-продукта), это ваша дыра + их бренд research. Риск: их купит Lakera/Cisco/PANW за бренд MCP.

B17. CodeIntegrity

Год: 2025 (по рынку MCP). Фаундеры: не раскрыто здесь. Что по названию и категории: целостность кода/инструментов агента, MCP-серверы, supply chain. Раунды: **не раскрыто**. Клиенты: не раскрыто. Команда: не раскрыто. В карту 2026 ставить как **MCP-native seed, тонкий публичный след**. Это как раз «новое, чего не было в 2024 списке». Не раздувать.

B18. Runlayer

Год: 2025–2026. Что: слой между агентом и MCP/tools — sandbox, allowlist, runtime. Раунды/фаундеры/клиенты: **не раскрыто**. Это прямой product-shape конкурент вашего runtime, скорее всего ранний. Проверять перед слайдом инвестору (сайт, YC batch, LinkedIn headcount).

B19. Trace

Неоднозначность: Traceable AI = API security (другая категория, исторические раунды + возможный M&A-шум). Отдельный agent-security «Trace» 2025–26 — публичный след тонкий. **Не сливать в одну карточку.** Если в вашем пайплайне встретится Trace — верифицировать legal name. Здесь: **данных недостаточно**.

B20. Pomerium

Год: 2019, zero-trust access (identity-aware proxy). Фаундеры: infra/security, не AI-lab. Раунды: исторический Series A (сумму AI-раундом считать нельзя). 2025–26: MCP/agent identity — «кто этот агент, к какому MCP ему можно». Клиенты: существующие ZTNA. Команда: устоявшийся product company. Близость: **identity/access для MCP**, не audit+бюджет+журнал флота. Не позиционироваться как «IAM для MCP» — они и Entra уже там.

B21. AgentOps-класс (observability, не security)

AgentOps (YC), LangSmith (LangChain), Langfuse (OSS, Berlin), Helicone, Arize Phoenix, Braintrust, Galileo, W&B Weave, Datadog LLM Obs, TruLens (TruEra → Snowflake), Keywords AI, LangWatch, OpenLLMetry/Traceloop. Деньги: LangChain сам по себе крупный (A $25M 2023, последующий рост 2025 — точную позднюю цифру здесь не бью); Langfuse — OSS+cloud, раунды скромнее, EU. Что есть: трейсы, evals, cost. Чего нет: **принуждение** (kill, gate, budget as policy). Ваш журнал может *выглядеть* как они; ваш клин — *остановка и разрешение*. Не называйте себя AgentOps, иначе попадёте в кучу obs и будете дешевым.

B22. Новые и смежные 2025–2026, которых не было в вашем списке (обязательны)

- **Microsoft Entra Agent ID / Agent 365** (2025): hyperscaler IAM+inventory агентов. Бесплатно убивает стартап «мы — Okta для агентов».
- **ServiceNow AI Control Tower**: язык «диспетчерская» уже занят у CIO. Объект — их workflow-агенты.
- **Salesforce Agentforce Command Center**, **Google Agentspace / ADK governance**, **AWS Bedrock AgentCore + Guardrails**.
- **Solo.io Gloo AI Gateway, Kong AI Gateway, Cloudflare MCP/AI Gateway**: control plane с другой стороны (API platform, не CISO).
- **Astrix, Oasis, Aembit, Token, Entro, SPIRL, Descope, WorkOS**: NHI/agent credentials.
- **Socket, Snyk, GitGuardian, Docker MCP Catalog**: supply chain MCP-пакетов.
- **Virtue AI, Haize, SplxAI, Mindgard, Giskard, Patronus, Gray Swan**: red-team/eval.
- **Credo, Holistic, IBM watsonx.governance, Saidot**: GRC, бумажный контур.
- **XBOW, 7AI, Dropzone, Prophet**: это агенты-*для*-секьюрити. В карту категории не класть — инвестор смешает и испортит TAM.
- **DeepKeep, CalypsoAI, TrojAI**: старая «model robustness» волна 2021–23.
- **Portkey / LiteLLM / TrueFoundry**: ключи, роутинг, слабые гардрайлы.

Размер команд (если не раскрыт): pre-seed 4–10; seed 8–25; A 30–80; B 80–180; после M&A растворяются в BU Cisco/PANW/S1.

Названные клиенты почти у всех на seed — NDA. Публичные логотипы появляются на A и часто врут (pilot ≠ prod). Не копируйте чужие логотипы в свою колоку без проверки.

---

C. БЛОК 2. ДЕНЬГИ КАТЕГОРИИ

C1. Методика (чтобы не соврать)

«Сколько всего зашло в agent-security» как строка PitchBook — фикция. В 2024 пресса писала «AI security», в 2025 — «agentic security», и туда же кидали: (i) firewall модели, (ii) DSPM для AI-данных, (iii) NHI, (iv) AI-SOC. Я разделяю:

- Ядро: продукт *про защиту AI/агентов/MCP* как primary.
- Смежное: NHI, AI gateway, eval/red-team.
- Не включать: Wiz $32B, AI-SOC, Copilot-стартапы.

Цифры ниже = **пол раскрытых именных equity-раундов, которые я готов назвать**, плюс порядок M&A. Это не рынок TAM.

C2. Equity по годам (ядро + тесно смежное)

**2024 (пол видимых раундов):** Aim A $18M + Witness A $27.5M + Operant seed ~$10.5M + Patronus A $17M + Lasso seed $6M (если считать 2023/24 границей — seed 2023) + Pillar seed ~$8M + Noma seed (не раскрыто) + Oasis A ~$40M (NHI) + части HiddenLayer не в 2024 + Zenity B ~$25M + мелкие EU/US seeds (Lakera follow, Giskard, Enkrypt, Knostic, Preamble, DeepKeep follow) ≈ **видимый пол $150–250M**. Реалистичный потолок категории «AI app/agent security + NHI» с недораскрытым: **$400–700M**. Уверенность: medium на пол, low на потолок.

**2025:** меньше «первых A» в prompt-firewall (полка закрывается M&A), больше B у identity и крупные seed AI-native runtime (Straiker и когорта) + Noma A + возможные HiddenLayer/Zenity follow-ons + MCP seeds. Видимый пол без M&A: **$200–400M**. С NHI-B (Oasis/Astrix, если закрылись) может уехать выше **$500M–1B**. Точный тотал **не раскрыт**. M&A в 2025 отдельно (ниже) — по объёму больше, чем VC.

**2026 (янв–авг):** характер волны — **много мелких MCP-native seed ($2–12M)**, мало новых mega-A в «AI-SPM» (покупатель уже у платформ). Оценка equity **$150–400M YTD** — гипотеза, не таблица. Крупных раскрытых якорных раундов уровня HiddenLayer $50M в ядре agent-sec за первую половину 2026 в этой компиляции **не фиксирую**. Если они были — это дыра прогона без Crunchbase.

C3. Крупнейшие события (не все — equity)

1. HiddenLayer A $50M (сен 2023, влияние на 2024 narrative) — M12. Источник: TechCrunch сен 2023.
2. Oasis A ~$40M (2024) — Sequoia + Cyberstarts. Источник: релизы Oasis/Sequoia 2024.
3. WitnessAI A $27.5M (мар 2024) — Ballistic. TechCrunch мар 2024.
4. Zenity B ~$25M (2023/24) — Intel Capital et al.
5. Astrix A $25M (2023) — NHI. Релизы 2023.
6. Aim A $18M (янв 2024) — Cyberstarts.
7. Patronus A $17M (2024) — Lightspeed.
8. Operant seed ~$10.5M (2024) — Ballistic.
9. Protect AI A $35M (дек 2023) — Evolution Equity.

Крупнее всех equity в «широком AI security» 2024–25 на самом деле **не стартапы ядра**, а платформенные истории (Wiz, масштабы Crowdstrike/PANW R&D). Их в сумму агентской категории не класть.

C4. Все M&A, которые двигают полку

| Цель | Покупатель | Дата анонса | Сумма | Источник | Что это значит |
|---|---|---|---|---|---|
| Robust Intelligence | Cisco | 26.08.2024 | не раскрыто | Cisco blog, TechCrunch 26.08.2024 | Model/app firewall стал Cisco AI Defense |
| Protect AI | Palo Alto Networks | апр 2025 | не раскрыто (пресс-оценки «сотни млн» ≠ факт) | релиз PANW апр 2025 | AI-SPM + Huntr + OSS scanners внутри Prisma AIRS |
| Prompt Security | SentinelOne | 1кв 2025 | не раскрыто | релиз SentinelOne 2025 | GenAI gateway/DLP внутри XDR |
| TruEra (TruLens) | Snowflake | 2024 | не раскрыто | релиз Snowflake 2024 | Eval/obs, не security, но журнал/качество |
| Adept | Amazon | 2024 | не раскрыто | пресса 2024 | Агенты как продукт, не security |
| Wiz | Google | 2025 | ~$32B | Google/Wiz 2025 | AI-SPM как *фича* CNAPP; искажает TAM, не агент-класс |
| Cider (AppSec, 2022) | PANW | 2022 | не этот цикл | — | Не класть в 2024–26 agent-sec |

Aim Security → ? **сделка в этом прогоне не подтверждена.**  
Lasso / Noma / Zenity / HiddenLayer / Lakera / Witness / Operant — **независимы по состоянию этой компиляции**, если не вышел анонс после среза знаний.

C5. Что скупка значит для новых игроков

Платформы купили **галочку CISO 2024 года**: «у нас есть AI security». Бюджет крупного банка на «ещё один AI firewall» в 2026 почти мёртв: его закроют бандлом PANW/Cisco/S1/Microsoft. Окно для новой компании:

- 18–36 месяцев, пока купленное жуётся в SKU и не умеет новый объект (MCP-флот, A2A, multi-host);
- новый покупатель (Head of AI Platform, не CISO);
- новый job-to-be-done (остановить *действие флота*, не промпт).

Это классика AppSec после покупки Snyk-конкурентов крупными: новые деньги уходят в runtime/supply-chain следующего субстрата. Субстрат 2026 — агенты и MCP.

---

D. БЛОК 3. СЕГМЕНТАЦИЯ: ГДЕ ТЕСНО, ГДЕ ПУСТО

Зрелость: 1 пусто/research → 5 платформенный бандл.

| Суб-ниша | Игроки (порядок) | Деньги | Зрелость | Теснота |
|---|---|---|---|---|
| 1. Prompt injection / LLM firewall | Lakera, Cisco AI Defense (ex-Robust), PANW, cloud (Azure Content Safety, AWS Bedrock Guardrails, Google Model Armor), NVIDIA NeMo, Prompt-ex, Preamble, NeuralTrust | Высокие (платформы) + ~$50–100M в стартапах ист. | 5 | Тесно до смерти |
| 2. AI-SPM / shadow AI / inventory моделей | Noma, Protect AI/PANW, HiddenLayer, Wiz, Zenity (SaaS), CrowdStrike | Высокие, M&A | 4–5 | Тесно |
| 3. SaaS copilot / low-code agent governance | Zenity (якорь), Microsoft Purview/DPS, ServiceNow | Zenity $40M+ ист. equity | 4 | Тесно в Microsoft-предприятии |
| 4. Enterprise AI traffic gateway / CASB-for-AI | WitnessAI, Prompt→S1, Netskope/Zscaler фичи, Aim | Witness $35M+ | 3–4 | Тесно у CISO |
| 5. Runtime app/agent instrumentation | Operant, Oligo-класс, Straiker, часть HiddenLayer | Operant ~$10M + скрытые seeds | 2–3 | Средне, ещё дышат |
| 6. MCP server security / tool supply chain | Invariant (OSS), CodeIntegrity, Runlayer, Socket/Snyk/Docker, Pillar | Seed, мало раскрытого | 1–2 | **Пусто коммерчески, шумно в research** |
| 7. Red-team / eval / jailbreak | Patronus, Giskard, Virtue, Haize, Mindgard, Calypso, Gray Swan, Huntr (PANW) | $50–150M ист. по классу | 3 | Средне, мало ACV |
| 8. Agent IAM / NHI | Astrix, Oasis, Aembit, Entro, Token, Microsoft Entra Agent ID, Descope, Pomerium | **Самые жирные смежные чеки** ($100M+ класс) | 3–4 | Тесно и хорошо финансировано |
| 9. Observability / traces / cost | AgentOps, LangSmith, Langfuse, Datadog, Arize, Portkey | Много, но это не security-мультипликатор | 4 | Тесно |
| 10. Governance/GRC/paper | Credo, Holistic, IBM, audit firms | Средние | 3 | Тесно в compliance |
| 11. Confidential compute / TEE inference | Opaque, Anjuna, Tinfoil, Fortanix | Отдельная полка | 2–3 | Не ваш клин |
| 12. **Fleet control plane: multi-agent, A2A, multi-host, kill-switch, budgets, approval graph, audit log as enforcement** | Нет чистого лидера. Обрывки: ServiceNow Control Tower (свой мир), Microsoft Agent 365 (свой мир), Witness (трафик), Operant (runtime), Zenity (SaaS inventory), Portkey/Kong/Solo (API gateway), n8n/Temporal (оркестрация без security) | **Почти нет выделенного капитала** | **1** | **ПУСТО** |

(a) Безопасность ОДНОГО агента/модели — **перенаселена**. Все M&A 2024–25 сюда.

(b) Безопасность ФЛОТА (multi-agent, A2A, оркестрация) — **игроков с этим как primary почти нет**.

Кто ближе всех к «диспетчерская для флота с kill-switch, бюджетами, журналом»:

1. **ServiceNow AI Control Tower** — язык 1:1, чужой двор (ITSM-агенты).
2. **Microsoft Agent 365 + Entra Agent ID** — inventory+identity+admin, только их граф.
3. **WitnessAI** — kill на трафике, не на оркестрации.
4. **Zenity** — inventory+policy SaaS-агентов, не multi-host runtime.
5. **Operant / Straiker** — runtime hook, слабый fleet UX (бюджеты, human gates).
6. **Portkey / Kong / Solo / Cloudflare** — ключи, лимиты, маршруты; не approval graph и не MCP-audit как security-продукт.
7. **Invariant** — политики и MCP-scan, нет прод-диспетчерской.
8. **n8n / Temporal / Prefect / Airflow** — оркестрация без security-контракта.

Никто из них не продаёт пакетом: аудит MCP-серверов + runtime kill + per-agent budget + human approval + immutable log + multi-machine. Это и есть пустая клетка.

A2A (Google, апр 2025) и MCP (Anthropic, ноя 2024 → стандарт 2025) создали объект, которого у Zenity в 2022 не было. Капитал не успел занять клетку специализированным Series A.

---

E. БЛОК 4. НАШ КЛИН НА ЭТОЙ КАРТЕ

E1. Ближайшие (не «похожие словами», а похожие job-to-be-done)

| Кто | Насколько близко | Чем профинансированы | Чего у них НЕТ (ваша щель) |
|---|---|---|---|
| Zenity | Высоко по «governance агентов» | ~$45M+ equity, Intel/Vertex | Мультимашинный runtime, MCP-флот, OSS-first, свой прод-флот, бюджеты tool-call |
| WitnessAI | Высоко по «control point» | $27.5M+ A, Ballistic | Объект = HTTP к LLM, не оркестрация агентов; нет knowledge-слоя |
| Operant | Высоко по runtime | ~$10.5M, Ballistic | Fleet UX, audit-as-product, human gates, MCP inventory |
| Invariant Labs | Высоко по MCP-аудиту | Research/не раскрыто | Коммерческий control plane, dogfood флота, GTM enterprise |
| Runlayer / CodeIntegrity | Высоко по MCP runtime/integrity | Seed не раскрыт | Зрелость, бренд, полный контур (budget+gates+log) |
| ServiceNow Control Tower | Высоко по языку «диспетчерская» | Публичная корпорация | Гетерогенный флот вне ServiceNow |
| Microsoft Agent 365 | Высоко по inventory | Microsoft | Всё, что не Entra/M365 |
| Astrix/Oasis/Entra | Средне (identity) | Крупные чеки | Не решают kill/budget/audit действий |
| Lakera/Cisco/PANW | Низко-средне (если вы скажете «security») | Платформы / $50M A | Флот как сущность |
| AgentOps/LangSmith | Низко (журнал без принуждения) | LangChain-масштаб / YC | Enforcement |

E2. Ваша дифференциация — проверить, что это правда, не слайд

Заявленное: живой мультимашинный флот в проде у самих / OSS-first / память и knowledge рядом.

Это реально редко в комбинации:
- IL-игроки: enterprise GTM, закрытый продукт, нет своего агентского флота как собачьей еды.
- US runtime: eBPF/gateway, нет knowledge-слоя.
- Invariant: research-first, нет ops-плоскости.
- Obs: память/трейсы, нет стопов.
- Hyperscalers: нет гетерогенности.

Если knowledge-слой — это RAG-permissions, вы пересекаетесь с Knostic. Держать формулировку: **память флота для политики** (что агент уже делал, какие секреты трогал, какой хост), не «AI DLP на ответы».

E3. Где раздавят

1. Fortune 500 CISO RFP на «AI security platform» — Zenity + PANW + Cisco.
2. «IAM агентов» — Microsoft + Oasis + Astrix.
3. «Лучший классификатор prompt injection» — Lakera + cloud + Cisco.
4. Если GTM = холодный SOC2-enterprise без OSS-клина — у вас нет 8200-машины продаж YL/Cyberstarts.
5. Если продавать через MSSP/канал Cisco — вас положат как feature request.

E4. Позиционирование, которое уходит от лобового столкновения

Не: AI-SPM. AI firewall. Agent IAM. Observability.

Да: **«Control plane / dispatch for mixed agent fleets»**.

Покупатель seed: CTO / Head of AI / platform lead в компании, где уже крутятся Claude Code / Codex / внутренние MCP / несколько ботов на нескольких хостах. Не банк. Банк — серия B, и то как sidecar к PANW.

Анти-колок: «We don’t replace Prisma AIRS or Entra Agent ID. We sit on the fleet that those tools don’t see: cross-model, cross-machine, tool-using agents with budgets and human gates.»

OSS-клин: сканер MCP + политики (Invariant-competitor или integration), не «ещё один Gandalf». Деньги: hosted dispatch (kill, budget, approval, SIEM export).

Риск кофаундера: категория «control plane» легко расползается в платформу на 3 года. Резать MVP до четырёх кнопок: inventory, policy, kill, log. Бюджеты — вторая. Knowledge — мост, не ядро продажи CISO.

---

F. БЛОК 5. КАРТА ИНВЕСТОРОВ

F1. Кибер-спецы

| Фонд | Тезис 2024–26 | Примеры в орбите | Pre-seed/seed без 8200? |
|---|---|---|---|
| YL Ventures | IL cyber → категория-лидер; AI security как следующий AppSec | Lasso | Практически нет. IL + pedigree |
| Cyberstarts | Фабрика IL security CEO; Aim и когорта | Aim, Oasis (часто рядом), Apex-класс | Нет для no-name non-IL |
| Team8 | IL cyber, platform-building | исторический cyber; AI-sec выборочно | Редко non-IL |
| Glilot / Magma / Pitango / Entrée / TLV | IL seed/A cyber | DeepKeep, NHI, AI-sec | Нет как системный путь |
| Ballistic Ventures | «New architecture security», US, runtime/gateway | WitnessAI, Operant; рядом HiddenLayer-тезис | **Да. Лучший кибер-фонд под вас** |
| NightDragon | Late cyber, Dave DeWalt, платформенные чеки | скорее A/B/growth | Seed no-name — нет |
| Merlin / Ballistic-adjacent US cyber | gov/US cyber | Calypso-класс | Иногда, нужен US-buyer |
| S28 / Boldstart / Work-Bench | US seed infra/cyber | тулзы, не 8200 | **Да, seed** |
| Evolution Equity | growth cyber-Europe/US | Protect AI A | Не pre-seed |
| CrowdStrike Falcon Fund / PANW Ventures / Cisco Investments | стратеги | то, что потом купят | Чек возможен, независимость нет |
| M12 | MSFT strategic | HiddenLayer A | Нужна traction + Azure-история |

Публичные тезисы (без URL-выгрузки, по известным линиям):
- Cyberstarts / Gili Golan: каждый новый субстрат (cloud → identity → AI) рожает IL-компанию, которую купит платформа. 2025–26 слова «agentic» в IL-пресса.
- YL: AI как новый endpoint; visibility-first.
- Ballistic: «security must move to the new runtime» — отсюда Witness (traffic) и Operant (eBPF). Их пост/подкасты 2024–25 ближе к вашей полке, чем YL.
- NightDragon / DeWalt: AI security как платформенная война incumbents — они будут в M&A, не в вашем seed.
- a16z: больше про AI apps/infra, чем про cyber-AI; «securing the AI stack» звучит в эссе 2024–25, чеки в чистый AI-SPM редки (XBOW = offensive agents, не ваша полка).
- Insight: Robust Intelligence исторически; thesis «AI in prod needs validation».
- Sequoia: Robust B; Oasis; LangChain-экосистема — они верят в агентов как платформу, security — accompanying.
- Lightspeed: Patronus (eval).
- Intel Capital: Zenity (copilot governance).
- Index / Bessemer: NHI (Astrix-класс), не fleet dispatch.
- YC: Guardrails, Giskard, AgentOps, куча AI-wrapper; **системно дают seed без 8200**, но follow-on в cyber слабый.

F2. Кто реально пишет pre-seed/seed non-IL, non-8200 — с примерами

Доказанные паттерны, не миф:

1. **Protect AI** — Seattle, не 8200 → $35M A (Evolution, Salesforce Ventures) → exit PANW. Заменитель pedigree: OSS (Huntr, ModelScan) + ML-supply-chain timing.
2. **Lakera** — швейцарские исследователи → $10M (Citi, Dropbox, redalpine). Заменитель: Gandalf, paper-quality, EU enterprise.
3. **Robust Intelligence** — Harvard academic → Insight + Sequoia → Cisco. Заменитель: профессор + бенчмарки моделей.
4. **HiddenLayer** — US/UK commercial security, не 8200 → $50M M12.
5. **WitnessAI, Operant** — US, Ballistic seed/A. Заменитель: enterprise-runtime story, не спецслужбы.
6. **Patronus** — ex-Meta, Lightspeed A $17M. Заменитель: FAANG research, не 8200.
7. **CalypsoAI** — US gov/DoD. Заменитель: clearance-buyer.
8. **YC-компании** (Giskard, Guardrails, AgentOps): seed без кибер-фамилии. Потолок часто низкий, если не выйти в research-бренд.

Контрпримеры (вам закрыто как системный путь): Aim, Lasso, Noma, Apex, большая часть IL NHI — Cyberstarts/YL/Team8, фаундеры 8200/81/репатрианты.

Вывод по фондам для вашего рейза: **не слать YL/Cyberstarts первым**. Слать: Ballistic, Work-Bench, Boldstart, YC, EU deep-tech (redalpine-тип), ангелы CISO из Protect-AI/HiddenLayer-сети, a16z seed только если есть вирусный MCP-артефакт, Sequoia/Lightspeed — на A после design partners. Strategic (PANW/Cisco) — только если готовы быть acqui-hire.

F3. Что они пишут про agent security 2025–26 (смысл, не цитата)

Общий консенсус публичных тезисов: агент = новый identity + новый runtime + новый supply chain (tools/MCP). Три чека уходят в три полки (IAM, firewall, SPM). Фраза «control plane for agents» звучит у hyperscalers и ServiceNow, почти не звучит как seed-тезис киберфондов — **значит, либо вы объясняете им новую полку, либо они запихнут вас в SPM и сравнят с Noma**. Вам нужно в мемо явно: «мы не Noma, вот таблица».

---

G. БЛОК 6. ВЕРДИКТ РАЗВЁРНУТЫЙ + PROOF-POINTS

G1. Есть ли место для no-name OSS-first с живым флотом?

Есть, условно, на **pre-seed/seed $2–6M**, не на «мы поднимем $25M A как Aim». Условия:

- Не называться AI-SPM.
- Показать принуждение на своём флоте (не слайды).
- OSS-артефакт, который живёт без вас (сканер, политики, бенч).
- 2 design-партнёра AI-native, не «банк поговорил».
- Покупатель — platform/AI ops. CISO — later, через SIEM-интеграцию, не как first call.

Если этих условий нет — места нет, вы будете «ещё одна AI security» в воронке, где побеждает 8200+Cyberstarts или PANW-бандл.

G2. Пять proof-points, которые эта категория требует на seed

Инвесторы кибер-AI вслух говорят «клиенты», руками проверяют другое.

1. **Production dogfood с цифрами.** Не «мы тоже пользуемся LLM». А: столько-то хостов, столько-то агентов, столько-то tool-call/день, N срабатываний kill-switch, $x не сожжено из-за budget cap, MTTK (mean time to kill) < T секунд. Без этого OSS-first читается как хобби.

2. **Design partners с частотой, не логотипом.** 2–3 команды, у которых агенты уже в проде (devtools, AI-native SaaS, внутренний platform team). Письмо «ready to pilot» < недельный доступ к логам. Платный пилот $15–40k сильнее, чем 10 F500 intro. F500 intro без пилота — анти-сигнал (долгий цикл, вас сравнят с Zenity).

3. **Исследовательский или OSS-кредит, который можно переслать.** CVE/класс атаки MCP, mcp-scan-конкурент с adoption, публичный benchmark (tool poisoning catch rate). Huntr/Invariant задали планку: CISO форвардит PDF, не ваш sales deck. Credit на GitHub stars — гигиена, не proof.

4. **Измеримый контроль vs eval-игрушки.** Precision/recall на опасных действиях (отправка наружу, удаление, расход $, эскалация прав), не «jailbreak % на HarmBench» (это полка Patronus/Lakera). Отдельно: dev-experience — сколько раз политика ложно остановила нормальную работу. Если DX убивает скорость — platform lead не купит.

5. **Карта non-overlap на одном слайде.** Явная таблица vs PANW Prisma AIRS, Cisco AI Defense, Zenity, Witness, Entra Agent ID, Invariant, Operant. Колонка «они / мы / почему вместе, а не вместо». Инвесторы этой категории уже обожглись на дублях 2023–24 (двадцать GPT-прокси). Кто не объясняет, куда не прёт — получает pass.

Чего **не** требуют на seed (и не надо врать): SOC2 Type II, 5 платящих банков, Unit 8200, Gartner Magic Quadrant, $1M ARR. Это A. На seed в новой полке хватает $10–30k pilots + retention 60 дней + график использования.

G3. Позиционирование максимальной конверсии в «да»

Заголовок слайда рынка:
«Agent security 2024–26 split into three crowded shelves (firewall, SPM, NHI) and one empty shelf (fleet control plane). We are the empty shelf.»

Заголовок продукта:
«Kill-switch, budgets, approvals, audit — for mixed agent fleets running across machines. OSS scanner in; dispatch in production.»

Заголовок почему мы:
«We already run the thing enterprises are about to run: a multi-machine agent fleet. The control plane is dogfooded, not imagined.»

Гео/фонд:
Первый чек — US/EU seed с тезисом runtime (Ballistic-класс, YC, Work-Bench), не IL cyber. Второй чек — после 3 пилотов, уже можно звать Intel Capital/Zenity-орбиту или strategic, не раньше.

G4. Честные дыры этой карты (чтобы слайд не соврал)

- Нет живой выгрузки PitchBook: тоталы 2024/25/26 — диапазоны.
- 2026 MCP-seed (CodeIntegrity, Runlayer, Trace, YC-батчи) — следы тонкие, карточки неполные.
- Aim/Noma/Straiker поздние раунды и слухи M&A — не факты.
- Оценки (valuation) почти ни у кого не раскрыты.
- Команды — ориентиры.
- Клиенты — маркетинг.
- Перед инвесторским слайдом нужен один проход по первичным релизам (компания blog + TechCrunch + Calcalist) на каждую цифру, которую вы оставляете на слайде. Что не бьётся — писать «не раскрыто», не оценку прессы.

G5. До → после на вашей формулировке (чтобы не размазать клин)

До (плохо, лобовое): «Мы делаем безопасность AI-агентов и MCP, AI-SPM и runtime guardrails для предприятий.» Вас кладут в одну клетку с Noma/Zenity/PANW.

После (чисто): «Мы делаем диспетчерскую для флота агентов на нескольких машинах: нашли MCP и агентов, включили политики, режем бюджет, требуем человеческое “да” на опасное, пишем журнал. Сканер открытый. Плоскость управления — наша. Собачья еда — наш прод-флот.» Вас кладут в пустую клетку №12.

Делать слайд «карта рынка + почему мы» только из клетки №12. Остальные клетки — «занято / нас туда не надо», не «мы тоже».
