---
dr_id: DR26-07-06-HUB-06
title: "SSH access to Hetzner Ubuntu VPS from Claude Desktop, VS Code, CLI, and phone"
date: 2026-07-06
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-06-HUB-06): SSH access to Hetzner Ubuntu VPS from Claude Desktop, VS Code, CLI, and phone

> Researched the best-practice architecture and troubleshooting steps for reliably and securely accessing a Hetzner Ubuntu VPS via SSH from Claude Desktop, VS Code Remote-SSH, terminal clients, and mobile devices.

## Ключевые выводы
- Best architecture: standard OpenSSH on the VPS reachable primarily over Tailscale, with public port 22 closed by default in the Hetzner firewall — this doesn't break Claude Desktop, VS Code, CLI, or mobile SSH as long as the client is on the tailnet.
- Claude Desktop's Code tab supports SSH sessions resolved via local SSH config, including ProxyCommand/ProxyJump after `ssh -G` resolution; enterprise `sshConfigs`/`sshHostAllowlist` are read from the local machine, but the SSH session itself reads managed Claude settings from the remote host.
- Anthropic's docs do NOT document SSH bootstrap internals: host-key/known_hosts handling, whether Desktop shells out to the platform SSH binary, agent-forwarding support, or whether the Claude CLI is auto-provisioned remotely vs must preexist — treated as a real documentation gap.
- VS Code hanging at 'Initializing VS Code Server' with no ~/.vscode-server almost always means the remote server bootstrap never completed, not a base SSH failure. Ranked causes: (1) remote download/outbound HTTPS/DNS/proxy failure, (2) local-download-then-SCP-copy fallback failing, (3) missing bash/curl/wget/tar, (4) noexec on /tmp or install path, (5) AllowTcpForwarding disabled, (6) wrong platform/binary auto-detected (e.g. Remote-SSH 0.122.0 misselecting cli-alpine-x64 on Ubuntu 24.04), (7) unsupported glibc/libstdc++ (rare on Ubuntu 20.04+).
- Newer VS Code Remote-SSH installs may live under ~/.vscode-server/cli/servers/Stable-<commit> instead of (or in addition to) the older ~/.vscode-server/bin/<commit> — diagnostics must check both paths.
- Recommended diagnostic VS Code settings: remote.SSH.showLoginTerminal=true, remote.SSH.useLocalServer=false, remote.SSH.localServerDownload='always'; if Remote-SSH stays broken, fall back to VS Code Remote Tunnels (different auth/transport plane, no SSH bootstrap dependency).
- Tailscale SSH changes the auth model (node keys/policy) and is a poor fit for a multi-user VPS (root + non-root users) already using authorized_keys-based OpenSSH — recommendation is to keep ordinary OpenSSH and use Tailscale purely as the private transport layer, not as the SSH auth mechanism.
- Best mobile stack: Tailscale app + a real SSH client — Blink (iOS, SSH+Mosh, terminal-native) or Termius (cross-platform, SFTP/sync). Tailscale SSH Console is a legitimate browser-based break-glass fallback but is still beta, admin-console-initiated, and DERP-relayed — not for daily use.
- Hetzner VNC Console + Rescue System provide a reliable break-glass path (root password reset, SSH key replacement, sshd_config repair) if SSH access is misconfigured while traveling.
- tmux remains valid for persistent sessions but requires Anthropic's passthrough config (allow-passthrough, extended-keys, terminal-features xterm*:extkeys) or loses desktop notifications/progress output/multiline input; Anthropic's product direction is shifting toward native background sessions, agent view, and mobile push notifications as a lower-friction alternative to raw tmux+send-keys.
- Tailscale Funnel must never be used for shell/terminal access (public internet exposure); a web terminal like ttyd should sit behind Tailscale Serve only (tailnet-internal).

## Рекомендации / решения
- Close public port 22 in the Hetzner Cloud firewall by default; keep OpenSSH running on the VPS reachable only via Tailscale, with a disabled emergency public-22 rule kept ready for manual enabling.
- Create one canonical SSH alias in ~/.ssh/config pointing at the VPS's Tailscale/MagicDNS address, and point both Claude Desktop's 'Add SSH connection' and VS Code Remote-SSH at that same alias.
- Curate the VPS's own «внутренний путь лаборатории», MCP config, hooks, skills, and project memory directly on the remote host — Claude Desktop SSH sessions read Claude config from the remote machine, not the local laptop.
- For the VS Code hang: verify Ubuntu version + bash/curl/tar present, set remote.SSH.showLoginTerminal/useLocalServer/localServerDownload for diagnosis, inspect both ~/.vscode-server/cli/servers and ~/.vscode-server/bin, check noexec on /tmp and AllowTcpForwarding in sshd_config; if unresolved, switch to VS Code Remote Tunnels as the editor path.
- Apply Anthropic's tmux passthrough config (allow-passthrough, extended-keys, terminal-features) and replace blind send-keys automation with pipe-pane (capture), remain-on-exit and respawn-pane (recovery); migrate long-running jobs to native Claude background sessions / claude agents where possible.
- Standardize mobile access on Tailscale + Blink (iOS) or Termius (cross-platform); keep Tailscale SSH Console enabled only as an admin break-glass fallback, not the primary workflow.
- Document and rehearse the Hetzner break-glass procedure (VNC console login, Rescue mode activation, root password reset, SSH key replacement) before relying on the tailnet-only SSH design during travel.
- Keep OpenSSH key-only auth as the standard; do not switch the VPS's whole auth model to Tailscale SSH given its multi-user/root+non-root setup.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, Microsoft, Tailscale, Hetzner
- **Продукты/инструменты:** Claude Desktop, Claude Code, Claude CLI, VS Code, VS Code Remote-SSH, VS Code Remote Tunnels, OpenSSH, Tailscale SSH, Tailscale SSH Console, Tailscale Serve, Tailscale Funnel, Hetzner Cloud Firewall, Hetzner VNC Console, Hetzner Rescue System, tmux, Mosh, Eternal Terminal, Blink, Termius, ttyd, fail2ban, code-server, OpenVSCode Server

## Открытые вопросы
- Exact SSH bootstrap sequence, host-key/known_hosts handling, agent-forwarding support, and whether Claude Desktop auto-provisions a remote Claude runtime remain undocumented by Anthropic.
- Whether the specific Hetzner VPS is affected by the newer VS Code Remote-SSH regressions (wrong platform/binary auto-selection, local-download+SCP copy failures) is unconfirmed without direct logs from that host.
- No official or maintainer-confirmed source elevates SSH ControlMaster conflicts as a leading cause of the 'Initializing VS Code Server' hang — treated as low-confidence practitioner lore pending log evidence.

## Источник
- DR-ID `DR26-07-06-HUB-06` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- ssh-access-architecture
- tailscale-networking
- vscode-remote-ssh-troubleshooting
- claude-desktop-ssh
- tmux-session-management
- hetzner-vps-security
- mobile-ssh-clients
- always-on-hub-machine
