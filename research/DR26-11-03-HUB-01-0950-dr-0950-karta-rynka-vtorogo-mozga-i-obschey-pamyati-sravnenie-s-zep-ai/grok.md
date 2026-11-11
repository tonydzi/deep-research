# DR26-11-03-HUB-01-0950 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

**Карта рынка «второго мозга» и общей памяти (личное / сотрудник / компания / агенты) vs Zep AI**  
**Дата исследования:** ноябрь 2026. Источники 2025–2026 приоритетны; старше помечены. Все нетривиальные утверждения с ссылками на открытые страницы. Если факт не найден — «нет данных». Названия продуктов и компаний на English.

**Scope / Overall confidence:** Medium–High по agent memory и enterprise search (много первичных сравнений и pricing-страниц); Medium по personal second brain и fusion (фрагментировано, мало единого графа); Low по точным независимым бенчмаркам (почти все числа — self-reported или с разными harness).

## Executive Summary
- Zep (на Graphiti) — один из немногих с **bi-temporal Context Graph** (valid_at/invalid_at, противоречия не стираются). Это его главное отличие от Mem0 (vector + ADD-only), Letta (agent-managed MemFS), Cognee (graph pipeline) и hyperscaler memory (AWS AgentCore, Vertex Memory Bank, Microsoft Foundry).  
- Agent memory layer 2025–2026 переполнен: Mem0 (~$24M, 40k–66k★), Hindsight (Vectorize, MIT, embedded Postgres, высокие self-reported LongMemEval), LangMem, MemOS, A-MEM и десятки OSS. Большинство — vector или simple graph; temporal validity редкость.  
- Multi-agent shared memory между разными вендорами (Claude + Codex + Cursor и т.д.) почти полностью в OSS MCP-серверах (OpenMemory от Mem0, basic-memory-подобные, compound-memory, OneMemory). Коммерческих зрелых решений с конфликтами, provenance и правами почти нет.  
- Personal second brain: mix manual (Obsidian + AI, Mem.ai, Tana, Reflect, Capacities) и passive (Screenpipe local, wearables Plaud/Omi/Bee; Limitless/Rewind куплен Meta в дек. 2025, продажи прекращены).  
- Company brain: Glean (knowledge graph + permissions, $7.2B valuation / ~$200M+ ARR по 2026 отчётам), Microsoft 365 Copilot / Work IQ / Graph, Atlassian Rovo (Teamwork Graph), Onyx (бывш. Danswer, OSS), Dust, Sana (Workday), Writer Knowledge Graph и др. Это поиск + ACL, не bi-temporal fact graph как у Zep.  
- **Слияние личного мозга сотрудника + компании в один граф с правами, приватностью и ownership при увольнении:** сильного готового продукта не найдено. Ближе всего Glean (personal graph + enterprise), Shared Brain-подобные эксперименты и DIY (Obsidian + MCP + permissions).  
- Свободные ниши: общая память нескольких ИИ-вендоров у одного человека/команды; второй мозг семьи; федерация личных мозгов сотрудников с экспортом при уходе.

## 1. Память агентов (agent memory layer)

