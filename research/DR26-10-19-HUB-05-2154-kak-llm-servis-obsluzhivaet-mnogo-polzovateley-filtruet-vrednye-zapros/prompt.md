# DR26-10-19-HUB-05-2154 — the exact prompt sent to every model

# DR26-10-19-HUB-05-2154 — Как LLM-сервис обслуживает тысячи пользователей параллельно, держит вредные запросы и хранит историю/контекст (подкаст как LLM думает, эпизод 2)

Deep research, answer in Russian, cite primary sources (papers, official docs, engineering blogs) with URLs for every factual claim; mark estimates as estimates; say explicitly where sources disagree.

Topic: How a production LLM service (ChatGPT, Claude, Gemini, open-source stacks like vLLM/SGLang/TensorRT-LLM) serves thousands of users in parallel from one set of weights, how it resists harmful requests, and how conversation memory/context actually works. Audience: a software engineer (Python/C++, PhD) new to LLM infrastructure.

Questions:
1. One copy of weights, many users: are weights shared read-only across requests? How do continuous batching (Orca, OSDI 2022), PagedAttention / vLLM (SOSP 2023), prefill vs decode, chunked prefill, disaggregated prefill/decode, tensor/pipeline/expert parallelism, speculative decoding work? Concrete numbers: GPU memory per token of KV cache for a known open model, typical batch sizes, throughput gains reported in the papers.
2. Isolation: can one user's request "break" or contaminate the model for others? Weights are frozen at inference; per-request state is KV cache. Known real incidents of cross-user leakage (e.g. ChatGPT March 2023 Redis bug), prompt-cache timing side channels (papers 2024-2025), batch non-determinism (Thinking Machines "Defeating Nondeterminism in LLM Inference", 2025).
3. Harmful requests: layers of defence: pretraining data filtering, RLHF/Constitutional AI, refusal direction (Arditi et al. 2024), input/output classifiers (Anthropic Constitutional Classifiers 2025, Llama Guard, OpenAI moderation), rate limits and abuse monitoring, ASL-3 deployment measures. What is known about jailbreak success rates before/after classifiers.
4. Memory: are chat APIs stateless (client resends entire message list each turn)? Compare: OpenAI Chat Completions vs Responses API (previous_response_id, store), Anthropic Messages API, Gemini. How do ChatGPT/Claude apps store history server-side; "memory" features (ChatGPT Memory, Claude memory/projects) as retrieval/injection into context, not weight updates.
5. Context: context window limits (current sizes of frontier models, Oct 2026), cost of attention (quadratic) vs KV-cache linear memory, prompt caching (Anthropic, OpenAI, Gemini): prices/discounts and TTLs, "lost in the middle" (Liu et al. 2023) and context rot research, compaction/summarization of long chats, RAG.
6. Correct common misconceptions: "the model learns from my chat in real time", "each user gets their own model", "the model remembers yesterday's conversation by itself".
Deliver: structured report, numbers table with sources, list of 20+ live primary URLs.
