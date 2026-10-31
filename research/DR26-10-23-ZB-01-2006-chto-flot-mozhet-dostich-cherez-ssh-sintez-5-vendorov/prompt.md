# DR26-10-23-ZB-01-2006 — the exact prompt sent to every model

# DR26-10-23-ZB-01-2006 — Что флот может достичь через SSH: удалённые видимые сессии, оркестрация, бэкапы, GPU хаба, безопасность ключей

# Deep Research: what a small private fleet of machines can achieve over SSH (2025-2026 practice)

CONTEXT
We run a small private "fleet" of 8 personal machines that act as one system for AI-agent work: 2 Windows 11 laptops/desktops (one is a home hub with 2 NVIDIA GPUs, RTX 5060 Ti class), 3 macOS machines (Intel and Apple Silicon, macOS 26), 1 Linux VPS (Hetzner, always-on coordinator), plus 3 machines owned by team members (2 Windows, 1 Mac). All sit in one Tailscale tailnet. Files sync via Syncthing; machines message each other through a Telegram group plus a Syncthing-shared "bus" folder. Every machine runs AI coding agents (Claude Code desktop app and CLI, OpenAI Codex CLI, others). Today OpenSSH works in a full mesh between our 5 machines: Windows via the built-in OpenSSH Server service, macOS either via Remote Login or a user-level sshd on port 2222 under launchd (no sudo), Linux via systemd. Keys are per-machine ed25519, public keys distributed over the sync bus, authorized only for registry machines. We already use SSH to: run a command on another node with an audit receipt, run PowerShell scripts on the Windows hub, and raise a VISIBLE new session in the Claude desktop app on a Mac by AppleScript over SSH (menu click + accessibility value). Known limit: an SSH session on Windows runs in a non-interactive session and cannot see or drive the user's desktop/GUI.

QUESTION
What can such a fleet realistically achieve through SSH, beyond "remote shell", and what is the best-practice way to do each? Cover these areas, with concrete tools, commands and known pitfalls:

1. REMOTE AND VISIBLE AI SESSIONS. Starting, resuming, steering and monitoring AI agent sessions (Claude Code, Codex CLI, similar) on another machine over SSH: tmux/screen/zellij persistence, headless vs interactive, "remote control" features of these tools, SSH-based remote development (VS Code Remote-SSH, JetBrains Gateway, Claude Code remote/SSH modes if any). How to make a session started remotely VISIBLE to the human on that machine's desktop: macOS (AppleScript/osascript, launchctl asuser, accessibility permissions, TCC limits for sshd), Windows (why SSH lands in a non-GUI session; scheduled task with "run only when user is logged on"/interactive token, PsExec -i, other legitimate ways), Linux.
2. ORCHESTRATION ACROSS NODES. Fan-out command execution and job dispatch over SSH: Ansible (incl. Windows over SSH), pssh/parallel-ssh, Fabric, pyinfra, Salt-SSH, Nix deploy tools; when SSH-based orchestration beats an agent/daemon model; queueing, locking, idempotency, audit trails of "who asked whom to do what". Using SSH as a transport for agent-to-agent protocols (MCP over SSH/stdio, A2A).
3. BACKUPS AND DATA MOVEMENT. rsync/restic/borg/kopia/rclone over SSH between heterogeneous OSes; SSH vs Syncthing for different data classes (large Obsidian vault ~130k notes, SQLite databases, model files); SFTP/SSHFS mounting; snapshot consistency for live SQLite.
4. SHARING GPU AND HEAVY COMPUTE OF THE HUB. Using a Windows GPU box as a remote worker over SSH: running inference (Ollama, llama.cpp, vLLM, whisper/ASR) on the hub from laptops; SSH port forwarding / tunnels to local model servers; WSL2 vs native Windows; job queues; pitfalls (Windows sshd session isolation, CUDA in services, sleep/power).
5. SECURITY OF KEYS AND ACCESS. Best practice for a family/small-team fleet: per-node keys vs SSH certificates (step-ca, Tailscale SSH, Teleport, Smallstep), forced commands and restricted keys for automation, agent forwarding risks, key rotation and revocation, logging/auditing, least privilege for AI agents that hold SSH access (prompt-injection risk: an agent reading untrusted text could be steered to run commands on other nodes), and how to give SSH access to team members' personal machines with informed consent and the ability to revoke.
6. RELIABILITY. Keeping sshd alive on laptops (sleep, lid, OS updates resetting settings), macOS updates breaking user-level sshd or TCC grants, Windows OpenSSH service updates, Tailscale MagicDNS vs IPs, health-check patterns.

For each area give: what is possible, the most mature tool(s) as of 2025-2026, a minimal working recipe, known failure modes, and whether it is worth doing for a fleet of this size.

STANDING REQUIREMENTS (apply throughout):

1. CITATIONS ARE MANDATORY. For every non-obvious factual claim, attach an inline source: working URL + publication date. At the end, list every source. If you cannot find a real source for a claim, write "[no source found]" and keep the claim clearly marked as unverified. NEVER invent a URL, a title, a date, or a quote. A fabricated citation is worse than an omitted one.
2. SINGLE-SOURCE FLAG. Any claim supported by only ONE source, or by a low-authority source (personal blog, anonymous social post, marketing page), must be explicitly tagged [single-source] or [low-authority].
3. CONFIDENCE TIER per key finding, using exactly these labels: [established] widely corroborated · [emerging] early but credible · [speculative] plausible, thin evidence · [fringe] outside consensus.
4. RECENCY. Prioritize the last 12-18 months (2025-2026). When the best available source is older, say so explicitly rather than implying it is current.
5. PRESERVE DISAGREEMENT. Where credible sources disagree, present BOTH sides with their source quality.
6. EPISTEMIC NEUTRALITY. Map evidence FOR and AGAINST rather than dismissing by default.
7. DECISION-ORIENTED ENDING. Close with a ranked list of the 5-7 highest-value SSH capabilities to build next for this fleet, each with effort, risk and payoff, plus one recommendation with its confidence tier and the main assumption it rests on.

Search the web extensively. Use 15-25 primary sources (official docs, vendor docs, GitHub repos, CVE/security advisories, reputable engineering blogs). Cite ONLY URLs you actually opened. Before the final report, list the searches you ran and the pages you opened.

Write the final report in Russian; keep commands, tool names and quotes in the original language.