| Продукт | Модель памяти | OSS / лицензия | Деплой | Цена (публично) | Тяга / деньги | Бенчмарки (с оговоркой) |
|---------|---------------|----------------|--------|-----------------|---------------|-------------------------|
| **Zep / Graphiti** | Temporal knowledge graph (bi-temporal edges, valid_at/invalid_at) | Graphiti Apache-2.0; Zep Cloud proprietary | Cloud / BYOK / BYOC (Enterprise); Graphiti self-host | Flex $125/мес (50k credits), Flex Plus $375/мес (200k); Emerging/Enterprise custom (ранее ~$13k/год) | Graphiti ~30k★; Zep позиционирует себя vs Mem0/Letta/Cognee/Supermemory/AWS/Vertex | Self-reported: LoCoMo 94.7%, LongMemEval 90.2%, retrieval ~155–162 ms (gpt-5.4 harness) |
| **Mem0** | Vector (+ optional graph на platform); ADD-only в OSS v3 | Apache-2.0 core; platform proprietary | Cloud + self-host | Free / от ~$19/мес | $24M (окт. 2025, Basis Set + Peak XV и др.); 41k–66k★ (разные даты) | Self-reported высокие на LoCoMo/LongMemEval после Apr 2026 обновления; разные harness |
| **Letta** (ex-MemGPT) | Agent-managed: core blocks + MemFS (файлы, git-like) + archival | Apache-2.0; Cloud proprietary | Self-host + Cloud | Free / Pro ~$20/мес | $10M seed (Felicis, 2024) | Community / paper числа; силён на long-running autonomy |
| **Cognee** | Graph + vector pipeline (ECL), temporal mode | Apache-2.0; Cloud proprietary | Self-host (embedded) + Cloud | Free / $1 / 1M tokens | $1.5M+ | Graph-focused; свои числа |
| **Hindsight** (Vectorize) | Multi-strategy (semantic + BM25 + graph + temporal) + reranker; banks (facts/experiences/observations/mental models) | MIT | One-Docker embedded Postgres; Cloud | Self-host free; hosted — нет данных точных | ~20k★ | Self-reported 94.6% LongMemEval-s, #1 на BEAM (10M tokens) |
| **LangMem** | Vector / semantic + procedural (LangGraph Store) | MIT | Library (ваш infra) | Free | LangChain ecosystem | Слабее standalone в сравнении |
| **MemOS / Memobase / MemMachine / A-MEM / Graphlit / Supermemory** | Vector / graph / summarization / Zettelkasten-like / MCP-first | Разные (Apache/MIT) | Mix | Mix | Supermemory $3M pre-seed; остальные в основном OSS | Разные self-reported; многие проигрывают plain RAG в independent тестах (arXiv 2026) |
| **AWS AgentCore Memory** | Short-term events + long-term strategies (semantic, summary, preference, episodic, custom) | Proprietary | AWS managed | Consumption | GA 2025 | Hyperscaler; TTL, namespaces |
| **Vertex AI Memory Bank** | Extracted facts + consolidation (create/update/delete on contradiction) | Proprietary | Google Cloud | Bundled / consumption | GA/rebrand 2026 | Managed consolidation |
| **Microsoft Foundry / Work IQ** | User profile + chat summary + procedural; Work IQ — continuous semantic from M365 | Proprietary | Azure / M365 | Copilot $30/user/мес + | Build 2026 фокус на shared intelligence | Preview → production |

**Источники:** Zep pricing и Graphiti pages; Hindsight comparison blog; Mem0 funding announcements; multiple 2026 comparison posts (NomadX, Cognee, particula.tech, octamem и др.). Бенчмарки почти всегда self-reported или с разными backbone/judge — не сравнивайте числа напрямую.

## 2. Общая память между агентами (multi-agent / multi-vendor)

- **MCP-серверы памяти:** OpenMemory (Mem0, local-first SQLite + vector, cross-client Claude/Cursor/Windsurf); compound-memory (Markdown + git, shared + private namespaces, compounding confidence); OneMemory / multi-agent-memory-MCP (SQLite, local, graph expansion); basic-memory-подобные.  
- Решают конфликты: в основном last-write-wins, ADD-only, или agent-managed rewrite. Provenance и fine-grained ACL — слабые или отсутствуют.  
- Коммерческих зрелых multi-vendor shared memory (Claude + Codex + Grok + Cursor одновременно с правами) **нет данных** о сильных игроках. Это в основном DIY / OSS MCP.  
- CrewAI имеет shared memory с весами, но это framework-internal.

**Ближе всех:** OpenMemory + Graphiti self-host + custom ACL.

## 3. Личный второй мозг

