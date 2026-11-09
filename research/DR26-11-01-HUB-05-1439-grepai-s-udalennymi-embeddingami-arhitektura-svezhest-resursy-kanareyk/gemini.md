# DR26-11-01-HUB-05-1439 — raw report from gemini

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Collector provenance


DR26-11-01-HUB-05-1439. Source conversation https://приватный чат . Verbatim rendered report text below, followed by the21 report source links visibly exposed in the DOM. This is a DOM extraction, not official Markdown export. No researcher text rewritten; collector framing is outside report. Full raw HTML retained separately.

# Raw researcher report starts

Enterprise Operational Analysis: GrepAI Cloud Embeddings & Indexing Architecture
A. Identity and Architecture

The subject of this operational evaluation is the open-source software project definitively identified as grepai. Dictation anomalies previously recorded the artifact as "GroupEye Cloud," which is an erroneous transcription; no product by that name exists in this software category, and no silent substitution has been made. The authoritative repository is located at github.com/yoanbernabeu/grepai, accompanied by official documentation hosted at yoanbernabeu.github.io/grepai/. The software functions as a privacy-first Command Line Interface (CLI) and Model Context Protocol (MCP) server designed to provision AI agents with semantic code search and call-graph analysis capabilities.   

Official Implementation and Source Revision

The implementation is authored entirely in the Go programming language. As of the evaluation date of November 1, 2026, the current stable release is designated as version v1.2.0, which was officially published on October 30, 2026. The precise source revision associated with this stable release is identified by the commit SHA dd38459. The software is distributed under the permissive MIT License, ensuring compatibility with our internal enterprise compliance frameworks for open-source adoption.   

A critical architectural shift occurred recently regarding the runtime dependencies. The compilation of the current source tree from the main branch now requires a minimum runtime of Go 1.25.5, an upward revision necessitated by upstream dependencies such as qdrant/go-client (v1.19.2), pgvector-go, and pgx/v5. The project maintains strict stable and prerelease divergence control by aligning the Continuous Integration (CI) build matrix dynamically with the go-version-file directive inside go.mod, thereby mitigating toolchain drift between pre-release development branches and tagged stable releases. The agent transport layer relies heavily on the github.com/mark3labs/mcp-go dependency, which was aggressively updated from v0.45.0 to v0.58.0 in the v1.2.0 release cycle to ensure compliance with the latest Model Context Protocol specifications.   

Architectural Topology: Separation of Concerns

The fundamental architecture of grepai strictly isolates the generation of vector embeddings from the storage and retrieval of those vectors. This decoupling is the most consequential design paradigm for our operational decision. There is no official "GrepAI Cloud" hosted indexing or search service provided by the vendor; the product is strictly a locally executed application. When configured for remote embeddings, the software transmits data to a third-party cloud provider exclusively for the mathematical transformation of text into high-dimensional floating-point arrays (vectors).   

The actual storage, indexing, similarity search, and reranking mechanisms remain entirely local to the executing machine (or a self-hosted database instance) through three supported storage backends:

GOB (Default): A localized, flat-file architecture utilizing Go's native binary serialization format (.grepai/index.gob and .grepai/symbols.gob). This backend is entirely self-contained within the project directory.   

PostgreSQL: An enterprise-grade relational backend utilizing the pgvector extension, designed for team environments requiring concurrent access.   

Qdrant: A dedicated vector database operating over gRPC, supporting custom corporate hosting or local Docker containerization, featuring Transport Layer Security (TLS) and project-namespaced payload isolation.   

Data Residence and Lifecycle

To satisfy our strict data sovereignty and confidentiality mandates, the exact physical and network location of all data structures must be mapped out accurately:

Original Source Code: Remains immutably on the local file systems of the Windows Hub, Intel Mac Pro, or Apple Silicon Mac Studio. grepai operates strictly as a read-only indexer regarding source files.

Code Chunks: During the ingestion phase, raw source files are loaded into local Random Access Memory (RAM), split into smaller segments (chunks), and transmitted over the network in plaintext to the configured remote embedding provider.   

Vectors: The high-dimensional arrays returned by the remote API are persisted locally into the selected backend (GOB, Postgres, or Qdrant). Remote storage is not implied by remote embeddings.   

