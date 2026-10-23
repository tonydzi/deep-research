# DR26-10-11-HUB-01-0535 — raw report from gemini

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Gemini (a@, Pro High) Deep Research report, collected 2026-10-11 from https://приватный чат

Deep Research Report: Desktop Multi-Harness Agent Environments
Executive Verdict

The analysis concludes that a Windows-first, multi-machine power user should proceed with a small, reversible canary deployment of a desktop multi-harness Agentic Development Environment (ADE). The fundamental transition from single-harness multi-model IDEs to agnostic multi-harness orchestrators provides verifiable advantages in preserving vendor-specific cognitive loops and utilizing existing consumer subscriptions.

The single best canary product for this specific environment is Emdash. It demonstrates superior native Windows subprocess safety, strict Git worktree isolation, and transparent local-first credential management. The designated fallback product is Orca, which offers a more expansive cross-platform interface and mobile control suite, but is currently severely penalized in Windows enterprise environments due to its reliance on aggressive PowerShell wrappers that trigger heuristic endpoint detection.

Confidence in the category's utility is [established], while confidence in the long-term Windows stability of the broader ecosystem remains [emerging]. All volatile metrics and repository data were retrieved on Sunday, October 11, 2026.

Establishing the Category Precisely

The software engineering landscape currently suffers from intense marketing dilution, where vendors conflate models, harnesses, and orchestrators to capture user workflows. Establishing precise taxonomic boundaries is required to evaluate the viability of these tools.

Core Definitions and Architectural Boundaries

A rigorous examination of the software stack reveals distinct operational layers. The model represents the underlying Large Language Model weights and neural architecture, such as Anthropic’s Claude 3.5 Sonnet, OpenAI’s GPT-6 Luna, or xAI’s Grok 4.7. The provider is the corporate entity or open-source collective hosting the model and serving it via API or consumer subscription tiers.   

The coding-agent harness (or runtime) is the native software loop that wraps the model. The harness dictates the autonomous cognitive cycle, managing prompt construction, tool dispatch, file system modifications, and context window compression. Tools like Claude Code, the OpenAI Codex CLI, and Grok Build operate at this layer.   

A multi-model IDE, such as Cursor or Zed, is a code editor that utilizes a single, proprietary internal harness but allows users to swap the underlying backend model via API keys. These applications do not preserve the native behavior of external harnesses; they merely route text to different models through their own monolithic logic.   

Conversely, a native-harness desktop shell or Agentic Development Environment (ADE) functions as an orchestration layer positioned above the harnesses. Instead of replacing the agent loop, an ADE launches multiple native CLI harnesses side by side in isolated pseudoterminals (PTYs). It manages their state, provisions independent Git worktrees, and provides unified differential review interfaces. Finally, configurations or routers, such as CC Switch, operate as network-level proxies or prompt managers. They route API calls but fail to provide the isolated, parallel workspace environments characteristic of a true ADE.   

The Engineering Value of Native Harness Preservation

The prior report claimed that preserving the native harness was a critical requirement. The evidence indicates that this claim is [established] based on three distinct engineering realities.

First, modern harnesses are deeply specialized for their target models. First-party harnesses incorporate proprietary meta-prompting, sub-agent delegation mechanics, and error-recovery routines that are entirely lost when a third-party IDE attempts to access the model through a generic API wrapper. For instance, Claude Code includes specialized sub-agent routines, and Grok Build delegates larger tasks to parallel subagents with independent context windows. Stripping the harness away degrades the model's effective autonomy.   

Second, native harness preservation dramatically alters the economics of agentic development. By executing the official CLIs, ADEs allow developers to authenticate using existing consumer subscriptions (e.g., ChatGPT Plus, Claude Pro at $20/month) via standard command-line logins (codex login, grok login). Generic multi-model IDEs typically require Bring-Your-Own-Key (BYOK) per-token billing, which accelerates cost accumulation during highly autonomous loop operations.   

Third, the ecosystem has standardized around two complementary but distinct protocols. The Model Context Protocol (MCP) connects an agent to external tools and data sources. The Agent Client Protocol (ACP)—recently transitioned to the Linux Foundation as A2A—standardizes communication between the IDE/ADE and the agent itself. An ADE utilizes ACP to communicate with the native harness, thereby allowing the harness to retain full autonomy over how it leverages its own MCP servers to execute tasks. Bypassing the native harness breaks this chain of delegated authority.   

Verification of Candidate Existence and Identity

The prior report's rankings included a combination of genuine ADEs, misidentified editor plugins, and network routers. Independent verification of every named candidate was conducted to establish their true identity, provenance, and architectural validity.

Claimed Name	Verified Identity	Canonical Source URL	License	Stars / Forks	Active Status
Orca	stablyai/orca	github.com/stablyai/orca	MIT	~4.1k / ~4.1k	Active
Superset	superset-sh/superset	github.com/superset-sh/superset	ELv2	~15k / 1.3k	Active
Emdash	generalaction/emdash	github.com/generalaction/emdash	Apache-2.0	~5.9k / 626	Active
Desktop CC GUI	vscode-cc-gui	github.com/zhukunpenglinyutong/vscode-cc-gui	MIT	~6.9k (JetBrains)	Active Plugin
bb / Get BB	get-bb/bb	github.com/get-bb/bb	UNVERIFIED	~4.1k / 602	Active
OpenChamber	openchamber/openchamber	github.com/openchamber/openchamber	UNVERIFIED	UNKNOWN	Active
Proliferate	proliferate-ai/claude-agent-acp	github.com/proliferate-ai/claude-agent-acp	UNVERIFIED	UNKNOWN	Active Adapter
CC Switch	farion1231/cc-switch	github.com/farion1231/cc-switch	UNVERIFIED	UNKNOWN	Active Router
Cursor / Zed	Cursor / Zed	cursor.com / zed.dev	Proprietary / MIT	N/A	Active IDEs

Retrieval Date for all metrics: October 11, 2026. Star counts represent market signaling, not definitive proof of software capability or security posture.

Detailed Candidate Audits