| Продукт | Авто-память или ручное | Тип | Примечание |
|---------|------------------------|-----|------------|
| **Obsidian + AI plugins / agents** | Ручное + agent-maintained | Local Markdown + graph | Ваш подход (vault + SQLite graph RAG) близок |
| **Mem.ai** | Полу-авто (AI организует) | Cloud notes | AI thought partner |
| **Tana / Reflect / Capacities / Heptabase** | Ручное + AI | Outliner / object / visual | Structured personal |
| **Khoj** | Search + agents | Self-host OSS | Second brain AI |
| **Screenpipe** | Passive (screen + audio local) | Local capture → agent memory | Силён для «помнит за тебя» |
| **Plaud / Omi / Bee** | Passive wearable | Hardware + transcript | Bee → Amazon; Limitless (ex-Rewind) → Meta дек. 2025, продажи прекращены, поддержка ~1 год |
| **Personal.ai / Recall / Limitless** | Mix | Mix | После покупки Limitless — wind-down |

Большинство требуют ручного ведения или хотя бы curation. Полностью automatic «помнит за человека» — Screenpipe + agent + wearables.

## 4. Второй мозг компании

- **Glean:** Enterprise knowledge graph + permissions-aware search (275+ connectors), personal graph layer. Высокий ARR, customer-hosted options.  
- **Microsoft 365 Copilot / Work IQ / Graph:** M365-native + connectors.  
- **Atlassian Rovo:** Teamwork Graph, Jira/Confluence-first.  
- **Onyx** (ex-Danswer): OSS (MIT) enterprise search + RAG, self-host, 50+ connectors, permissions.  
- **Dust, Sana (Workday), Guru, Slack AI, Notion Enterprise, Hebbia, Moveworks, Writer Knowledge Graph:** Mix search + agents + graph over docs.  
- Кто строит именно «граф знаний компании», а не чистый поиск: Glean, Rovo (Teamwork Graph), Writer KG, частично Cognee/Graphiti-подходы. Большинство — hybrid search + ACL.

## 5. Слияние личного и корпоративного

**Прямой ответ:** сильного продукта, где «у каждого сотрудника свой второй мозг, они объединяются в общий мозг компании» с чёткими правами, приватностью и **владением данными при увольнении** (экспорт личного слоя), **нет данных**.  

Ближе всего:  
- Glean (personal graph + enterprise graph).  
- Shared Brain-подобные (оркестрация личных AI seats + shared memory).  
- DIY: Obsidian/Logseq + MCP + permissions + company layer.  
- Microsoft Work IQ + personal Copilot memory (но silos).

Личная память агента и память компании как **два слоя одного графа** с provenance и revocation — редкость. Zep ближе по temporal graph, но это per-user/account, не employee-federation.

## 6. Сравнительная таблица с Zep

| Игрок | Сегмент | Похож на Zep | Отличается |
|-------|---------|--------------|------------|
| Mem0 | Agents | Drop-in memory layer, hybrid search | Vector-first, ADD-only, нет native bi-temporal invalidation |
| Letta | Agents | Persistent agent memory | Agent сам правит MemFS; runtime lock-in; нет temporal graph |
| Cognee | Agents / docs | Graph + vector | Pipeline-oriented, не bi-temporal fact edges как Graphiti |
| Hindsight | Agents | Hybrid retrieval + graph | Embedded Postgres, accuracy focus, слабее native temporal validity |
| Glean | Corporate | Knowledge graph + context | Document/search + ACL, не agent fact memory с valid_at |
| AWS AgentCore / Vertex Memory Bank / Microsoft | Agents (cloud) | Managed long-term extraction | Hyperscaler lock-in, нет bi-temporal graph, разные стратегии |
| OpenMemory / MCP | Multi-agent shared | Shared across clients | Local OSS, слабые конфликты/ACL/provenance |
| Obsidian + graph RAG | Personal / your stack | Local graph memory | Manual/DIY, нет managed temporal + enterprise compliance |

**Zep unique:** bi-temporal Context Graph + hybrid retrieval (<200 ms claims) + Managed/BYOK/BYOC + SOC 2 Type II + HIPAA BAA + Graphiti OSS. Цена выше Mem0/Letta на self-serve.