Metadata and Call Graphs: File paths, line numbers, caller/callee relationships, and symbol definitions are never sent to the embedding provider. This metadata is extracted locally using static analysis and stored locally in the symbols.gob file or the relational database.   

Queries and Reranking: Natural language search queries submitted by the AI agent are transmitted to the remote embedding provider to generate a prompt vector. The actual similarity search—calculating the cosine distance between the query vector and the stored codebase vectors—and any subsequent reranking are executed purely locally against the local storage backend.   

B. Cloud Path and External Transmissions

The operational directive requires the integration of remote embeddings while strictly avoiding the default behavior of spawning local Ollama or LM Studio instances. This necessitates a comprehensive audit of the supported cloud pathways, secret management protocols, and the financial unit economics associated with external vectorization.

Supported Providers and Configuration Matrices

As currently implemented in the v1.2.0 stable release, grepai supports a specific subset of external embedding providers. The software explicitly supports the official OpenAI API. Furthermore, it supports gateway and routing architectures, specifically OpenRouter (acting as a multi-provider gateway via https://openrouter.ai) and Requesty (a gateway integration via https://router.requesty.ai/v1). There is no explicit implementation or codebase reference to a native "Synthetic" provider; any routing to such a service would require utilizing a custom OpenAI-compatible endpoint configuration.   

The configuration parameters, defined in the .grepai/config.yaml or .grepai/workspace.yaml files, govern the behavior of the network requests. The configuration allows granular control over the embedder.provider, the specific model string, the endpoint URI, and the dimensions of the output vector. A critical enhancement introduced in the v1.1.0 and refined in the v1.2.0 release cycle is the implementation of robust network resilience mechanics. The software now features a Token Bucket rate limiter for proactive Token Per Minute (TPM) management, coupled with exponential backoff and jitter algorithms to automatically recover from HTTP 429 Rate Limit Exceeded errors. Furthermore, administrators can configure the parallelism directive (e.g., executing 8 concurrent requests), alongside explicitly bounded embedder.max_batch_size and embedder.max_batch_tokens to optimize network throughput without overwhelming provider endpoints.   

Secret Handling and Environmental Safeguards

A severe vulnerability regarding API key management was remediated in the v1.2.0 release. Prior to this update, a developer could clone a malicious repository containing a .grepai/config.yaml file that specified a rogue custom endpoint while relying on the host machine's global <key-var> environmental variable. This could result in the silent exfiltration of corporate API keys to unauthenticated, attacker-controlled servers.

The current stable implementation enforces a strict environmental safeguard. If an API key is read from the shell environment (e.g., export <key-var>="..."), grepai is hardcoded to transmit that bearer token exclusively to the official provider endpoint (e.g., https://api.openai.com/v1). If the configuration file specifies a custom endpoint (such as an internal corporate proxy, Azure OpenAI, or a local vLLM instance), the software will refuse to operate unless the api_key is explicitly and deliberately defined within the configuration file itself (e.g., api_key: unused for unauthenticated endpoints, or a specific scoped key). This mechanism ensures that environment secrets are mathematically sealed from accidental transmission to third-party endpoints.   

Data Transmission, Retention, and Privacy

When utilizing remote embeddings, the operational reality is that the actual, plaintext source code chunks are transmitted over the public internet (via HTTPS) to the provider. Therefore, the directive to exclude secrets, cookies, cryptographic keys, and confidential third-party data cannot rely on the remote provider's filtering. It must be enforced locally prior to ingestion. grepai supports hierarchical .grepaiignore files, which supersede standard .gitignore rules, allowing for explicit negation and path exclusion.   

The retention and training deletion policies are entirely dictated by the selected provider and the specific tier of service. Standard chat subscriptions (e.g., ChatGPT Plus, Claude Pro) are fundamentally distinct from API entitlements. It is an operational fallacy to assume that an existing chat subscription provides any allocation of API credits. API usage operates on a strictly metered, pay-as-you-go billing architecture. By default, data submitted to the OpenAI API is not utilized to train foundation models, and payloads are retained for a maximum of 30 days strictly for trust and safety abuse monitoring.

Budget Gates and Cost Formulas

Because chat subscriptions do not cover API usage, initiating a pilot without a pre-funded API account will immediately result in an HTTP 402 Payment Required or HTTP 429 Insufficient Quota error. A zero-new-spend pilot route is only viable if the enterprise currently maintains a funded OpenRouter, Requesty, or OpenAI organizational API account with available balance. If no such account exists, explicit financial authorization must be obtained to deposit a minimum balance (typically $5.00) to clear the budget gate.

To project the exact financial impact, we utilize the recommended text-embedding-3-small model, which provides 1,536 dimensions and offers an optimal balance of latency and quality. The unit cost as of late 2026 is $0.02 per 1,000,000 tokens ($0.00002 per 1,000 tokens).   

Operational Scenario	Estimated Token Volume	Derivation Formula	Projected Cost (USD)
Initial Indexing (100k Code Tokens)	100,000	100,000 * ($0.02 / 1,000,000)	$0.002
Initial Indexing (1M Code Tokens)	1,000,000	1,000,000 * ($0.02 / 1,000,000)	$0.02
Incremental Churn (10% of 1M/day)	100,000 / day	100,000 * ($0.02 / 1,000,000)	$0.002 / day
Agent Queries (1k requests)	~20,000	20,000 * ($0.02 / 1,000,000)	$0.0004
Retry Overhead (Assuming 2% failure)	~20,000	20,000 * ($0.02 / 1,000,000)	$0.0004

Note: The cost calculations represent the absolute minimum metered charges for the API endpoint. They do not account for the network egress costs or the localized computational overhead required to deserialize the storage backend.

C. Search Capabilities and Multilingual Processing

The core value proposition of integrating grepai into the existing CodeIndexer workflow is to augment exact-match lexical discovery with semantic intent resolution.

Lexical, Exact, and Semantic Methodologies

Traditional code search utilizes regular expressions or BM25 algorithms to match exact variable names or syntax patterns. grepai introduces dense vector similarity calculations, allowing an AI agent to query "user authentication flow" and retrieve the handleUserSession function based on conceptual proximity. When the Qdrant backend is deployed, the system supports true Hybrid Search, allowing a single query to push down both a dense vector approximation and a sparse lexical filter simultaneously to the database engine.   

The system incorporates robust duplicate handling. A --path flag allows the AI agent to constrain semantic searches to specific directories, pushing the filter directly into the backend query optimizer. Furthermore, a configurable search.dedup parameter actively collapses multiple relevant chunks originating from the exact same source file into a single, cohesive result, significantly reducing repetitive noise in the agent's context window.   

Multilingual Queries (RU vs. EN)

A critical operational factor is the handling of queries submitted in languages other than English, such as Russian (RU). Because the actual indexing and search process relies on the mathematical properties of the embedding model's latent space, the multilingual quality is entirely model-dependent. The software authors explicitly recommend utilizing English queries because models like nomic-embed-text are predominantly trained on English code comments and documentation. However, by utilizing OpenAI's text-embedding-3-small, the semantic mapping between a Russian query (e.g., "логика аутентификации") and English source code variables (auth_logic) is inherently handled by the foundation model's extensive multilingual training corpus. The CLI does not natively translate the text; it relies on the vectors aligning in the embedding space.   

Chunk Ingestion vs. Symbol Parsing

It is imperative to conceptually separate the two distinct analytical pipelines running inside the software:

Chunk Ingestion (The Vector Path): Source files are passed through a new algorithm introduced in v1.1.0 known as cAST (AST-aware chunking). Unlike rudimentary sliding-window splitters that chop files blindly every 512 tokens, cAST leverages the Abstract Syntax Tree to identify function and class boundaries, ensuring that chunks remain semantically whole. If a perfectly bounded chunk still exceeds the embedding model's maximum context length, a ReChunk() subroutine recursively divides the oversized segment by half and retries up to three times. These chunks are converted to vectors and stored.   

Symbol and Call Parsing (The Metadata Path): Completely independent of the vectorization process, the indexer scans the files to extract defined functions, classes, and their corresponding call sites, storing these structural edges in symbols.gob.   

Language Support and PowerShell Discrepancy

The product documentation frequently advertises extensive multi-language support. A rigorous inspection of the supported languages reveals a critical discrepancy regarding our operational requirements. Python (Py), JavaScript (JS), and TypeScript (TS) are fully supported across both the AST-aware chunking pipeline and the precise symbol extraction pipeline.   

However, PowerShell is not natively supported for AST-based call graph tracing or semantic chunking. While PowerShell is mentioned in the documentation, it is exclusively in the context of the installation script (install.ps1) and the generation of CLI shell auto-completion artifacts. If the operational pilot expects to build deep dependency graphs of enterprise PowerShell modules, this product will fail to deliver; it will default to blind token chunking without syntactic awareness.   

Agent Integration, Transport Lifetimes, and Formatting

Integration with coding agents (Claude Code, Cursor) is executed via the Model Context Protocol (MCP). The software utilizes the stdio transport layer, meaning that the AI agent spawns a child process of the grepai binary, communicates via standard input/output streams, and terminates the process when the context is fulfilled.   

To prevent context window exhaustion, the system offers highly optimized stdout framing.

Output Format	Invocation Flag	Description	Token Efficiency
Standard JSON	--json	Verbose output including full code content fields.	Baseline
Compact JSON	--json --compact	Omits the raw code content, returning only file paths, line numbers, and symbol names.	

~80% reduction


TOON (Token-Oriented)	--toon	Proprietary minimal syntax designed specifically for LLM tokenizers.	

~50% reduction vs JSON


Compact TOON	--toon --compact	Combines the TOON syntax with the omission of content fields.	

Maximum efficiency

  

The RAM Loading Vulnerability: The strict operational constraint mandates "no permanent stdio MCP per session." This requires the MCP server to operate in a cold/warm one-shot behavior pattern. If the default GOB backend is utilized, every single time the agent invokes grepai search, the entire .grepai/index.gob file must be deserialized and loaded into the local machine's RAM to perform the vector calculations. The developer explicitly added a warning in v1.1.0 that GOB indexes exceeding 50,000 chunks will suffer severe latency and memory degradation, prompting a migration to Qdrant. This claim requires immediate local measurement on the Intel Mac Pro and Apple Silicon Mac Studio to determine if the deserialization time exceeds the agent's internal tool timeout thresholds.   

D. Call Graph Analysis vs. Native Language Servers (LSP)

The grepai trace module provides the AI agent with tools (grepai_trace_callers, grepai_trace_callees, grepai_trace_graph) to understand dependency chains. This module operates using two distinct build tags and dependencies.   

Regex (Light Mode) vs. Tree-Sitter (Precise Mode)

Fast Mode: The default execution path utilizes lightweight, pre-compiled regular expressions to heuristically identify function declarations and call sites. It requires no external dependencies and executes rapidly, but it is highly susceptible to false positives (e.g., misidentifying a string literal as a function call).   

Precise Mode: Utilizing the smacker/go-tree-sitter CGO bindings, this mode parses the raw text into a structural Abstract Syntax Tree (AST). This provides exact syntactic boundaries for languages like Python, JS, TS, Java, C#, and F#.   

Crucially, Precise Mode does not participate in the vector indexing process. The high-fidelity AST parsing is utilized exclusively for generating the symbols.gob reference file for the trace commands. The vector chunks are embedded independently.   

The Illusion of Semantic Resolution

A critical operational hazard is conflating the precise syntactic parsing of Tree-Sitter with the deep semantic type resolution provided by a native Language Server Protocol (LSP) such as pyright (Python) or tsserver (TypeScript). Tree-sitter understands grammar; LSP understands meaning, compilation, and memory references.

The grepai trace tooling will structurally fail in several common enterprise paradigms:

Ambiguous Same-Name Symbols: If a TypeScript repository contains a User.authenticate() method and a Device.authenticate() method, Tree-Sitter cannot infer the type of the calling object. A query for callers of authenticate will return a blended union of both, whereas a native LSP uses the type checker to differentiate them.

Dynamic Dispatch and Callbacks: In Python, if a function is passed as a first-class object (def execute_handler(handler): return handler()), the static AST cannot trace the execution edge back to the original function definition.

Decorators and Metaprogramming: Python decorators that dynamically alter function signatures or inject dependencies at runtime are entirely opaque to static AST analysis.

Fair Fixture Protocol for Evaluation:
To objectively measure this deficiency, a source-linked fixture must be constructed containing:

An abstracted Factory Pattern in TypeScript where implementations share method names.

A Python script utilizing getattr(obj, 'method_name')() dynamic invocation.

A Python class utilizing heavily nested @property and @classmethod decorators.
This fixture must be evaluated against the native CodeIndexer (powered by LSP truth) and grepai trace. The independent judge must distinguish between a tool displaying a structurally correct string match versus a tool providing semantically accurate execution pathways. grepai provides a map of the syntax, but correctness is not mathematically guaranteed.

E. Freshness, Mutability, and Concurrency Risks

The operational requirement forbids the deployment of "seven background watchers." Consequently, the standard grepai watch daemon—which relies on continuous filesystem polling to incrementally update the vector index—must be abandoned in favor of one-shot, on-demand indexing.   

Edit, Rename, and Deletion Handling

When operating in a one-shot paradigm (grepai index or triggered via an agent MCP call), the software must reconcile the current state of the repository against the stored index. The software handles mutability through atomic writes and content hashing.

Content Reuse: When a file is modified, the chunks are re-hashed. If a chunk's cryptographic content hash is identical to a previously stored chunk, grepai reuses the existing embedding vector, thereby bypassing unnecessary cloud API charges and reducing latency.   

Rename and Move Operations: Historically, moving a file caused an embedding cache miss because the file path was prepended to the chunk data. The v1.2.0 release implemented an embedding cache path fix, ensuring that renamed files correctly reuse their existing vectors without requiring re-embedding.   

Stale Symbol Pruning: The indexer accurately processes file deletions by removing orphaned vectors and pruning dead edges from the symbols.gob call graph.   

Cross-Platform Normalization: To maintain index portability between the Windows Hub and the macOS systems, the indexer applies filepath.ToSlash normalization, converting all path separators to Unix-style forward slashes prior to hashing or storing paths. This ensures that an index generated on Windows remains functional if transferred to an Apple Silicon machine.   

Concurrency, Locks, and Memory Spikes

The most severe architectural limitations of the software manifest during concurrent operations and bulk mutability, validating the decision to avoid the background watcher.

Event Dropping and Save Storms (Windows): On Windows platforms, rapid sequences of automated agent edits (save storms) can overflow the filesystem event queue. Bug reports (#348, #349, #355) document that under embedding backpressure, the queue blocks indefinitely, causing events to be silently dropped and leaving the index permanently stale.   

O(n) Linear Scan on Reuse (Bug #360): In the default GOB store, the LookupByContentHash function lacks a secondary index. When an agent edits a file, the software attempts to find reusable vectors by performing a linear O(n) scan across every single chunk stored in local memory. On a medium-sized codebase, this burns excessive CPU cycles, directly violating the mandate to minimize local CPU utilization.   

Peak RAM Spikes (Bug #361): During a full index (or a significant branch switch), the IndexAll decide-phase retains the complete, raw plaintext contents of every hashed file in the stats.HashedFiles map until the symbol extraction pass concludes. This causes peak RAM spikes that will directly compete with the memory overhead of the LLM or the IDE on the local machine.   

Multi-Client Shared Index Risks (CVE-2026-11479)

If the enterprise opts to deploy the PostgreSQL or Qdrant backend to support multiple developers simultaneously, a critical security vulnerability must be acknowledged. Disclosed as CVE-2026-11479, a flaw existed where the Qdrant chunk identity was generated using only the relative filePath and the chunkIndex (e.g., README.md_0), completely omitting a unique project namespace.   

Consequently, if Project A and Project B shared a backend collection, and both contained a README.md, the vector payload of Project B would violently overwrite the payload of Project A. An identical flaw existed in the Postgres cache lookup path (Bug #249), allowing one project to reuse vectors generated by another based purely on hash collisions. While patches were introduced to prefix project namespaces, the underlying fragility of the shared architecture suggests that strict logical isolation (one localized GOB file per worktree) is the only secure configuration.   

F. Resource and Cost Evidence
Debunking the 97% Token Reduction Claim

The developer's marketing materials heavily promote a benchmark claiming a 97% reduction in AI agent input tokens (dropping from 51,147 to 1,326) and a 27.5% reduction in API billing. A rigorous inspection of the primary source data reveals severe oracle contamination and methodological bias.   

The benchmark was executed against the native Glob and Grep tools inside Claude Code on the 155,000-line Excalidraw codebase. The native Claude Code architecture relies heavily on context caching (cache_creation_input_tokens). Because Glob returns wildcard file paths, the agent was forced to spawn 5 independent "subagents" to read the files sequentially. Each subagent instantiation required a complete duplication of the context window, consuming 563,883 cache creation tokens.   

The 97% reduction is primarily measuring the complete elimination of the subagent recursive loop, not a direct 1-to-1 comparison of search context density. While grepai successfully shortcuts the read-filter-repeat loop by returning the correct file path instantly via semantic match, declaring a universal 97% efficiency based on bypassing a specific framework's looping inefficiency is highly contextual. No ranking or operational decision should be based solely on this contaminated metric.   

Resource Utilization Matrix

Based on independent evaluation of the architectural constraints and known bug reports, the resource footprint is mapped as follows. Fields lacking empirical confirmation are explicitly marked UNKNOWN.

Resource Metric	Measurement / Status	Architectural Justification
Peak Process RAM	High (Spike)	

Retention of full file bodies in stats.HashedFiles during ingestion (Bug #361).


Idle Process RAM	UNKNOWN	Varies drastically based on the deserialized size of .grepai/index.gob.
CPU Seconds (Cold)	UNKNOWN	Bound by local AST tree-sitter parsing overhead and network API latency.
CPU Seconds (Warm)	High (Spike)	

O(n) linear scan during LookupByContentHash on GOB reuse path (Bug #360).


GPU / VRAM	0 GB	All VRAM overhead is successfully offloaded to the remote provider.
Disk Reads/Writes	UNKNOWN	

Mitigated by a 30s config write throttling mechanism.


Index Bytes	UNKNOWN	Scales with source code volume and configured vector dimensions.
Network Requests	Batched	

Minimized via parallelism and max_batch_size settings.


Network Bytes	UNKNOWN	Plaintext chunk transmission scales linearly with codebase churn.
Cloud Token Charges	$0.02 / 1M Tokens	

Defined by OpenAI text-embedding-3-small pricing.


Cold/Warm Churn Cost	Minimal	

Incremental re-embedding is avoided via content hash reuse.

  

The Storage/Search RAM Paradox: While utilizing remote embeddings successfully eliminates the requirement to dedicate 4GB-8GB of local VRAM to an Ollama instance, it does not eliminate local memory constraints. The mathematical vectors must still be stored and searched locally. In the GOB backend, the entirety of the high-dimensional vector space must be deserialized and loaded into standard RAM to compute cosine similarity during a search. Thus, large codebases will still impose heavy memory pressure on the local machine.

Simplest On-Demand Architecture Recommendation

To satisfy the mandate of minimizing TOTAL local CPU/RAM/GPU without deploying new background daemons or shared routers, the recommended architecture is:

One-Shot GOB Execution with TOON Compact Formatting.

No Watcher: Disable the daemon. Agents must explicitly invoke grepai index or trigger ingestion at the start of a session, entirely bypassing the Windows filesystem queue blocking (Bug #355).

TOON Output: Enforce the --toon --compact flag for all MCP interactions to guarantee maximum context window efficiency.   

Local Isolation: Utilize the default GOB store. Do not attempt to stand up a shared Qdrant or PostgreSQL cluster, avoiding all CVE-2026-11479 collision risks.

API Safeguards: Inject the <key-var> via the secure environment variable path to guarantee transmission strictly to official endpoints.

G. Additive Isolated Canary Protocol

To objectively evaluate the comparative benefit of integrating this tool without contaminating the existing CodeIndexer baseline, the following rigorous Additive Isolated Canary protocol must be executed.

1. Snapshot Sealing and Sanitization

Target: Select a representative, immutable snapshot of real, proprietary code encompassing Python, JS, and TS. (PowerShell will be included to explicitly test the failure mode of the AST parser).

Manifest: The baseline owner will supply a cryptographic hash manifest of the snapshot.

Sanitization: Execute a rigorous secret scan to ensure zero live API keys, session cookies, or confidential third-party data exist within the snapshot, as this data will be transmitted in plaintext to the OpenAI endpoint.

Isolation: The source truth and the evaluation rubric must remain strictly sealed from the independent answerers performing the test.

2. Task Construction

The protocol will feature tasks distributed across specific cognitive intents:

Multilingual Evaluation: Submit RU queries targeting EN code to test the semantic mapping of the text-embedding-3-small latent space.

Lexical Exact Match: Test the reliability of basic symbol location to ensure the vector approximation does not hallucinate false positives where a standard grep would succeed.

Semantic Navigation: Complex queries requiring conceptual understanding (e.g., "Identify the retry logic for failed database transactions").

No-Answer Thresholds: Submit queries for features deliberately stripped from the snapshot. The judge must evaluate whether the tool returns noise or correctly signals a null result based on confidence thresholds.

3. Execution and Counterbalance

Group A: Baseline environment relying solely on the native CodeIndexer and native LSP tools.

Group B: Baseline environment augmented with the grepai MCP stdio tool utilizing remote OpenAI embeddings.

Counterbalance: Both groups must run against the exact same snapshot in randomized order with independent answerers to neutralize caching bias or familiarization. All modifications (create/edit/rename/delete) must be performed exclusively on isolated copies of the snapshot.

4. Measurement Thresholds (Preregistered)

Before initiating the measurements, the following must be logged:

Serialized Context: An exact token count of ALL delivered context, including the TOON/JSON wrappers, structural overhead, and agent echoes.

Invocations: The raw count of actual product invocations (e.g., how many times grepai_search was called versus standard read_file).

Quality Metrics: Precision (accuracy of the retrieved file), Recall (missed files), and False Positives (hallucinated relationships).

Resource Constraints: Wall clock time (latency of the stdio cold start), peak RAM consumption (monitoring Bug #361), and total CPU seconds (monitoring Bug #360).

Financials: The exact fraction of API credits consumed, verified against the provider dashboard.

5. Abort, Rollback, and Readback Sequence

To guarantee no lingering side effects:

Abort: Issue a terminal signal (SIGTERM) to the agent and the child stdio MCP processes.

Rollback: Recursively delete the .grepai/ directory from the test worktree.

Readback Verification: Utilize a secondary OS-level readback script to scan the process tree and filesystem to verify that all PID locks (*.pid.lock) and binary GOB indexes have been successfully destroyed.

H. Skeptical Recommendation

Decision: TRY (Under strict, constrained conditions).

The Strongest Argument Against Integration:
The primary risk lies in the severe local resource monopolization introduced by the software's current architectural flaws, which directly violates the mandate to minimize local CPU/RAM. Despite offloading the computationally heavy VRAM embedding tasks to the cloud, the local execution path is highly unoptimized. The O(n) linear scan during content hash lookups (Bug #360) and the retention of full file bodies in RAM during index decisions (Bug #361) dictate that dynamic agent workflows—where the agent rapidly iterates, edits, and saves code—will trigger massive localized CPU and memory spikes. Furthermore, the tree-sitter precise mode provides a false sense of security; it accurately maps syntax but fails to resolve semantic inheritance or dynamic dispatch, potentially misleading an agent relying on structurally sound but semantically inaccurate call graphs. Finally, the total lack of PowerShell AST support renders it useless for a subset of the required ecosystem.   

Unresolved Gaps:

The exact byte footprint and deserialization latency of a 100k-token GOB file in idle RAM during a cold stdio MCP start is currently unmeasured (UNKNOWN).

The Windows filesystem event dropping and queue blocking issues under backpressure have not been conclusively patched, rendering continuous background indexing unreliable.

Narrowly Named Consumer:
The ideal consumer for this specific, constrained pilot is a Senior Developer operating Cursor or Claude Code exclusively on an Apple Silicon Mac Studio. This hardware restriction mitigates the Windows-specific filesystem event issues. The ideal repository is a large, relatively static, legacy TypeScript or Python codebase where semantic discovery currently costs the agent excessive context tokens, and where rapid, continuous multi-file automated refactoring (which triggers the O(n) reuse bugs) is not the primary workload.

Exact Next Experiment:
Do not issue a fleet-wide PASS based on a successful install and exit 0 acknowledgment.
Execute the Additive Isolated Canary Protocol (Section G) on a single Apple Silicon machine. Configure the tool explicitly for one-shot execution (disabling the watcher daemon). Utilize the environment-secured API path to OpenAI using existing organizational credits (guaranteeing zero new spend). The sole objective of this experiment is to evaluate whether the grepai search --toon --compact output successfully decreases agent context window consumption compared to the native CodeIndexer baseline, separating the functional utility of semantic discovery from the contaminated 97% marketing benchmark.

# Raw researcher report ends

# Visible report source links (collector extraction)

- github.com GitHub - yoanbernabeu/grepai: Semantic Search & Call Graphs for — https://github.com/yoanbernabeu/grepai
- github.com CHANGELOG.md - yoanbernabeu/grepai - GitHub — https://github.com/yoanbernabeu/grepai/blob/main/CHANGELOG.md
- github.com Releases · yoanbernabeu/grepai - GitHub — https://github.com/yoanbernabeu/grepai/releases
- mcpinstall.com Grepai Config Reference Agent Skill | MCPInstall — https://www.mcpinstall.com/skills/yoanbernabeu/grepai-skills/grepai-config-reference
- explainx.ai grepai-storage-gob — AI agent skill | explainx.ai — https://explainx.ai/skills/yoanbernabeu/grepai-skills/grepai-storage-gob
- github.com GrepAI Embeddings with OpenAI - GitHub — https://github.com/yoanbernabeu/grepai-skills/blob/main/skills/embeddings/grepai-embeddings-openai/SKILL.md
- github.com [Bug]: IndexAll decide-phase HashedFiles retains full file bodies — https://github.com/yoanbernabeu/grepai/issues/361
- yoanbernabeu.github.io Call Graph Analysis | grepai - GitHub Pages — https://yoanbernabeu.github.io/grepai/trace/
- github.com grepai/CLAUDE.md at main - GitHub — https://github.com/yoanbernabeu/grepai/blob/main/CLAUDE.md
- github.com Activity · yoanbernabeu/grepai · GitHub — https://github.com/yoanbernabeu/grepai/activity
- github.com grepai/install.sh at main - GitHub — https://github.com/yoanbernabeu/grepai/blob/main/install.sh
- github.com GitHub - yoanbernabeu/grepai-skills: 27 AI Agent Skills for GrepAI — https://github.com/yoanbernabeu/grepai-skills
- github.com Adaptive watch debounce for agent save storms · Issue #348 - GitHub — https://github.com/yoanbernabeu/grepai/issues/348
- github.com Watch flush blocks forever when event queue is full under embed — https://github.com/yoanbernabeu/grepai/issues/355
- github.com Pause watch indexing during bulk agent edits (freeze/unfreeze — https://github.com/yoanbernabeu/grepai/issues/349
- github.com GOB LookupByContentHash is O(n) — needs secondary index for — https://github.com/yoanbernabeu/grepai/issues/360
- github.com [Bug]: Cross-project Qdrant vector index overwrite via non ... - GitHub — https://github.com/yoanbernabeu/grepai/issues/247
- github.com [Bug]: Cross-project Postgres embedding cache reuse via ... - GitHub — https://github.com/yoanbernabeu/grepai/issues/249
- yoanbernabeu.github.io Blog | grepai — https://yoanbernabeu.github.io/grepai/blog/
- yoanbernabeu.github.io Benchmark: grepai vs grep on Claude Code - GitHub Pages — https://yoanbernabeu.github.io/grepai/blog/benchmark-grepai-vs-grep-claude-code/
- reddit.com [Open Source] I reduced Claude Code input tokens by 97% using — https://www.reddit.com/r/ClaudeAI/comments/1qiv0d3/open_source_i_reduced_claude_code_input_tokens_by/