Orca (stablyai/orca): [established] Orca is a highly active multi-harness ADE maintained by Lovecast Inc. (StablyAI). It operates under the MIT license and demonstrates significant daily development velocity. It genuinely launches and preserves multiple native harnesses, spawning isolated pseudoterminals for Claude Code, Codex, Grok, and others. It features a robust parallel worktree system, allowing a single prompt to be fanned across multiple agents in isolated Git environments. Authentication relies on the native CLI logins. However, its Windows implementation relies on generating PowerShell wrappers (-ExecutionPolicy Bypass -EncodedCommand), which consistently trigger aggressive heuristics in endpoint security software.   

Superset (superset-sh/superset): [established] Superset is an agentic IDE designed to orchestrate hundreds of coding agents in parallel. It is released under the Elastic License 2.0 (ELv2), which is source-available but not strictly open-source by OSI definitions. It boasts approximately 15,000 GitHub stars. It supports AI assistants through the Model Context Protocol (MCP) and manages parallel execution effectively. However, specific documentation regarding its native Windows executable stability and deep credential isolation mechanisms remains opaque compared to its peers.   

Emdash (generalaction/emdash): [established] Emdash is an open-source ADE (Apache-2.0) backed by Y Combinator (W26). It explicitly runs multiple coding agents in parallel by isolating them in distinct Git worktrees and branches. It detects installed provider CLIs automatically and preserves their native operation through standard OS PTY sessions. Emdash ships native macOS, Windows (installer and portable), and Linux binaries. Crucially for the Windows use case, it utilizes safe execFile subprocess management rather than shell injection, and stores app state locally in an SQLite database.   

Desktop CC GUI / ccgui (zhukunpenglinyutong/vscode-cc-gui): [contradicted] The prior report categorized this as a desktop ADE. The analysis confirms this is a misidentification. The project is a 100% open-source GUI plugin designed strictly for VS Code and JetBrains IDEs. It renders an interface for Claude Code and Codex but is bound to the host IDE's execution environment. It is not a standalone multi-harness desktop shell.   

bb / Get BB (get-bb/bb): [established] Described as "the agent IDE that builds itself," bb organizes work into persistent "threads" and relies on a server/host-daemon architecture. It preserves native CLI execution via its @bb/agent-runtime adapters. However, its Windows x64 installer is explicitly labeled by its maintainers as "alpha: expect problems," with a strong recommendation that Windows users deploy it inside the Windows Subsystem for Linux (WSL2) instead.   

OpenChamber (openchamber/openchamber): [ambiguous] OpenChamber is an agentic development environment offering desktop, web, and mobile interfaces. While it advertises multi-run capabilities across different models, architectural evidence indicates it is tightly coupled to the OpenCode agent harness as its foundational layer. It functions more as a sophisticated graphical wrapper for OpenCode than a truly agnostic orchestrator capable of preserving Anthropic or xAI proprietary harnesses independently.   

Proliferate / AnyHarness & CC Switch: [contradicted] Proliferate (AnyHarness) is an Agent Client Protocol (ACP) adapter, functioning as middleware rather than a desktop control plane. CC Switch is a cross-platform companion tool and API relay, acting as a configuration router rather than an orchestrator of isolated parallel Git worktrees.   

Architectural Categorization

Based on the empirical audit, the candidates must be strictly separated to prevent methodological errors in deployment planning.

True Multi-Harness Shells (ADEs)	Single-Harness Multi-Model IDEs	Routers & Config Managers


Emdash: Manages native CLI binaries in isolated Git worktrees with strict PTY isolation and local SQLite state.

	

Cursor: Replaces native harnesses with proprietary backend routing, charging API costs or requiring subscription lock-in.

	

CC Switch: Proxies API requests but provides no parallel workspace orchestration.




Orca: Spawns isolated terminals and worktrees for Claude Code, Codex, and others simultaneously, offering mobile supervision.

	

Zed: Open-source editor providing a unified AI interface rather than native agent preservation.

	

Proliferate / AnyHarness: Middleware mapping different agent outputs via ACP specifications.




Superset: Orchestrates parallel tasks via MCP interfaces with a built-in diff viewer.

	

Desktop CC GUI: A plugin rendering an interface for CLIs inside standard IDEs, lacking standalone capabilities.

	


bb: Manages threads and environment states over a host daemon, but lacks native Windows maturity.

	

OpenChamber: Functions primarily as a GUI overlay for the OpenCode agent rather than a true agnostic ADE.

	
  
Audit of First-Party Harnesses

The prior report asserted the existence of several first-party vendor harnesses. The verification process reveals a rapidly maturing but highly volatile landscape where terminal-native tools are becoming the industry standard.

Heavyweight Native Harnesses

Grok Build: [established] Published by xAI, Grok Build is a powerful terminal-based TUI coding agent. It supports interactive modes, headless execution (grok -p), and full ACP implementation. Following a severe incident in July 2026 where the CLI was found to be exfiltrating entire Git repositories to cloud storage, xAI open-sourced the harness (~840,000 lines of Rust) under the Apache-2.0 license. It is a highly capable harness but requires strict local execution constraints due to its provenance.   

DeepSeek Harness (dsh): [established] Released by DeepSeek AI on August 13, 2026, alongside the DeepSeek-V4-Pro model. It is an open-source (MIT) agent runtime built on the Cordis meta-framework. It allows developers to replace lower-runtime components that Anthropic and OpenAI keep locked down, making it highly extensible for local orchestration.   

Google Antigravity: [established] Relaunched as Antigravity 2.0 at Google I/O in May 2026, this is a standalone cross-platform agentic desktop application utilizing Gemini 3 models. However, it functions as an orchestrator itself rather than a headless harness. Crucially, its execution environment contains a fatal flaw for the target demographic: its "Strict Mode" terminal sandboxing explicitly fails on Windows, returning sandboxing is not supported on Windows. This renders it unsafe for isolated operations on Microsoft operating systems.   

ZCode (for GLM): [established] Launched by Zhipu AI in July 2026 for the GLM-5.3 model, ZCode is a desktop agentic IDE. Like Antigravity, it leans heavily toward being a monolithic environment rather than a lightweight CLI harness easily embedded in third-party ADEs.   

Emerging Lightweight Harnesses

Mistral Vibe: [emerging] Maintained by Mistral AI, this is a command-line coding assistant that also ships as a VS Code extension. It executes autonomous codebase sweeps but is primarily optimized for editor-panel operation.   