## 7. Свободные ниши (2026)

1. **Общая память нескольких разных ИИ-вендоров у одного человека/команды** (Claude + Codex + Cursor + Grok из одной папки). Улика: только OSS MCP (OpenMemory и подобные); нет зрелого SaaS с конфликтами, provenance и multi-vendor. Ближе всех: OpenMemory + Graphiti/Mem0 self-host.  
2. **Второй мозг семьи** (shared между членами семьи с правами). Улика: почти нет специализированных; personal tools + shared Notion/Obsidian. Ближе всех: Notion + MCP или DIY.  
3. **Федерация личных мозгов сотрудников** (личный слой → company graph, ownership/export при увольнении). Улика: Glean personal graph есть, но не full federation + temporal + agent-shared. Ближе всех: Glean + custom или ваш подход (Obsidian + graph + permissions).

## Чего в данных нет
- Независимые head-to-head бенчмарки LoCoMo/LongMemEval/DMR с одинаковым harness и judge (почти все self-reported).  
- Точные ARR/клиенты большинства (кроме Glean ~$200M+, Mem0 funding).  
- Зрелый commercial multi-vendor shared memory с production ACL/provenance.  
- Готовый продукт «личный мозг сотрудника + company brain = один граф с revocation при exit».  
- Точные цены Enterprise большинства (contact sales).  
- Свежие (нояб. 2026) независимые аудиты SOC2/HIPAA кроме заявлений вендоров.

## Три вывода, которые меняют действия
1. **Для собеседования в Zep (04.11.2026):** Главный differentiator Zep — bi-temporal Context Graph (valid_at/invalid_at, противоречия сохраняются) + hybrid retrieval + enterprise deployment (BYOC/SOC2/HIPAA). Конкуренты (Mem0, Letta, Cognee, Hindsight, hyperscalers) решают personalization или autonomy, но не native temporal validity на графе. Скажите: «Мы видим, что рынок agent memory переполнен vector/ADD-only; Zep выигрывает там, где факты меняются и нужна audit-able history. Forward Deployed Engineering — это как раз внедрение этого в enterprise data fusion».  
2. **Для вашего продукта (Obsidian vault + SQLite graph RAG + shared 8 машин + multi-agent):** Вы в нише «local-first multi-vendor shared memory + personal/family + evidence-gated». Это **не повторяет** Zep (нет managed bi-temporal SaaS) и **не повторяет** Glean (нет enterprise connectors/ACL). Уникально: multi-machine + multi-vendor agents (Claude/Codex/Grok) из одной папки с детерминированными evidence gates. Не конкурируйте с Zep head-on в temporal SaaS; используйте Graphiti как возможный backend или оставайтесь local.  
3. **Следующий шаг:** На собеседовании спросите Zep про multi-agent / multi-vendor shared memory и про fusion personal+company layers — это их слабое место относительно вашего стека и свободных ниш. Для себя: протестировать Graphiti self-host vs ваш SQLite graph на temporal invalidation и multi-agent writes.

**Ключевые источники (открытые):**  
- https://www.getzep.com/pricing/ и Graphiti page  
- https://hindsight.vectorize.io/blog/2026/08/11/open-source-agent-memory-systems  
- Mem0 funding (TechCrunch/ET/BuiltIn, окт. 2025)  
- Glean comparisons и enterprise context pages  
- Meta acquires Limitless (TechCrunch/Reuters, дек. 2025)  
- Multiple 2026 agent memory comparison posts (NomadX, Cognee, particula, octamem, membase и др.)  
- AWS AgentCore, Vertex Memory Bank, Microsoft Foundry / Work IQ docs и обзоры  

Если нужно углубить конкретный продукт, pricing, GitHub или подготовить talking points для CEO Zep — скажите.

