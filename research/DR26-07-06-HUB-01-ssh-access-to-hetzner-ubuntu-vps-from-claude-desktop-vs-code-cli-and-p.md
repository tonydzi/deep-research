---
dr_id: DR26-07-06-HUB-01
title: "SSH access to Hetzner Ubuntu VPS from Claude Desktop, VS Code, CLI, and phone"
date: 2026-07-06
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-06-HUB-01): SSH access to Hetzner Ubuntu VPS from Claude Desktop, VS Code, CLI, and phone

> The report determines the safest, most compatible architecture for reaching a Hetzner VPS from Claude Desktop, VS Code Remote-SSH, terminal, and mobile, and diagnoses why VS Code Remote-SSH hangs at server bootstrap.

## Ключевые выводы
- Standard OpenSSH on the VPS carried over Tailscale, with public port 22 closed at the Hetzner firewall, is the highest-confidence architecture — it doesn't break Claude Desktop, VS Code Remote-SSH, CLI, or mobile SSH apps as long as the client is on the tailnet.
- Claude Desktop's Code tab supports SSH sessions (local/cloud/SSH), resolves hosts via local SSH config with ProxyCommand/ProxyJump after `ssh -G`, and reads managed settings (sshConfigs, sshHostAllowlist) from the LOCAL machine, while the actual Claude session settings/MCP/hooks/skills are read from the REMOTE host — so the VPS must be a fully configured Claude execution host.
- Anthropic's docs do not document the SSH bootstrap internals (host-key handling, agent forwarding, whether a Claude runtime is auto-provisioned remotely) — an acknowledged official documentation gap.
- VS Code hanging at 'Initializing VS Code Server' with no ~/.vscode-server is most likely a bootstrap failure, not a base SSH problem: causes ranked by likelihood are (1) remote download/outbound HTTPS/DNS/proxy failure, (2) local-download-fallback/SCP copy failure (a known 2025-2026 regression class), (3) missing bash/curl/wget/tar, (4) noexec on /tmp or install path, (5) AllowTcpForwarding disabled, (6) wrong server binary chosen (e.g. cli-alpine-x64 misdetection on Ubuntu 24.04), (7) unsupported glibc/libstdc++ (Ubuntu 20.04+ is officially supported).
- Newer VS Code Remote-SSH installs may live under ~/.vscode-server/cli/servers/Stable-<commit> instead of the older ~/.vscode-server/bin/<commit> — diagnostics must check both paths.
- Tailscale SSH (vs. plain SSH over Tailscale) changes the auth model (node keys/policy) and is a poor fit for a multi-user box with root + non-root users and existing authorized_keys/key workflows — ordinary OpenSSH over Tailscale-as-transport is the better choice here.
- Hetzner Cloud VNC console + Rescue system provide a genuine break-glass path (root password reset, SSH key replacement) if SSH is misconfigured while traveling.
- Tailscale SSH Console is a real 2026 browser-based fallback but is beta, admin-console-initiated, DERP-relayed, and restricted to owner/admin — good for emergencies, not daily use.
- For mobile, Tailscale + a real SSH client is the strongest stack: Blink (iOS, SSH+Mosh, terminal-native) and Termius (iOS/Android, cross-platform sync) are the top picks; a ttyd web terminal must sit behind Tailscale Serve, never Funnel, if serving a shell.
- Anthropic's 2026 roadmap (Remote Control, mobile push notifications, background sessions via `claude agents`, remote crash recovery) is shifting long-lived remote work away from raw tmux `send-keys` toward native backgrounding — but tmux remains valid if Anthropic's passthrough config (allow-passthrough, extended-keys) is applied, since without it multiline input and desktop notifications silently break.

## Рекомендации / решения
- Close public port 22 in the Hetzner Cloud firewall by default; keep OpenSSH running but reachable only over Tailscale (tailnet IP/MagicDNS).
- Do not switch fully to Tailscale SSH — keep standard OpenSSH key-based auth as the system of record, using Tailscale purely as the private transport layer.
- Create one canonical SSH alias in local ~/.ssh/config pointing at the VPS's Tailscale/MagicDNS name; point both Claude Desktop's 'Add SSH connection' and VS Code Remote-SSH at that same alias.
- Curate the VPS's own Claude config (CLAUDE.md, .mcp.json, hooks, skills, PATH/shell profile) since SSH sessions read settings from the remote host, not the laptop.
- To fix/diagnose the VS Code hang: set remote.SSH.showLoginTerminal=true, remote.SSH.useLocalServer=false, remote.SSH.localServerDownload='always'; inspect both ~/.vscode-server/cli/servers and ~/.vscode-server/bin plus logs; verify bash/curl-wget/tar present, /tmp and install path not noexec, and AllowTcpForwarding yes in sshd_config.
- If Remote-SSH remains flaky after one disciplined diagnostic pass, fall back to VS Code Remote Tunnels (GitHub/Microsoft-account auth, no SSH transport) rather than continuing to fight the bootstrap bug.
- Apply Anthropic's required tmux config (allow-passthrough on, extended-keys on, terminal-features xterm*:extkeys) to avoid silently losing notifications/progress output/multiline input in remote Claude sessions.
- Move tmux orchestration off pure blind send-keys: use pipe-pane for output capture, remain-on-exit + respawn-pane for pane lifecycle/recovery; shift long-running jobs to native Claude background sessions (`claude agents`/`--bg --exec`) where possible.
- Keep a disabled/emergency public-22 firewall rule template in Hetzner for one-click fallback, plus documented Hetzner VNC/Rescue break-glass steps (root password reset, SSH key swap).
- Standardize mobile access on Tailscale + Blink (iOS/Mosh-heavy use) or Termius (cross-platform sync); keep Tailscale SSH Console enabled as an admin-only browser fallback, not primary workflow.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, Microsoft, Hetzner, Tailscale
- **Продукты/инструменты:** Claude Desktop, Claude Code CLI, Claude Agents (background sessions), VS Code Remote-SSH, VS Code Server, VS Code Remote Tunnels, code-server, OpenVSCode Server, OpenSSH, Tailscale, Tailscale SSH, Tailscale SSH Console, Tailscale Serve, Tailscale Funnel, Hetzner Cloud Firewall, Hetzner VNC Console, Hetzner Rescue System, tmux, Mosh, Eternal Terminal, ttyd, Blink (iOS), Termius, Fail2Ban, Ubuntu Server (20.04+/24.04)

## Открытые вопросы
- Anthropic does not publicly document Claude Desktop's exact SSH bootstrap sequence, host-key/known_hosts handling, whether it shells out to the platform SSH binary, or whether a remote Claude runtime is auto-installed vs. must pre-exist.
- No official or maintainer-confirmed source elevates SSH ControlMaster conflicts as a leading cause of the VS Code 'Initializing VS Code Server' hang for 2025-2026 — plausible practitioner lore only, not verified without log evidence.
- Whether the specific Hetzner VPS in question is affected by the newer VS Code Remote-SSH platform-misdetection regression (e.g. wrongly selecting cli-alpine-x64) has not been confirmed against its actual logs.

## Источник
- DR-ID `DR26-07-06-HUB-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории» (2).md`

## Связано
- tailscale-networking
- claude-code-remote-sessions
- vscode-remote-ssh-troubleshooting
- hetzner-vps-hardening
- tmux-agent-orchestration
- mobile-ssh-access