Qwen Code & Kimi Code: [emerging] Terminal-native coding agents from Qwen AI and Moonshot AI, respectively. Both actively implement the Agent Client Protocol (ACP) and are designed to be fully embeddable by third-party desktop shells.   

Weighted Windows-First Comparison

To definitively support the deployment decision, the true ADE candidates were evaluated against the strict parameters of a Windows-first, multi-machine power user. The scoring penalizes theoretical capabilities that fail in practical Windows environments.

(Weightings applied: Windows Native & Stability: 20%; Preserves Native Harness & Logins: 20%; Security, Privacy & Isolation: 20%; Parallel Worktrees & Resumability: 15%; Openness & Provenance: 10%; Multi-Machine / Remote: 10%; Maturity & Reversibility: 5%)

Evaluation Criteria (Weight)	Emdash	Orca	bb (Get BB)	Superset
Windows Native Support & Stability (20%)	

High: Provides highly stable native .exe installers and portable binaries. Employs safe execFile subprocess management, avoiding shell injection risks entirely.

	

Medium-Low: While a native Windows executable exists, its reliance on PowerShell wrappers (-ExecutionPolicy Bypass -EncodedCommand) consistently triggers AhnLab V3 and Kaspersky AV behavioral heuristics, crashing the worktree creation process.

	

Low: The Windows x64 build is explicitly documented as "alpha: expect problems." The maintainers require users to deploy it inside WSL2 for stability, violating the native execution requirement.

	UNKNOWN: Specific Windows architectural documentation and stability metrics could not be retrieved.
Preserves Native Harness & Logins (20%)	

High: Wraps native CLIs (Claude Code, Codex) directly via standard OS PTYs. Honors existing consumer subscriptions seamlessly.

	

High: Offers full support for any CLI agent. Renders Ghostty-class WebGL terminal splits to perfectly preserve native TUI interfaces.

	

High: Utilizes the @bb/agent-runtime adapter to preserve native ACP/CLI execution over a host daemon.

	

High: Supports 100+ agents via CLI and MCP servers, orchestrating them without overwriting their logic.


Security, Privacy & Isolation (20%)	

High: Operates entirely local-first using an SQLite database. Features robust credential scanning (ThreatCrush) in its CI pipeline. Uses a specific Result pattern for expected failures, minimizing silent state corruption.

	

Medium: Lacks a published SECURITY.md or formal vulnerability disclosure program. While worktree isolation is architecturally sound, the AV-flagged PowerShell subprocess behavior denotes noisy and potentially insecure execution patterns on Windows.

	

UNKNOWN: Telemetry exists (recording OS and thread counts), but specific credential isolation architecture documentation could not be retrieved.

	UNKNOWN: Security architecture and execution isolation mechanisms were not retrieved.
Parallel Worktrees & Resumability (15%)	

High: Isolates every task in a highly independent Git worktree. Recovers stale network connections and pending prompts seamlessly during crashes.

	

High: Allows users to fan a single prompt across multiple agents, each in its own isolated Git worktree, and provides excellent state resumption.

	

Medium: Organizes work in branching "threads" and environments. Can fork threads to test different approaches.

	

High: Built explicitly to orchestrate vast quantities of agents in parallel.


Openness & Provenance (10%)	

High: Licensed under Apache-2.0. Transparent provenance backed by Y Combinator. Features reproducible pnpm workspace builds.

	

High: MIT licensed. Maintained by Lovecast Inc. (StablyAI). Windows binaries are cleanly signed by SignPath.

	

Medium: Open source, maintained by the "get-bb" collective, but absolute licensing clarity was not extracted.

	

Medium: Licensed under the Elastic License 2.0 (ELv2), which provides source availability but carries commercial restrictions.


Multi-Machine / Remote Operation (10%)	

High: Connects robustly to remote machines via SSH/SFTP, securely utilizing the OS keychain for OpenSSH certificate authentication.

	

High: Automatically deploys a managed Orca server upon SSH connection, seamlessly carrying over local sessions to the remote host.

	

Medium: The Server/Host-Daemon architecture theoretically allows remote daemons, but setup requires heavy custom provisioning compared to seamless SSH tunneling.

	UNKNOWN: Remote execution paradigms were not retrieved.
Maturity & Reversibility (5%)	

High: Exhibits a highly active, focused pull request cadence. Supports clean database resets (pnpm run db:reset) and portable executables, ensuring zero system pollution.

	

High: Ships daily updates. Features a highly mature user interface, complete with a mobile companion application.

	

Emerging: API and schema structures are evolving rapidly (e.g., the 0.43.0 release overhaul).

	Emerging: Offers a massive feature set, but specific architectural details regarding deployment reversibility are sparse.
  
Adversarial Validation and Critical Red Flags

The adversarial validation of the prior report exposed several critical security and operational risks. The rush to deploy autonomous agentic tooling has resulted in severe compromises to fundamental operating system security.

1. The Grok Build Data Exfiltration Incident (July 2026)

[established] A paramount security red flag surrounds the Grok Build CLI. In July 2026, security researchers utilizing network interception tools (e.g., mitmproxy) discovered that the agent was silently uploading entire user Git repositories to a Google Cloud Storage bucket. This unauthorized exfiltration included deleted Git history, .env files, and SSH keys. Crucially, the in-app privacy toggle provided by xAI did not halt this behavior [single-source: 198, 232].   

In response to the resulting scandal, xAI deleted the retained data and open-sourced the agent (~840,000 lines of Rust) under the Apache-2.0 license to restore trust. While compiling the binary locally from source prevents exfiltration to xAI's servers, the incident proves that running opaque coding agents with unfettered filesystem access fundamentally bypasses traditional Data Loss Prevention (DLP) controls. Any deployment of Grok Build must be strictly sandboxed.   

2. Windows Antivirus Collisions in Orca

