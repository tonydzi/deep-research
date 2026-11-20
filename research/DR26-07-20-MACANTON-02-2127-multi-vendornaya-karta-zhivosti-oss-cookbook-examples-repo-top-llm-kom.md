---
dr_id: DR26-07-20-MACANTON-02-2127
title: "Мульти-вендорная карта живости OSS cookbook/examples-репо топ-LLM-компаний: кто реально ме"
date: 2026-07-20
lang: mixed
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-20-MACANTON-02-2127): Мульти-вендорная карта живости OSS cookbook/examples-репо топ-LLM-компаний: кто реально мержит внешние PR

> Два независимых deep-research прогона (Grok и ChatGPT) проверили и скорректировали ручную карту merge-friendliness ~20 официальных LLM-вендорских OSS-репо на июль 2026, дав per-repo вердикт, гейткиперов, текущие запросы контента и ранжированный TOP-10 конкретных PR-возможностей под ассеты Антона.

## Ключевые выводы
- Оба вендора сходятся: google/adk-python-community — HIGH, проверено мерджем внешнего PR #141 (governance plugin), гейткипер @DeanChensj (Grok) / чуть иначе визибл в scan; openai-agents-python/-js — HIGH, гейткипер @seratch, минуты-часы на мердж узких фиксов, issue #1097 (-js) = approval-with-modified-args точное совпадение с ассетом authority routing.
- Расхождение по Groq: изначальная карта Антона ставила HIGH (быстрый рубрик-мердж), но оба независимых прогона понижают до MED/LOW — 0 мерджей за последние 90 дней (последняя волна ~сен-ноя 2025), при этом публичный тон остаётся welcoming (рубрика Usefulness/Originality/Clarity/Accuracy/Depth/Grammar жива, но не спасает при отсутствии review bandwidth).
- xAI cookbook аналогично понижен с MED-HIGH до LOW/MED: большой backlog closed-unmerged PR, у ChatGPT-прогона 0 мерджей за 90 дней и отсутствие стабильного публичного гейткипера; при этом почти все portable-ассеты Антона (human approval, verifiable reasoning, tracing, MCP) уже заняты открытыми PR других авторов.
- Hugging Face cookbook: Grok держит HIGH (issue #303 'search agent cookbook' всё ещё открыт >1 года, гейткиперы @merveenoyan/@stevhliu), а ChatGPT-прогон понижает до MEDIUM — только 1 мердж за 90 дней несмотря на историческую открытость; более productive target сейчас — почин сломанных рецептов (#316 удалённый smolagents.ManagedAgent, #326).
- Microsoft: repo-приоритет смещается с semantic-kernel (MED-HIGH только для фиксов/безопасности, LOW для новых фич) на microsoft/agent-framework как новый основной 'production successor' Autogen+SK — HIGH, 12k+ звёзд, 197+ контрибьюторов, гейткипер @eavanvalkenburg, открытый живой баг #7212 (compaction/tool-call pairing) точно ложится на ассет persistent memory.
- Обнаружены 'missed doors' (репо, которых не было в исходной карте Антона, но реально мержат внешние PR): microsoft/agent-framework, genkit-ai/genkit (MCP schema/validation issues #4366/#4365 — открытые TODO), agentscope-ai/agentscope (Alibaba-associated, гейткипер @DavdGao, живой HITL-race баг #2123), awslabs/agentcore-samples (Cedar-политики, issue #1587 с готовой реализацией автора), openai/openai-cookbook (селективно, пример Oracle-backed memory PR #2772).
- Copybara/internal-sync паттерн (закрытые PR, реально приземлившиеся внутри) подтверждён только для google-gemini/genai-processors (copybara-service[bot] мержит после review под веткой контрибьютора) и частично для cohere-ai/cohere-developer-experience (one-way internal sync OpenAPI/snippets). НЕ подтверждён для OpenAI, Microsoft, xAI, Groq, Mistral, Qwen-Agent, Together — их 'кладбища' PR реальны, не иллюзия.
- 'Do-not-bother' список согласован между вендорами: autogen (maintenance mode, редиректит на agent-framework), meta-llama/llama-cookbook (нет коммитов с ~ноя 2025), DeepSeek (нет полноценного cookbook-репо, хотя есть боковая дверь deepseek-ai/awesome-deepseek-agent с реальными мерджами), Cohere (архив/staff-only с оговоркой), QwenLM/Qwen-Agent (security-фиксы закрываются без ответа, напр. SSRF-PR #871), mistralai/cookbook (депприоритет из-за ~49 stale внешних PR).
- Кросс-катинг причины отказа PR совпадают у обоих вендоров: отсутствие CLA (Google/MS) или DCO (NVIDIA), пропуск issue-first обсуждения, сторонние pip-зависимости (особенно у Gemini), non-runnable/no-Colab/no-tests, широкий multi-concern PR, низкая оригинальность/промо-тон.
- Оптимальный формат для мерджа — маленькие, узкоспециализированные notebook/PR (не multi-file демо), с live runnable evidence (Colab-ссылки, замеренные выходы), честным AI-authorship дисклоузом и явным кредитом автору issue; подача в середине недели повышает шансы.

## Рекомендации / решения
- Приоритет действий (TOP-10 по обоим прогонам, вероятность мерджа убывает): 1) openai-agents-python/-js — authority routing / approve-with-modification под issue #1097 (оценки 40–85%); 2) adk-python-community — MCP tool-call signing/governance или tamper-evident audit trail под issue #142 (45–80%); 3) microsoft/agent-framework — починка compaction/tool-call pairing в issue #7212 (60–75%, лучший текущий Microsoft-таргет вместо semantic-kernel).
- Дополнительно высокий приоритет: genai-processors contrib/ — bounded backpressure/cancellation в issue #164 (55–70%); huggingface/cookbook — сначала почин сломанного multi-agent рецепта (#316), потом заявка на #303, тегая @merveenoyan/@stevhliu; genkit-ai/genkit — сплит PR на MCP schema conversion (#4366) и argument validation (#4365), не смешивать с signing/policy в одном PR.
- Не открывать конкурирующие PR там, где issue-автор уже заявил 'implementation ready' (agentcore-samples #1587, adk-community #142) — вместо этого предложить коллаборацию/тесты, это резко повышает вероятность мерджа против параллельной реализации.
- Понизить приоритет/не тратить усилия на Groq и xAI cookbook несмотря на изначально высокую оценку в карте Антона — публичная риторика welcoming, но текущий (июль 2026) merge rate практически нулевой; если всё же пробовать — только один сфокусированный notebook с высоким рубрик-скором и после подтверждения maintainer'а о доступной review-bandwidth.
- Для Google-экосистемы (Gemini cookbook, ADK, genai-processors) обязательно issue-first + один Google CLA на все проекты + минимизация сторонних pip-зависимостей — это системная причина отказов, не зависящая от качества самого PR.
- Использовать боковые двери, пропущенные в исходной карте: awslabs/agentcore-samples (authority routing + adversarial verify), agentscope-ai/agentscope (fleet-ops/concurrency через issue #2123), deepseek-ai/awesome-deepseek-agent (лёгкий интеграционный гайд вместо полноценного framework PR).

## Сущности
- **Люди:** @DeanChensj, @seratch, @merveenoyan, @stevhliu, @sergiopaniego, @eavanvalkenburg, @TaoChenOSU, @Giom-V, @markmcd, @kkorpal, @andycandy, @kibergus, @DavdGao, @yuminshen, @psv901, @giskard09, @janzheng, @shaunjoshi, @ozenhati, @billytrend-cohere, @tianyicui, @IgorKasianenko
- **Компании:** Google, Hugging Face, Microsoft, Groq, OpenAI, xAI, Together, Mistral AI, Alibaba (AgentScope), AWS, NVIDIA, Meta, DeepSeek, Cohere, Perplexity
- **Продукты/инструменты:** google/adk-python-community, huggingface/cookbook, microsoft/semantic-kernel, microsoft/agent-framework, groq/groq-api-cookbook, openai/openai-agents-python, openai/openai-agents-js, openai/openai-cookbook, openai/evals, xai-org/xai-cookbook, google-gemini/cookbook, google-gemini/genai-processors, togethercomputer/together-cookbook, mistralai/cookbook, agentscope-ai/agentscope, awslabs/agentcore-samples, aws-samples/amazon-bedrock-samples, genkit-ai/genkit, meta-llama/PurpleLlama, meta-llama/llama-cookbook, meta-llama/llama-models, meta-llama/synthetic-data-kit, QwenLM/Qwen-Agent, deepseek-ai/awesome-deepseek-agent, cohere-ai/cohere-developer-experience, perplexityai/api-cookbook, NVIDIA/GenerativeAIExamples, dashscope/dash-cookbook, microsoft/autogen

## Открытые вопросы
- Полный ответ ChatGPT-прогона обрезан ('...[truncated for length]') на пункте 10 (huggingface/cookbook), не видно продолжения ранжированного TOP-10 и итогового do-not-bother списка этого вендора целиком — сверить с оригинальным файлом при необходимости.
- Нет широкого 'community consensus' корпуса (Reddit/HN/X/Discord) для большинства репо — оценки построены преимущественно на merged-PR истории и maintainer-заявлениях; сила этого сигнала ниже, чем прямых цитат сообщества.
- Не проверено, распространяется ли Copybara-паттерн (внешне закрытые, но внутренне приземлившиеся PR) на другие Google-репо кроме genai-processors — для core ADK и DeepMind-репо это отдельно не подтверждено.
- Авторская принадлежность (external vs vendor-affiliated) части недавних мерджей в together-cookbook, NVIDIA/GenerativeAIExamples и aws-samples/amazon-bedrock-samples осталась 'ambiguous' — статус HIGH/MED для этих репо не полностью доказан.

## Источник
- DR-ID `DR26-07-20-MACANTON-02-2127` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- everything-becomes-content
- multi-vendor-cookbook-radar
- ship-github-no-plus-wait
- dr-numbering-registry
- second-brain-northstar
- cofounder-pirate-voice
- insight-2026-07-16-recovered-lost-alpha — обе заметки независимо приходят к тому же выводу: xai-cookbook мёртв, живая дверь = plugin-marketplace
- insight-DR-DR26-08-11-HUB-02-2212-асимметрия-слияния-pr-почему-mid-size-инфра-oss-ме — прямой предшественник по живости вендорских cookbook-репо (те же 5 вендоров) — эта DR продолжает и адъюдицирует гипотезу