## SOURCES (webSearchResults)
https://hindsight.vectorize.io/blog/2026/08/11/open-source-agent-memory-systems
https://x.com/heydengai/status/2117114445667442858
https://segmentfault.com/a/1190000048319118
https://fourweekmba.com/ai-plain-rag-topped-all-11-agent-memory-systems-in-one-test/
https://membase.com/blog/best-ai-agent-memory-tools-2026
https://juejin.cn/post/7702047226117472307
https://rpabotsworld.com/ai-agent-memory-architecture-mem0-letta-zep-cognee-guide-2026/
https://github.com/Snseam/awesome-agent-memory/blob/main/docs/products-landscape.md
https://www.apolo.us/blog-posts/memory-for-llm-agents-milla-jovovich-and-20-20-hindsight
https://renue.co.jp/posts/ai-agent-memory-mem0-zep-letta-langmem-locomo-benchmark-2026
https://www.michaelnemtsev.com/memoryagents
https://www.graphlit.com/blog/survey-of-ai-agent-memory-frameworks
https://mnemoverse.com/docs/library/ai-memory-solutions-2026-q3
https://devpress.csdn.net/v1/article/detail/160620303
https://openchamber.dev/blog/agent-memory-research/
https://github.com/baryhuang/mcp-openmemory
https://docs.mem0.ai/openmemory/overview
https://mem0.ai/blog/introducing-openmemory-mcp
https://github.com/chinwe/compound-memory
https://github.com/clbhundley/openmemory-multiuser
https://juejin.cn/post/7510055871465259060
https://libraries.io/pypi/openmemory-ai
https://techbytes.app/posts/onememory-self-hosted-shared-memory-for-ai-agents-over-mcp/
https://github.com/jhunter11/multi-agent-memory-MCP
https://huggingface.co/blog/lynn-mikami/open-memory-mcp-server
https://nomadx.ae/blog/mem0-vs-zep-vs-letta-vs-cognee-2026/
https://thegenios.com/blog/open-source-memory-layers-2026
https://fenbs.ai/blog/mem0-alternatives
https://www.cognee.ai/mem0-vs-zep
https://theaiengineer.substack.com/p/cognee-vs-zep-vs-mem0-vs-letta
https://past.dev/blog/mem0-vs-zep-vs-letta
https://www.contextely.com/blog/llm-memory-database-comparison-2026
https://particula.tech/blog/agent-memory-frameworks-tested-mem0-zep-letta-cognee-2026
https://www.contextely.com/blog/best-agent-memory-frameworks-2026
https://coworker.ai/blog/mem0-vs-zep-vs-letta
https://www.thisandthat.chat/blog/ai-second-brain-apps-teams/
https://www.intriq.app/blog/best-second-brain-apps/
https://secondbrainmemory.com/blog/best-second-brain-apps/
https://www.saner.ai/blogs/10-best-second-brain-ai-apps
https://wetheflywheel.com/en/radar/best-second-brain-tools/
https://gethippo.ai/en/blog/best-second-brain-apps-2026
https://locul.ai/blog/best-ai-second-brain-apps
https://www.iwoszapar.com/p/best-ai-second-brain-solutions
https://littlebird.ai/blog/best-second-brain-apps
https://brain-tube.com/compare/best-second-brain-apps-2026
https://www.getzep.com/pricing/
https://renezander.com/guides/zep-vs-graphiti/
https://www.getzep.com/platform/graphiti/
https://www.getzep.com/pricing
https://memoryatlas.dev/frameworks/zep
https://techcrunch.com/2025/12/05/meta-acquires-ai-device-startup-limitless/
https://luci.memories.ai/blog/limitless-pendant-discontinued-alternatives
https://www.reuters.com/business/meta-acquires-ai-wearables-startup-limitless-2025-12-05/
https://legendmemory.ai/blogs/the-archive/limitless-pendant-discontinued-the-best-alternatives-in-2026
https://luci.memories.ai/alternatives/limitless
https://memora.company/blog/top-glean-alternatives-enterprise-ai-search
https://community.atlassian.com/forums/discussion/2839707/atlassian-rovo-vs-glean-ai-how-do-they-compare/p1
https://www.glean.com/compare/glean-vs-copilot
https://www.glean.com/enterprise-context
https://www.beri.net/article/glean-vs-microsoft-365-copilot-vs-dust-enterprise-search-2026
https://theplanettools.ai/tools/glean
https://theaieconomy.substack.com/p/glean-context-layer-enterprise-ai
https://www.cognee.ai/best-ai-knowledge-base-platforms
https://www.bwdisrupt.com/article/mem0-raises-24-mn-from-basis-set-ventures-peak-xv-partners-others-577381
https://www.ssdnodes.com/learn/hindsight-vs-mem0
https://economictimes.indiatimes.com/tech/funding/mem0-raises-24-million-from-basis-set-ventures-peak-xv-ventures-others/articleshow/124896884.cms
https://www.tipranks.com/private-companies/mem0
https://builtin.com/articles/mem0-raises-24m-AI-memory-infrastructure-20251103
https://e-string.com/articles/your-company-brain-shouldnt-live-in-peoples-heads/
https://neuraltrust.ai/blog/how-to-build-a-company-brain
https://brainshared.com/
https://neuraltrust.ai/blog/what-is-a-company-brain
https://www.linkedin.com/pulse/what-company-brain-isnt-anand-nirgudkar-tumlc
https://docs.screenpipe.com/second-brain
https://github.com/khoj-ai/khoj
https://screenpipe.com/blog/second-brain-ai-2026
https://github.com/aristoapp/awesome-second-brain/blob/main/README.md
https://www.youtube.com/watch?v=UQWWqZ30UsA
https://github.com/Mindola-ai/awesome-second-brain
https://ai.miraheze.org/wiki/AI_Second_Brain
https://raw.githubusercontent.com/aristoapp/awesome-second-brain/main/comparisons/chooser.md
https://deepwiki.com/aristoapp/awesome-second-brain
https://raw.githubusercontent.com/aristoapp/awesome-second-brain/main/comparisons/personal-vs-team.md
https://github.com/daneb/ai-system-design-primer/blob/master/docs/deep-dives/3.11-memory.md
https://www.lyzr.ai/blog/ai-agent-memory-systems-comparison/
https://techcommunity.microsoft.com/blog/azure-ai-foundry-blog/remember-retrieve-revalidate-or-forget-architecting-memory-for-production-ai-age/4564392
https://futuretweets.com/bedrock-agentcore-vs-azure-foundry-vs-vertex-agent-runtime-2026/
https://theagentfieldguide.com/research/agent-memory-systems/
https://octamem.com/blog/best-enterprise-ai-memory-platforms
https://www.maximem.ai/blog/best-ai-agent-memory-platforms-2026
https://news.google.com/rss/articles/CBMiO0FVX3lxTE5KeFh1Uy1tbGpySG9BbVAyVUxnbFZYMTA0WW5XbTZoSC0wdHhuUkxOT0xtcXFnRkozQlYw?oc=5
https://buildnextgen.ai/blog/enterprise-agent-platform-lock-in
https://www.youtube.com/watch?v=Sh0Ro00_rpA
https://atlan.com/know/ai-agent/context-layer/writer-knowledge-graph-vs-enterprise-context-layer/
https://faun.dev/toolbox/onyx-vs-sana-agents/
https://dust.tt/blog/onyx-ai-alternatives-enterprise-knowledge-search
https://techcrunch.com/2025/03/12/why-onyx-thinks-its-open-source-solution-will-win-enterprise-search/
https://sanalabs.com/agents-blog/enterprise-ai-agents-workday-sana-guide-2026
https://www.promptquorum.com/power-local-llm/onyx-review
https://onyx.app/alternatives/dust
https://vdf.ai/blog/enterprise-ai-search-tools/
https://www.onyx.app/blog/benchmarking-agentic-rag-on-workplace-questions
https://faun.dev/toolbox/compare-workplace-ai-search-platforms/