[established] A critical operational flaw limits Orca's viability for the Windows power user. Orca executes terminal setups by passing generated .cmd files through a PowerShell wrapper configured with -ExecutionPolicy Bypass -EncodedCommand. This exact execution signature is heavily utilized by malware and is subsequently flagged as PDM:Exploit.Win32.Generic (Trojan behavior) by Enterprise Detection and Response (EDR) systems, including AhnLab V3 and Kaspersky. For a corporate or security-conscious power user, adopting Orca currently requires whitelisting a dangerous execution policy bypass globally, severely weakening the machine's security posture. Furthermore, an identified bug in Orca (#2927) causes worktree removal to force-delete unpublished branch commits, resulting in irretrievable data loss.   

3. Google Antigravity Windows Sandbox Failure

[established] Google Antigravity relies on an OS-level sandbox to safely execute the terminal commands generated by its autonomous agent. However, this sandbox utilizes macOS and Linux native execution paradigms (sandbox-exec). When a Windows user attempts to enable "Strict Mode," the agent errors out completely with the fatal message: failed to set up sandbox: sandboxing is not supported on Windows. Consequently, running Antigravity on Windows inherently means executing AI-generated shell commands directly against the host OS without isolation—an unacceptable risk profile.   

4. bb's Native Windows Immaturity

[established] The get-bb/bb orchestrator explicitly documents its Windows x64 native binary as "alpha." The maintainers actively advise users to run the Linux version via WSL2 to avoid instability. For a dedicated Windows-first user relying on native OS file paths, credential managers, and tooling, this introduces massive and unnecessary virtualization friction.   

Methodological Failures of the Prior Report

The prior report reached flawed conclusions by committing three analytical errors:

Confusing Editor Plugins with Orchestrators: It erroneously classified Desktop CC GUI as a standalone ADE, failing to verify that it is merely a visual plugin dependent on an underlying IDE (VS Code/JetBrains).   

Ignoring OS-Specific Execution Flaws: It recommended Orca and bb without auditing their native Windows execution pipelines, completely missing the PowerShell AV collision blocking Orca and the alpha status hamstringing bb.   

Treating Market Signal as Security Proof: It failed to scrutinize the provenance and security architecture of the first-party harnesses, omitting the massive data exfiltration risks inherent in the pre-compiled Grok Build binary.   

Audit of Claims from the Prior Report
Prior Claim	Audit Result	Evidence / Justification
1. The category is a "desktop ADE" or "multi-harness orchestrator."	Confirmed	

The industry strictly delineates between multi-model IDEs (Cursor) and multi-harness orchestrators (Orca, Emdash).


2. Cursor, VS Code, and Zed are IDEs, not multi-harness environments.	Confirmed	

They use a single integration layer, failing to preserve the unique runtime cognitive loops of the native vendor CLIs.


3. Desktop CC GUI / ccgui is a top ADE candidate.	Contradicted	

It is not a standalone desktop ADE; it is a JetBrains/VS Code plugin requiring a host IDE to function.


4. OpenChamber is a generic multi-harness orchestrator.	Ambiguous	

While supporting multiple tools, it acts primarily as the GUI shell for the OpenCode agent rather than a true agnostic orchestration layer.


5. bb / Get BB is a top-tier desktop option.	Contradicted	

Unsuitable for a native Windows-first environment due to its self-admitted alpha status and reliance on WSL2.


6. First-party harnesses include Grok Build, DeepSeek Harness, ZCode, Antigravity.	Confirmed	

All explicitly exist, though Antigravity and ZCode function closer to monolithic ADEs than easily embeddable CLI harnesses.

  
Recommendation

Based on the exhaustive analysis of native Windows stability, secure subprocess architecture, and genuine native harness preservation, the prior report's rankings are fundamentally flawed for a Windows-first deployment.

Canary Recommendation: The single best product to canary is Emdash.

Main Assumption: This recommendation assumes the user requires the retention of existing consumer CLI subscriptions (avoiding API token costs) and demands strict boundary isolation for agent execution on the local filesystem.

Justification: Emdash achieves true multi-harness preservation by isolating provider CLIs in independent Git worktrees and managing them via standard OS PTYs. Crucially, it utilizes safe subprocess execution (execFile), ships highly stable Windows portable binaries, relies on a local-first SQLite database, and is explicitly governed by an Apache-2.0 license. It completely avoids the aggressive PowerShell abstractions that trigger EDR systems.   

Fallback Recommendation: Orca.

Trade-offs: Orca boasts an arguably superior user interface, including WebGL terminal splits and comprehensive mobile companion app integration. However, its reliance on encoded PowerShell execution policies triggers behavioral AV heuristics on Windows, introducing significant deployment friction in secured environments.   

Explicit No-Go Conditions:

bb (Get BB): Do not deploy natively on Windows; its alpha status introduces unacceptable stability risks.

Google Antigravity: Do not deploy on Windows for any code requiring execution isolation; the protective sandbox is mathematically broken on the operating system.

Grok Build (Pre-compiled/Hosted): Do not deploy without compiling directly from the Apache-2.0 source code and explicitly auditing network egress paths, given its proven history of catastrophic repository exfiltration.

60-Minute Canary Acceptance Plan

To safely evaluate the recommended ADE without modifying production repositories, compromising valuable credentials, or triggering global system pollution, execute the following zero-cost, fully reversible canary protocol.

Phase 1: Environment Provisioning (15 minutes)

Download Portable Executable: Retrieve the latest emdash-x64.exe portable release directly from the generalaction/emdash GitHub Releases page. Do not use the global system installer. This ensures all application state is kept strictly localized.   

Verify Agent Availability: Ensure at least two native CLIs (e.g., Claude Code, OpenAI Codex) are installed and authenticated using existing free or pro consumer tiers. Verify they operate headlessly in a standard Windows cmd.exe prompt.   

Establish Disposable Worktree: Create a net-new directory (C:\Temp\EmdashCanaryRepo). Initialize an empty Git repository (git init). Add a simple script (e.g., a basic Python calculator) and commit it. Do not place valuable credentials or .env files anywhere in this path hierarchy.

Phase 2: Execution and Isolation Testing (30 minutes)

Launch the Orchestrator: Open the portable .exe. Point the UI at C:\Temp\EmdashCanaryRepo.

Parallel Spawn: Use the ADE interface to spawn two tasks simultaneously:

Task A (Claude Code): "Refactor the calculator to support object-oriented patterns."

Task B (Codex): "Write a suite of unit tests for the calculator."

Worktree Audit: While the agents are running, open a standard Windows file explorer. Navigate to the hidden .scratch or .claude directories generated by Emdash. Verify that the application has correctly created two isolated, parallel Git worktrees, ensuring the agents are not overwriting each other's files mid-execution.   

Security and Network Audit: Open the Windows Resource Monitor. Observe the subprocess tree under Emdash. Verify that Emdash has spawned standard child CLI processes without anomalous network egress spikes (mitigating Grok-style telemetry leakage risks).

Phase 3: Rollback and Review (15 minutes)

Differential Review: Utilize Emdash's internal diff viewer to review the work produced by both agents. Merge one of the changes into the main disposable branch to validate the Git state engine's accuracy.

Zero-Trace Teardown: Close the Emdash application. Delete the C:\Temp\EmdashCanaryRepo directory entirely. Delete the emdash-x64.exe portable binary. To ensure absolute reversibility, navigate to %APPDATA%\Emdash and delete the SQLite database directory, leaving zero trace of the application on the host machine.   

Required Output 9: Source List

[cite: 4] Nimbalyst Blog: Best AI Coding Agents 2026. https://nimbalyst.com/blog/best-ai-coding-agents-2026/ (Retrieved Oct 11, 2026).

[cite: 13] Codecrush Skills Framework. https://codecrush.com.br/skills (Retrieved Oct 11, 2026).

[cite: 8] Onepoint Blog: Piloting Multiple Agents. https://dev.to/onepoint/y-a-t-il-un-copilote-dans-lavion-de-lagentique-1m8c (Retrieved Oct 11, 2026).

[cite: 23] Superset GitHub Repository. https://github.com/superset-sh/superset (Retrieved Oct 11, 2026).

[cite: 33] Superset Blog: Herding Parallel Agents. https://blog.bms.ovh/blog/supersetsh-editor-herding-parallel-ai-coding-agents (Retrieved Oct 11, 2026).

[cite: 7] Agentastic: Agent Orchestration Guide. https://www.agentastic.dev/agent-orchestration (Retrieved Oct 11, 2026).

[cite: 9] Emdash GitHub Repository. https://github.com/generalaction/emdash (Retrieved Oct 11, 2026).

[cite: 24] Emdash CONTRIBUTING.md. https://github.com/generalaction/emdash/blob/main/CONTRIBUTING.md (Retrieved Oct 11, 2026).

[cite: 1] Emdash Releases Changelog. https://github.com/generalaction/emdash/releases (Verified Sep 27, 2026, Retrieved Oct 11, 2026).

[cite: 50] Emdash Pull Requests. https://github.com/generalaction/emdash/pulls (Retrieved Oct 11, 2026).

[cite: 25] CCGUI GitHub Repository. https://github.com/zhukunpenglinyutong/vscode-cc-gui (Retrieved Oct 11, 2026).

[cite: 25] CCGUI VSIX Documentation. https://github.com/zhukunpenglinyutong/vscode-cc-gui (Retrieved Oct 11, 2026).

[cite: 26] JetBrains CC GUI Repository. https://github.com/zhukunpenglinyutong/jetbrains-cc-gui (Retrieved Oct 11, 2026).

[cite: 27] bb (Get BB) GitHub Repository. https://github.com/get-bb/bb (Retrieved Oct 11, 2026).

[cite: 15] bb App Package Documentation. https://github.com/get-bb/bb/blob/main/packages/bb-app/README.md (Retrieved Oct 11, 2026).

[cite: 54] bb GitHub Tool Shelf Reference. https://github.com/get-bb/bb?ref=toolshelf (Retrieved Oct 11, 2026).

[cite: 28] Flavio Copes: bb Agentic IDE Architecture. https://flaviocopes.com/bb-agentic-ide/ (Retrieved Oct 11, 2026).

[cite: 28] Flavio Copes: Agent Runtime Adapters. https://flaviocopes.com/bb-agentic-ide/ (Retrieved Oct 11, 2026).

[cite: 29] OpenChamber GitHub Repository. https://github.com/openchamber/openchamber (Retrieved Oct 11, 2026).

[cite: 61] OpenChamber Official Site. https://openchamber.dev/ (Retrieved Oct 11, 2026).

[cite: 30] Hacker News Comments: OpenChamber Discussion. https://news.ycombinator.com/item?id <tg-id> (Retrieved Oct 11, 2026).

[cite: 31] Proliferate Architecture Docs. https://proliferate.com/docs/how-proliferate-works/architecture (Retrieved Oct 11, 2026).

[cite: 62] Proliferate GitHub Repository. https://github.com/proliferate-ai/proliferate (Retrieved Oct 11, 2026).

[cite: 32] Proliferate ACP Adapter. https://github.com/proliferate-ai/claude-agent-acp (Retrieved Oct 11, 2026).

[cite: 11] CC Switch SourceForge Mirror. https://sourceforge.net/projects/cc-switch.mirror/ (Retrieved Oct 11, 2026).

[cite: 12] CC Switch GitHub Repository. https://github.com/farion1231/cc-switch (Retrieved Oct 11, 2026).

[cite: 5] CodeAgentSwarm: Grok Build Guide. https://www.codeagentswarm.com/en/guides/how-to-use-grok-build (Retrieved Oct 11, 2026).

[cite: 2] xAI Official Docs: Grok Build. https://docs.x.ai/build/overview (Retrieved Oct 11, 2026).

[cite: 63] Grok Build GitHub Repository. https://github.com/xai-org/grok-build (Retrieved Oct 11, 2026).

[cite: 34] Grok Build CLI Reference. https://docs.x.ai/build/cli/reference (Retrieved Oct 11, 2026).

[cite: 14] xAI Build Documentation. https://x.ai/build (Retrieved Oct 11, 2026).

[cite: 36] Eigent AI: DeepSeek Harness Runtime. https://www.eigent.ai/blog/deepseek-harness-agent-runtime (Retrieved Oct 11, 2026).

[cite: 37] XCloud Host: DeepSeek Harness Overview. https://xcloud.host/what-is-deepseek-harness-features-architecture-use-cases/ (Retrieved Oct 11, 2026).

[cite: 38] Reddit /r/LocalLLaMA: DeepSeek Harness Launch. https://www.reddit.com/r/LocalLLaMA/comments/1vnb66j/deepseek_harness_is_up/ (Retrieved Oct 11, 2026).

[cite: 42] ZCode Official Site. https://zcode.z.ai/en (Retrieved Oct 11, 2026).

[cite: 64] Flowtivity Blog: ZCode GLM Coding Agent. https://flowtivity.ai/blog/zcode-glm-coding-agent-harness/ (Retrieved Oct 11, 2026).

[cite: 43] Venture Atlas: Zhipu AI ZCode Specs. https://www.ventureatlas.org/product/zhipu-ai-autoglm-zcode-agents-and-coding (Retrieved Oct 11, 2026).

[cite: 39] Google Codelabs: Getting Started with Antigravity. https://codelabs.developers.google.com/getting-started-google-antigravity (Retrieved Oct 11, 2026).

[cite: 40] Wikipedia: Google Antigravity. https://en.wikipedia.org/wiki/Google_Antigravity (Retrieved Oct 11, 2026).

[cite: 44] Mistral Vibe GitHub Repository. https://github.com/mistralai/mistral-vibe (Retrieved Oct 11, 2026).

[cite: 45] VS Code Marketplace: Mistral Vibe. https://marketplace.visualstudio.com/items?itemName=mistralai.mistral-vibe-code (Retrieved Oct 11, 2026).

[cite: 19] Orca LICENSE file. https://github.com/stablyai/orca/blob/main/LICENSE (Retrieved Oct 11, 2026).

[cite: 48] Orca README (apifox mirror). https://github.com/stablyai/orca/?ref=apifox.com (Retrieved Oct 11, 2026).

[cite: 20] Orca README. https://github.com/stablyai/orca/blob/main/README.md (Retrieved Oct 11, 2026).

[cite: 55] Orca Releases Changelog. https://github.com/stablyai/orca/releases (Retrieved Oct 11, 2026).

[cite: 52] Orca Pull Requests. https://github.com/stablyai/orca/pulls (Retrieved Oct 11, 2026).

[cite: 65] Orca GitHub Discussions / Stars. https://github.com/stablyai/orca/discussions/1178 (Retrieved Oct 11, 2026).

[cite: 23] Superset GitHub Repo Main. https://github.com/superset-sh/superset (Retrieved Oct 11, 2026).

[cite: 49] Superset Org Repositories. https://github.com/orgs/superset-sh/repositories (Retrieved Oct 11, 2026).

[cite: 66] Superset GitHub Stats. https://github.com/superset-sh (Retrieved Oct 11, 2026).

[cite: 22] Superset README.ru.md (License). https://github.com/superset-sh/superset/blob/main/readme/README.ru.md (Retrieved Oct 11, 2026).

[cite: 21] Orca Issue #14233: AhnLab/Kaspersky AV Detections. https://github.com/stablyai/orca/issues/14233 (Retrieved Oct 11, 2026).

[cite: 21] Orca Issue #14233: AhnLab V3 Internet Security. https://github.com/stablyai/orca/issues/14233 (Retrieved Oct 11, 2026).

[cite: 60] Orca Issue #2927: Worktree Data Loss Bug. https://github.com/stablyai/orca/issues/2927 (Retrieved Oct 11, 2026).

[cite: 67] Emdash Issue #2140: Windows WSL installation hangs. https://github.com/generalaction/emdash/issues/2140 (Retrieved Oct 11, 2026).

[cite: 47] bb Issue #1251: Windows Permissions. https://github.com/get-bb/bb/issues/1251 (Retrieved Oct 11, 2026).

[cite: 68] OpenChamber Issue #183: Windows WSL termination. https://github.com/openchamber/openchamber/issues/183 (Retrieved Oct 11, 2026).

[cite: 16] ACP GitHub Repository. https://github.com/agentclientprotocol/agent-client-protocol (Retrieved Oct 11, 2026).

[cite: 17] ACP Dev Docs (Linux Foundation A2A). https://agentcommunicationprotocol.dev/introduction/welcome (Retrieved Oct 11, 2026).

[cite: 18] IBM Think: Agent Communication Protocol (ACP). https://www.ibm.com/think/topics/agent-communication-protocol (Retrieved Oct 11, 2026).

[cite: 6] CircleCI Blog: ACP vs MCP for Agentic Coding. https://circleci.com/blog/acp-vs-mcp-whats-the-difference-for-agentic-coding/ (Retrieved Oct 11, 2026).

[cite: 46] JetBrains ACP Documentation. https://www.jetbrains.com/acp/ (Retrieved Oct 11, 2026).

[cite: 56] Reddit /r/LocalLLaMA: Grok Build Repo Uploads. https://www.reddit.com/r/LocalLLaMA/comments/1ut7tis/grok_build_cli_uploads_your_whole_repo_full_git/ (Retrieved Oct 11, 2026).

[cite: 69] Reddit /r/better_claw: Grok Build privacy toggle failure. https://www.reddit.com/r/better_claw/comments/1uvi1d1/grok_build_cli_was_uploading_entire_repos/ (Retrieved Oct 11, 2026).

[cite: 20] Orca Code Signing Info. https://github.com/stablyai/orca/blob/main/README.md (Retrieved Oct 11, 2026).

[cite: 9] Emdash GitHub Repository / Install Instructions. https://github.com/generalaction/emdash (Retrieved Oct 11, 2026).

[cite: 9] Emdash Generalaction Profile. https://github.com/generalaction/emdash (Retrieved Oct 11, 2026).

[cite: 24] Emdash CONTRIBUTING.md (Dependencies & Security). https://github.com/generalaction/emdash/blob/main/CONTRIBUTING.md (Retrieved Oct 11, 2026).

[cite: 10] Emdash AGENTS.md Architecture. https://github.com/generalaction/emdash/blob/main/AGENTS.md (Retrieved Oct 11, 2026).

[cite: 27] bb Get BB Repository README. https://github.com/get-bb/bb (Retrieved Oct 11, 2026).

[cite: 53] bb CHANGELOG.md. https://github.com/get-bb/bb/blob/main/CHANGELOG.md (Retrieved Oct 11, 2026).

[cite: 51] Orca Security Policy Dashboard. https://github.com/stablyai/orca/security (Retrieved Oct 11, 2026).

[cite: 57] DevOps.com: xAI Open-Sources Grok Build. https://devops.com/xai-open-sources-grok-build-coding-agent-after-cloud-upload-exposes-ssh-keys-repos/ (Retrieved Oct 11, 2026).

[cite: 59] Appwrite Blog: Grok Build Open Source. https://appwrite.io/blog/post/grok-build-open-source (Retrieved Oct 11, 2026).

[cite: 58] Humphrey Theodore: AI Coding Agent Exfiltration. https://humphreytheodore.com/writing/ai-coding-agent-grok-build-repo-exfiltration-2026 (Retrieved Oct 11, 2026).

[cite: 35] QCode: Grok Build Open Source Guide 2026. https://qcode.cc/en/grok-build-open-source (Retrieved Oct 11, 2026).

[cite: 3] QWE Edu Tutorial: Grok Build Open Source. https://www.qwe.edu.pl/tutorial/grok-build-open-source/ (Retrieved Oct 11, 2026).

[cite: 41] Google AI Developers Forum: Windows Sandbox Failure. https://discuss.ai.google.dev/t/error-executing-cascade-step-cortex-step-type-run-command-failed-to-set-up-sandbox-sandboxing-is-not-supported-on-windows/138073/6 (Retrieved Oct 11, 2026).

[cite: 41] Google Antigravity Issue Feedback: Strict Mode Sandbox. https://discuss.ai.google.dev/t/error-executing-cascade-step-cortex-step-type-run-command-failed-to-set-up-sandbox-sandboxing-is-not-supported-on-windows/138073/6 (Retrieved Oct 11, 2026).

## Source links from panel (a[href])
- https://github.com/generalaction/emdash/releases
- https://docs.x.ai/build/overview
- https://www.qwe.edu.pl/tutorial/grok-build-open-source/
- https://nimbalyst.com/blog/best-ai-coding-agents-2026/
- https://www.codeagentswarm.com/en/guides/how-to-use-grok-build
- https://circleci.com/blog/acp-vs-mcp-whats-the-difference-for-agentic-coding/
- https://www.agentastic.dev/agent-orchestration
- https://dev.to/onepoint/y-a-t-il-un-copilote-dans-lavion-de-lagentique-1m8c
- https://github.com/generalaction/emdash
- https://github.com/generalaction/emdash/blob/main/AGENTS.md
- https://sourceforge.net/projects/cc-switch.mirror/
- https://github.com/farion1231/cc-switch
- https://codecrush.com.br/skills
- https://x.ai/build
- https://github.com/get-bb/bb/blob/main/packages/bb-app/README.md
- https://github.com/agentclientprotocol/agent-client-protocol
- https://agentcommunicationprotocol.dev/introduction/welcome
- https://www.ibm.com/think/topics/agent-communication-protocol
- https://github.com/stablyai/orca/blob/main/LICENSE
- https://github.com/stablyai/orca/blob/main/README.md
- https://github.com/stablyai/orca/issues/14233
- https://github.com/superset-sh/superset/blob/main/readme/README.ru.md
- https://github.com/superset-sh/superset
- https://github.com/generalaction/emdash/blob/main/CONTRIBUTING.md
- https://github.com/zhukunpenglinyutong/vscode-cc-gui
- https://github.com/zhukunpenglinyutong/jetbrains-cc-gui
- https://github.com/get-bb/bb
- https://flaviocopes.com/bb-agentic-ide/
- https://github.com/openchamber/openchamber
- https://news.ycombinator.com/item?id <tg-id>
- https://proliferate.com/docs/how-proliferate-works/architecture
- https://github.com/proliferate-ai/claude-agent-acp
- https://blog.bms.ovh/blog/supersetsh-editor-herding-parallel-ai-coding-agents
- https://docs.x.ai/build/cli/reference
- https://qcode.cc/en/grok-build-open-source
- https://www.eigent.ai/blog/deepseek-harness-agent-runtime
- https://xcloud.host/what-is-deepseek-harness-features-architecture-use-cases/
- https://www.reddit.com/r/LocalLLaMA/comments/1vnb66j/deepseek_harness_is_up/
- https://codelabs.developers.google.com/getting-started-google-antigravity
- https://en.wikipedia.org/wiki/Google_Antigravity
- https://discuss.ai.google.dev/t/error-executing-cascade-step-cortex-step-type-run-command-failed-to-set-up-sandbox-sandboxing-is-not-supported-on-windows/138073/6
- https://zcode.z.ai/en
- https://www.ventureatlas.org/product/zhipu-ai-autoglm-zcode-agents-and-coding
- https://github.com/mistralai/mistral-vibe
- https://marketplace.visualstudio.com/items?itemName=mistralai.mistral-vibe-code
- https://www.jetbrains.com/acp/
- https://github.com/get-bb/bb/issues/1251
- https://github.com/stablyai/orca/?ref=apifox.com
- https://github.com/orgs/superset-sh/repositories
- https://github.com/generalaction/emdash/pulls
- https://github.com/stablyai/orca/security
- https://github.com/stablyai/orca/pulls
- https://github.com/get-bb/bb/blob/main/CHANGELOG.md
- https://github.com/get-bb/bb?ref=toolshelf
- https://github.com/stablyai/orca/releases
- https://www.reddit.com/r/LocalLLaMA/comments/1ut7tis/grok_build_cli_uploads_your_whole_repo_full_git/
- https://devops.com/xai-open-sources-grok-build-coding-agent-after-cloud-upload-exposes-ssh-keys-repos/
- https://humphreytheodore.com/writing/ai-coding-agent-grok-build-repo-exfiltration-2026
- https://appwrite.io/blog/post/grok-build-open-source
- https://github.com/stablyai/orca/issues/2927
- https://github.com/happycatlabs/orca
- https://www.reddit.com/r/OrcaADE/comments/1w77fui/turning_orca_into_a_multiharness_ai_coding/
- https://www.reddit.com/r/ClaudeWorkflows/comments/1w0hl7n/workflow_automated_multiharness_code_development/
- https://github.com/echoVic/orca-agent
- https://www.reddit.com/r/ClaudeCode/comments/1wgutzu/orca_for_orchestration_anyone_using_it_here_with/
- https://forum.cursor.com/t/how-are-people-handling-context-across-different-ai-coding-tools/159891/88
- https://skillsllm.com/skill/superset
- https://superset.apache.org/user-docs/using-superset/using-ai-with-superset/
- https://github.com/generalaction/emdash/blob/main/agents/integrations/providers.md
- https://github.com/zhukunpenglinyutong/desktop-cc-gui
- https://community.obsidian.md/plugins/bb-ai-agent
- https://github.com/get-bb/bb/blob/main/AGENTS.md
- https://sourceforge.net/projects/bb-ide.mirror/
- https://www.skillsdirectory.com/skills/get-bb-bb-plugin-authoring
- https://openchamber.dev/
- https://openchamber.dev/blog/ai-coding-agents/
- https://github.com/openchamber
- https://github.com/openchamber/openchamber/releases
- https://github.com/openchamber/openchamber/blob/main/AGENTS.md
- https://github.com/syntax-syndicate/opencode-chamber
- https://www.reddit.com/r/opencode/comments/1wxjlar/openchamber_is_actually_pretty_good/
- https://github.com/proliferate-ai/proliferate
- https://github.com/topics/cc-switch
- https://github.com/farion1231/cc-switch/blob/main/README_ZH.md
- https://github.com/farion1231/cc-switch/releases
- https://github.com/xai-org/grok-build
- https://www.truefoundry.com/docs/ai-gateway/grok-build
- https://x.ai/news/grok-build-cli
- https://www.eigent.ai/blog/grok-build-cli
- https://www.mindstudio.ai/blog/deepseek-harness-agentic-coding
- https://www.datacamp.com/tutorial/deepseek-harness
- https://github.com/deepseek-ai/deepseek-harness
- https://www.datacamp.com/blog/what-is-deepseek-harness
- https://shop.zimaspace.com/blogs/tech-ai-hub/de-minimal-and-creator-explained
- https://www.deepseek.com/en/harness/
- https://myclaw.ai/blog/deepseek-harness
- https://flowtivity.ai/blog/zcode-glm-coding-agent-harness/
- https://zcode.z.ai/en/docs/configuration
- https://developers.googleblog.com/build-with-google-antigravity-our-new-agentic-development-platform/
- https://antigravity.google/
- https://antigravity.google/docs/home/
- https://antigravity.google/product/antigravity-2
- https://antigravity.google/product/
- https://antigravity.google/docs/agent/
- https://antigravity.google/docs/overview/
- https://antigravity.google/blog/introducing-google-antigravity
- https://github.com/stablyai/orca
- https://github.com/stablyai/orca/discussions/1178
- https://github.com/stablyai/orca/blob/main/docs/readme/README.fr.md
- https://github.com/stablyai/orca/blob/main/docs/readme/README.pt.md
- https://github.com/stablyai/orca/blob/main/docs/readme/README.ja.md
- https://github.com/superset-sh/superset/releases
- https://github.com/superset-sh/superset?ref=ssp.sh
- https://github.com/superset-sh/superset?ref=producthunt
- https://github.com/superset-sh
- https://github.com/superset-sh/superset/blob/main/readme/README.pt-BR.md
- https://github.com/superset-sh/superset/blob/main/readme/README.cs.md
- https://github.com/stablyai/orca/issues/19624
- https://github.com/stablyai/orca/issues/12444
- https://github.com/stablyai/orca/issues/8029
- https://github.com/stablyai/orca/issues/16724
- https://github.com/stablyai/orca/issues/6896
- https://github.com/stablyai/orca/issues/11960
- https://github.com/stablyai/orca/issues/10671
- https://github.com/generalaction/emdash/issues
- https://github.com/generalaction/emdash/issues/688
- https://github.com/generalaction/emdash/issues/1902
- https://github.com/generalaction/emdash/issues/3192
- https://github.com/generalaction/emdash/issues/2140
- https://github.com/get-bb/bb/issues/3401
- https://en.wikipedia.org/wiki/Microsoft_Defender_Antivirus
- https://github.com/openchamber/openchamber/issues/183
- https://github.com/openchamber/openchamber/issues/1808
- https://github.com/openchamber/openchamber/issues/1136
- https://github.com/openchamber/openchamber/issues/1010
- https://github.com/openchamber/openchamber/issues/2544
- https://github.com/agentclientprotocol
- https://agentic-ai.readthedocs.io/en/latest/Standards/agent-client-protocol/
- https://blog.marcnuri.com/agent-client-protocol-acp-introduction
- https://tyk.io/learning-center/agent-protocols-a-complete-guide-to-mcp-a2a-and-acp/
- https://www.reddit.com/r/better_claw/comments/1uvi1d1/grok_build_cli_was_uploading_entire_repos/
- https://cereblab.com/
- https://www.qwe.edu.pl/tutorial/grok-build-cli-what-it-sends-to-xai/
- https://moltbook.com/post/812b873a-2d07-41ce-afc4-af220cdd8d5b
- https://www.reddit.com/r/AISEOInsider/comments/1ve7cb9/new_grok_update_today_is_live_everywhere_at_once/
- https://www.reddit.com/r/technology/comments/1uvmgiw/xais_grok_build_cli_uploads_users_entire_git/
- https://ai-tldr.dev/releases/grok-build-cli-uploads-jul10/
- https://www.consens.io/s/does-grok-code-upload-your-entire-repository-to-xai-QxMJTQsuHj2bW5dn
- https://antigravity.google/docs/faq/
- https://github.com/flpsnd/emdashCustory
- https://github.com/generalaction/emdash/blob/main/CLAUDE.md
- https://github.com/generalaction
- https://github.com/DavidWells/stars/blob/master/README.md
- https://github.com/generalaction/emdash/blob/main/package.json
- https://github.com/DavidWells/stars/blob/master/stars/generalaction/emdash.md
- https://github.com/generalaction/emdash?ref=producthunt
- https://github.com/MGrin/awesome-bb-plugins
- https://github.com/stablyai/orca/issues/9699
- https://www.progressiverobot.com/2026/09/23/grok-remote-control-xai-desktop-app/
- https://github.com/xai-org/grok-build/security
- https://x.ai/open-source
- https://www.aipolicydesk.com/blog/xai-grok-build-coding-agent-enterprise-data-governance-2026
- https://antigravity.google/docs/settings/
- https://antigravity.google/docs/sandbox/
- https://antigravity.google/docs/agent-settings/
- https://antigravity.google/docs/permissions/
- https://antigravity.google/docs/mcp/
- https://antigravity.google/docs/features/
- https://antigravity.google/docs/cli/modes/
- https://www.reddit.com/r/google_antigravity/comments/1q6rkkj/how_do_you_sandbox_your_antigravity_warning_it/
- https://www.cybedefend.com/en/blog/is-google-antigravity-safe
