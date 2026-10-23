# DR26-07-06-HUB-06 — raw report from ubu

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR26-07-06-HUB-01 SSH access to a Hetzner Ubuntu VPS from Claude Desktop, VS Code, CLI, and phone

## Executive Summary

For your stack, the highest-confidence architecture is **standard OpenSSH on the VPS, carried primarily over Tailscale, with public port 22 closed at the Hetzner firewall by default**. That preserves compatibility with Claude Desktop SSH, VS Code Remote-SSH, plain terminal clients, and mobile SSH apps, while materially shrinking attack surface. Closing public 22 should **not** break Claude Desktop or VS Code if the client device itself can reach the VPS over Tailscale using a tailnet IP or MagicDNS name, because Claude Desktop supports SSH host resolution through local SSH config and explicitly allows `ProxyCommand`/`ProxyJump` patterns after `ssh -G` resolution, and VS Code Remote-SSH uses standard SSH targets and config files as well. Hetzner’s VNC console and Rescue system give you a viable break-glass path if you misconfigure SSH while traveling. citeturn10view3turn19view0turn27search1turn28search2turn28search6turn36search9

On the Claude side, the current official docs confirm that the Desktop app’s Code tab can run sessions **locally, in the cloud, or over SSH**, that SSH connections are stored in «внутренний путь лаборатории» or provisioned through managed `sshConfigs`, and that **SSH sessions read managed settings from the remote host**, while the Desktop app reads SSH host allowlists and preconfigured SSH connections from the **local** machine. The same docs also show that the Desktop and CLI share Claude configuration, MCP servers, hooks, skills, and settings, but not necessarily the same session history. Anthropic’s 2026 changelog further shows active work on Remote Control, mobile push notifications, and remote session crash recovery. The official gap is that Anthropic does **not** currently document the precise remote bootstrap internals, host-key handling internals, agent-forwarding behavior, or whether a preinstalled Claude CLI is strictly required versus auto-provisioned. citeturn10view0turn10view1turn10view3turn11view3turn8view1turn8view2turn9view0

For the VS Code failure, the pattern you described — **SSH itself works, VS Code hangs at “Initializing VS Code Server,” and no `~/.vscode-server` appears** — most often points to **server bootstrap never completing**, not to a base SSH problem. In a modern Ubuntu x86 Hetzner VM, the highest-probability causes are: remote download or local-download fallback failing; missing `curl`/`wget`/`tar`/`bash`; outbound HTTPS, proxy, CA, or DNS problems; install-path permission or `noexec` issues; or a newer client/extension regression around download, SCP transfer, or wrong binary selection. Ubuntu 20.04+ is officially supported; glibc problems usually hit older systems or misdetected platforms, but they are now a real regression class in 2025–2026 and should still be checked explicitly. citeturn14view0turn14view1turn14view2turn20view2turn20view3turn22view1turn23view1turn22view3

For phone access, the best current approach is **Tailscale on the phone plus a real SSH client**. On iOS, Blink remains the strongest terminal-native option if you want SSH plus Mosh. On iOS and Android, Termius remains the broadest cross-platform client, with SSH/Mosh/SFTP and sync across devices. As a browser fallback, **Tailscale SSH Console** is real and current in 2026, but it is still beta, admin-console initiated, and DERP-relayed. A web terminal such as `ttyd` can work well for emergency mobile access, but only **behind Tailscale Serve**, not Funnel, if the service is a shell. citeturn38view2turn24search1turn24search13turn26view0turn26view1

For long-lived Claude Code sessions, tmux still works, but the state of the art in Anthropic’s own product has moved toward **background sessions, agent view, workflows, and mobile push notifications**. If you keep the tmux farm, use tmux for persistence and out-of-band control, but increasingly use Claude’s native backgrounding for jobs that need fewer brittle `send-keys` injections. Anthropic’s docs also now have explicit tmux guidance: enable passthrough and extended keys, or you will lose desktop notifications, progress output, and proper multiline input behavior. citeturn33view0turn35view0turn35view1turn35view2turn35view3turn8view2

## Key Findings

### Claude Desktop SSH

The current Claude Desktop documentation explicitly says the Code tab can run **on your machine, in the cloud, or over SSH**, and that sessions are isolated conversations with their own history, project folder, and code changes. Anthropic also documents enterprise configuration for SSH: admins can preconfigure connections with `sshConfigs`, restrict connections with `sshHostAllowlist`, and those allowlist checks run **after** SSH config resolution via `ssh -G`, which means local SSH aliases, `ProxyCommand`, and `ProxyJump` are supported as long as the resolved hostname matches policy. That is the strongest official evidence that your Desktop app should tolerate Tailscale hostnames, SSH aliases, or even bastion patterns. citeturn10view0turn10view3

What the docs **do** say about remote state is important. SSH sessions read the managed settings file from the **remote host**. Meanwhile, the Desktop app itself reads `sshConfigs` and `sshHostAllowlist` from the **local** machine’s managed settings when the connection is created. Desktop and CLI also share Claude configuration such as `CLAUDE.md`, MCP servers in «внутренний путь лаборатории» or `.mcp.json`, hooks, skills, and settings; for SSH sessions that means the effective Claude environment is substantially determined by what exists on the remote machine, not on the local laptop, except for the SSH connection definition itself. citeturn10view1turn10view3turn11view3

The strongest official inference on remote prerequisites is negative rather than positive: Anthropic’s Desktop docs do **not** declare that you must preinstall Node.js or the Claude CLI on the remote host before using “Add SSH connection.” The docs instead position Desktop as running the same underlying engine as CLI and discuss remote/local/cloud session surfaces, but they stop short of documenting the SSH bootstrap sequence. The troubleshooting section does mention that if commands like `npm` or `node` are missing, you should verify PATH and shell profile setup, which implies remote sessions use the remote shell environment and expect project tooling to be present there. That means your VPS should be treated as a real Claude execution host with its own PATH, shell init, MCP config, and filesystem permissions. citeturn11view3

Anthropic’s 2026 changelog materially changes the picture versus older assumptions. Remote Control expanded in February 2026, mobile push notifications for Remote Control arrived in April 2026, `/context`, `/exit`, and `/reload-plugins` started working from Remote Control clients, remote MCP reconnect retries were enabled, and remote-session crash recovery was improved again in June 2026. Those changes strongly suggest Anthropic is investing in long-lived remote sessions and remote supervision rather than treating SSH as a thin novelty feature. citeturn8view1turn8view2turn9view0turn9view1

The official limitations are also concrete. Desktop lacks some CLI-only / terminal-first features, including scripting-oriented flags, agent teams, and terminal-dialog commands like `/permissions`, `/config`, and `/doctor`, which are unavailable in the Code tab and must be managed by editing files or using the standalone CLI. Desktop supports MCP servers, plugins, local and SSH file mentions, and images/PDF attachments, but third-party-provider support is constrained compared with the CLI unless you are in specific enterprise or research-preview paths. citeturn11view3

The most important unresolved gap is that Anthropic’s current public docs **do not document** the exact SSH implementation details you asked about: whether Desktop shells out to the platform SSH binary, how it handles host keys and `known_hosts`, whether it supports SSH agent forwarding, how identities are loaded internally, and whether a remote Claude bootstrapper is copied versus a preinstalled agent being reused. That should be treated as an official documentation gap, not guessed away. citeturn10view3turn11view3

### VS Code Remote-SSH

Microsoft’s current docs remain clear on fundamentals that directly map to your failure class. Remote-SSH installs a **VS Code Server** on the remote host; the server runs as the same user you used to sign in; installation requires a remote environment with `bash`, `curl` or `wget`, `tar`, OpenSSH server, and on Linux generally kernel `>= 4.18`, glibc `>= 2.28`, and libstdc++ `>= 3.4.25`. Ubuntu Server 20.04+ is explicitly listed as supported, which means a normal Hetzner Ubuntu x86 VPS is not a fringe target anymore. citeturn19view0turn14view1turn14view2

Microsoft’s troubleshooting guidance still says the first things to do for a hang are to **show the login terminal**, try `"remote.SSH.useLocalServer": false`, ensure TCP forwarding is enabled server-side, and verify the remote machine has internet access. The FAQ adds a critical point that is easy to miss: by default, Remote-SSH first tries to download the server **on the remote host**, and then can fall back to downloading locally and transferring remotely; you can force that behavior with `remote.SSH.localServerDownload`. In other words, the absence of `~/.vscode-server` is fully consistent with a bootstrap that never finished downloading, copying, or extracting the server. citeturn14view0turn14view1

The current failure landscape is broader in 2025–2026 than older blog posts suggest. There are now recent issue classes for misdetected server platform selection, regressions in manual installation paths, failures in local-download-plus-SCP copy logic, and configuration/data migration issues across newer Remote-SSH releases. One 2026 report shows Remote-SSH 0.122.0 incorrectly selecting `cli-alpine-x64` on Ubuntu 24.04, installing only a limited server, and never creating the expected full commit directory. Another 2025 issue shows local download plus SCP copy breaking on newer clients. Those are not proof that your host has the same exact bug, but they materially raise the probability that your symptom is a **client-side bootstrap regression**, not merely a server-side Linux defect. citeturn20view2turn20view3turn21search17turn21search19

At the same time, the old classic failure modes are still alive. Microsoft’s own docs cite missing remote internet / outbound HTTPS as a cause. Maintainer-triaged and user-reproduced issues still show `Permission denied` when the install script or server executable is placed on a `noexec` filesystem, and hangs or parse failures when `/tmp` or the chosen install path cannot execute. A separate path class is old or unsupported glibc, which became sharper in March 2025 as Remote-SSH stopped supporting older glibc targets such as 2.24 and now fails much earlier. citeturn14view0turn22view1turn23view1turn22view3turn14view2

A few specific 2026-era details matter for diagnosis. Newer Remote-SSH installations often use `~/.vscode-server/cli/servers/Stable-<commit>` rather than only the older `~/.vscode-server/bin/<commit>` mental model, so “I don’t see `~/.vscode-server/bin/...`” is no longer sufficient evidence that nothing happened. Some logs and bugs now show the CLI server path under `cli/servers/Stable-<commit>`, while other failure reports still reference the traditional `bin/<commit>` path for the full server. That means your diagnostic needs to inspect **both** trees. citeturn20view0turn20view1turn21search10turn21search4

The best current manual/offline recovery path is therefore pragmatic, not elegant. Extract the **client commit ID** from `code --version` or the Remote-SSH log, inspect whether the local client already downloaded the correct tarball, and if necessary force `"remote.SSH.localServerDownload": "always"` so VS Code downloads on the laptop and copies to the host. If that still fails, a manual placement matching the commit-specific server directory can work, but Microsoft explicitly says VS Code Server is designed to be managed by VS Code rather than installed as a standalone product, so offline manual install is still an unsupported workaround rather than a first-class path. citeturn18view0turn14view1turn20view1turn20view3

If you want a cleaner fallback, **Remote Tunnels** is now the most credible Microsoft-native alternative. It avoids SSH for transport, works with desktop and web clients, can be kept running as a service, and is explicitly pitched for machines where SSH support is limited or where browser-based access matters. Its trade-off is that it is a different access plane, authenticated with GitHub or Microsoft account, and Microsoft states a remote server instance is designed for a single user/client rather than collaborative multi-user access. citeturn29view0turn13search3

### Plain SSH, tmux, and mobile access

Your existing design — long-lived tmux sessions with agent CLIs inside them — is still valid, but Anthropic’s own 2026 product direction now gives you better primitives. `claude agents` is explicitly a unified view for background sessions, background sessions can keep running without a terminal attached, sessions pinned in `claude agents` stay alive when idle, and background jobs can be started directly, including shell commands prefixed with `!` in `claude agents` or `claude --bg --exec ...`. That does not replace tmux for operator control, but it does mean the “always-on agent box” pattern is becoming productized inside Claude Code itself. citeturn35view0turn35view2turn35view3turn35view1

If you keep tmux as the outer supervisor, Anthropic’s own tmux configuration is now mandatory, not optional. Their docs say that inside tmux, Shift+Enter for multiline prompts and desktop notifications / progress output both break by default. Their fix is to enable `allow-passthrough`, `extended-keys`, and `terminal-features 'xterm*:extkeys'`. Without that, your remote Claude sessions lose capabilities silently. citeturn33view0

For operator-grade tmux automation, the most important tmux man-page details are these: `send-keys` sends its arguments sequentially, treating each recognized token as a key name and everything else as character sequences; `pipe-pane` can mirror pane output to a command or inject command output into a pane; `remain-on-exit` can keep dead panes visible; and `respawn-pane` can relaunch a pane after a crash or clean exit. That makes a more reliable control plane than repeatedly firing blind `send-keys` into unknown pane state. The robust pattern is to separate **command injection**, **output capture**, and **pane lifecycle**, instead of relying on `send-keys` alone as both transport and state machine. citeturn39view0turn39view1turn39view2turn39view3

For phone access in 2026, the strongest stack is **Tailscale app + SSH client**. On iOS, Blink officially supports both SSH and Mosh and is still one of the few mobile terminals that behaves like a real operator terminal rather than a toy app. On iOS and Android, Termius remains the broadest unified client and is explicitly designed for cross-device productivity with encrypted vaults and mobile sync. citeturn38view2turn38view0turn24search5turn24search13

There is also now a genuine browser fallback: **Tailscale SSH Console**. It launches an SSH session in the browser from the Tailscale admin console, creates an ephemeral browser node, reauthenticates the user, and keeps the traffic end-to-end encrypted. The downsides are significant: it is still beta, it is restricted to owner/admin-class users, it must be initiated from the admin console, and all SSH Console traffic uses DERP relays. This makes it an excellent **break-glass travel fallback**, but not the primary daily workflow for your tmux farm. citeturn26view0

Mosh remains relevant, especially on shaky mobile networks. Its official site still describes the core advantages correctly: roaming across IP changes, intermittent connectivity survival, and predictive local echo. Eternal Terminal is a more niche alternative that also emphasizes automatic reconnection without interrupting the session, and it explicitly positions itself against autossh and Mosh while claiming better support for native scrolling and tmux control mode. The trade-off is ecosystem maturity: Mosh is more established; ET is interesting but less standard. citeturn31search0turn38view3

For a browser terminal, `ttyd` is the practical choice if you decide you need a “tap a URL and get a shell” fallback. The project remains active and is explicitly built to share a terminal over the web. But if the backend is an actual shell, this should sit behind **Tailscale Serve only**. Tailscale’s own docs distinguish Serve from Funnel: Serve is for access within the tailnet, Funnel is for internet exposure. Shell access through Funnel is a bad idea unless it is an intentionally internet-facing bastion with its own strong auth layer, logging, and compensating controls. citeturn24search2turn26view1

### Security architecture

For your exact situation — an always-on Hetzner anchor node, multiple client machines, month-long travel, and a desire for universal access without daily babysitting — **public 22 plus fail2ban is acceptable but not the best design anymore**. It is still a valid old-school baseline, and Fail2Ban still does what it has always done: watch auth logs and ban IPs after repeated failures. But leaving public 22 open guarantees constant scan noise, while your real trust plane is already Tailscale plus SSH keys. citeturn36search4turn36search10

A better fit is **SSH over Tailscale**, not necessarily **Tailscale SSH**. Tailscale’s docs are explicit that if Tailscale SSH is not a good fit, you can still run a normal SSH server over Tailscale as the network layer. That matters because Tailscale SSH changes authentication semantics: for tailnet-originated connections on port 22 it takes over the auth plane, uses node keys and policy, and is not a great fit for some multi-user machines or setups that rely on `authorized_keys` semantics and forced commands. Your VPS currently already uses an OpenSSH ed25519 key model with `root` plus non-root users and multiple operators/tools. That argues for preserving **standard OpenSSH auth** and using Tailscale primarily as the private transport fabric. citeturn37view0turn37view1

The hard compatibility question is whether closing public 22 breaks the tooling. For Claude Desktop, the answer is effectively **no**, provided the local laptop has Tailscale and the SSH target resolves to a reachable tailnet address or a valid proxy chain; the docs explicitly say `ProxyCommand` and `ProxyJump` work after `ssh -G` resolution. For VS Code, the answer is also **no** if the SSH target is reachable using standard SSH from the client machine; Remote-SSH is built on that assumption. For phones, again, **no** if the phone is on Tailscale and the SSH client knows the host. The only time public 22 still helps is when you are on a new or borrowed device that is not yet in your tailnet, or if Tailscale itself is unavailable on a client. citeturn10view3turn19view0turn25search2turn25search9turn25search4

For break-glass recovery, Hetzner Cloud gives you enough to be aggressive here. The VNC console lets you log in even when SSH is broken. Rescue mode can be enabled from the console, gives you a Linux-based rescue environment with a root password or SSH key, and lets you repair `sshd_config`, restore keys, or reset root password. Hetzner also documents changing SSH keys through Rescue if you lose access entirely. That is exactly the control-plane safety net you need if you close public 22 and later lock yourself out from the tailnet side. citeturn27search1turn27search2turn28search2turn28search3turn28search6

My verdict is therefore simple: **close public port 22 in the Hetzner Cloud firewall by default**. Keep OpenSSH listening on the server, but expose it only to the tailnet path. If you want an additional fallback, make it **temporary** and explicit: a disabled public-22 firewall rule you can enable briefly from Hetzner Console if the tailnet path is unavailable. That is a better trade than living on a permanently exposed SSH surface for a month. citeturn36search9turn36search12turn37view1

## Contradictions and Debates

The biggest practical debate is **Tailscale SSH versus ordinary SSH over Tailscale**. Tailscale SSH reduces key management and adds policy / check-mode benefits, but Tailscale’s own docs warn it may not suit multi-user machines, environments with per-user outbound SSH trust concerns, or systems that use `authorized_keys` to constrain commands. For your “anchor node with root plus non-root, multiple clients, and preexisting SSH key workflows,” ordinary OpenSSH over Tailscale is the lower-friction choice. You already solved key distribution; your bigger risk is exposure, not key ceremony. citeturn37view0turn37view1

A second debate is **Mosh versus plain SSH over Tailscale**. Mosh still wins on high-latency and lossy mobile networks, and its official advantages remain real. But Tailscale itself already smooths many network changes, and plain SSH inside tmux is often “good enough” once the network layer stops flapping. My view is: use Mosh as a **mobile specialist tool**, not your universal default. It adds UDP and server-package requirements, and it does not replace tmux or SSH features like forwarding. citeturn31search0turn37view1

A third debate is **VS Code Remote-SSH versus Tunnels versus web IDEs**. Remote-SSH keeps the most native desktop workflow and integrates best with standard SSH, but it is still vulnerable to server-bootstrap bugs, proxy friction, and client-extension regressions. Remote Tunnels removes SSH transport from the equation and is increasingly credible, especially for travel or tablet/browser access, but it introduces a different auth and service dependency plane. `code-server` and OpenVSCode Server are viable if your real requirement is “always reachable from a browser,” but they are not drop-in substitutes for Microsoft’s Remote-SSH feature set or extension compatibility. In particular, `code-server`’s own docs acknowledge differences from Microsoft VS Code and note marketplace differences. citeturn29view0turn13search2turn30search0turn30search2turn30search4turn30search6turn30search1

The last debate is whether your **tmux farm** should remain the center of gravity now that Anthropic has background sessions, agent view, workflows, and mobile push. My conclusion is hybrid. Keep tmux as the durable operator shell and for tools that are not yet first-class in Claude backgrounding. But reduce dependence on brittle “remote-control lines” where a typed prompt is the only control surface; Anthropic is clearly moving toward a richer native remote-agent model. citeturn35view0turn35view1turn35view2turn35view3turn8view2

## Best Practices

For **Claude Desktop SSH**, treat the VPS as a first-class Claude execution host. Put its Claude settings, MCP config, hooks, and project memory where you want them on the VPS, because the SSH session reads those from the remote machine. Standardize a remote working directory for Desktop SSH sessions through `startDirectory` if you use managed `sshConfigs`, and standardize the same shell PATH that you expect from the CLI. If you need policy boundaries, use `sshHostAllowlist` and external network controls together; Anthropic explicitly says the allowlist governs what Desktop connects to, not hard network egress. citeturn10view3turn11view3

For **VS Code Remote-SSH**, standardize one dedicated SSH alias for Remote-SSH to the VPS and keep its behavior boring. Use a normal shell, normal home directory, executable install path, `AllowTcpForwarding yes`, and a supported Ubuntu LTS baseline. If any bootstrap doubt exists, force `"remote.SSH.showLoginTerminal": true`, `"remote.SSH.useLocalServer": false`, and `"remote.SSH.localServerDownload": "always"` during diagnosis so you can see prompts and avoid remote-host download dependence. Also inspect both `~/.vscode-server/cli/servers` and `~/.vscode-server/bin`. citeturn14view0turn14view1turn14view2turn20view1

For **tmux-driven Claude sessions**, make three changes. First, apply Anthropic’s tmux passthrough config. Second, stop treating `send-keys` as the only orchestration primitive; use `pipe-pane` for capture and `remain-on-exit` plus `respawn-pane` for recovery. Third, where possible, shift long jobs into Claude background sessions and use `claude agents --json` or agent view for status instead of scraping a live terminal. citeturn33view0turn39view1turn39view2turn39view3turn35view0turn35view3

For **mobile**, prefer Tailscale + SSH app as the mainline path. Blink is the strongest iOS terminal-native choice if you value Mosh and keyboard-heavy workflows. Termius is the best cross-platform standardization choice if your goal is one host inventory and synced credentials across Windows, Mac, iPhone, and Android. Keep Tailscale SSH Console enabled as an admin break-glass route, not as your main operational interface. If you add a web terminal, keep it tailnet-only through Serve. citeturn38view2turn38view0turn24search5turn26view0turn26view1

For **security**, keep OpenSSH key-only, expose SSH only over Tailscale by default, and use Hetzner firewall rules as the outer network choke point. If you need a true emergency path, store a disabled public-22 allow rule or a narrow source-IP allowlist rule in Hetzner rather than leaving 22 open globally. Keep Hetzner Console / Rescue as the ultimate fallback. citeturn36search9turn36search12turn27search1turn28search2turn28search6

## Failure Modes

For your specific symptom class — **VS Code hangs at “Initializing VS Code Server” and there is no obvious `~/.vscode-server` install** — the ranked causes for a Hetzner Ubuntu x86 VPS are:

**Most likely: remote download path or remote outbound HTTPS / DNS / CA / proxy failure.** Microsoft’s docs still say the remote machine must have internet access for server and extension download unless the client downloads locally and transfers it. In practice, this is the first thing to suspect whenever SSH itself works but bootstrap does not. citeturn14view0turn14view1

**Very likely: local-download fallback not happening, or happening and then failing during copy.** This class is now common enough to be taken seriously, with 2025 issue reports showing local download plus SCP copy failures on newer Remote-SSH builds. Force `"remote.SSH.localServerDownload": "always"` to collapse the ambiguity. citeturn20view3turn15search13

**Likely: missing remote prerequisites (`bash`, `curl`/`wget`, `tar`, CA certs) or a broken shell environment.** Microsoft still documents these as prerequisites. Minimal images, stripped containers, or hardened shells still trigger this exact shape of failure. citeturn14view2

**Moderately likely: install path cannot execute or has bad permissions.** `noexec` on `/tmp` or `$HOME`, or a nonstandard home path with wrong permissions, still causes `Permission denied` during bootstrap or on `server.sh` launch. citeturn22view1turn23view1

**Moderately likely: SSH tunnel bootstrap restrictions.** If `AllowTcpForwarding` is disabled, Remote-SSH can connect but fail during the forwarding phase. Microsoft documents this explicitly. citeturn14view0

**Lower probability on Ubuntu 20.04+, but still real: wrong server binary chosen or prereq detection bug.** Recent issues show Remote-SSH misdetecting platforms and choosing the wrong Alpine/musl build, which then prevents creation of the expected full server directory. This is rarer on stock Ubuntu than on embedded or custom environments, but it is now a live regression class. citeturn20view2turn23view3

**Lower probability for your stack, but quick to eliminate: unsupported glibc / libstdc++.** Officially Ubuntu 20.04+ is supported, but if the box is actually older or heavily customized, glibc failures still present exactly as “failed to parse remote port” or “host may not meet prerequisites.” citeturn14view2turn22view3

**Unresolved from the sources reviewed:** I did not find a strong current maintainer-cited source that elevates **ControlMaster conflicts** to a top-tier cause for this exact symptom in 2025–2026. It remains plausible as practitioner lore, but I would not rank it highly here without logs. citeturn14view0

The shortest high-yield diagnostic sequence is:

1. Confirm the host is truly a supported Ubuntu version and has `bash`, `tar`, and either `curl` or `wget`. citeturn14view2  
2. In VS Code, enable `"remote.SSH.showLoginTerminal": true`, `"remote.SSH.useLocalServer": false`, and `"remote.SSH.localServerDownload": "always"`. Retry and watch the login/output terminal. citeturn14view0turn14view1  
3. On the remote host, inspect both `~/.vscode-server/cli/servers` and `~/.vscode-server/bin`, plus `~/.vscode-server/*.log`. citeturn20view1turn21search10  
4. Verify `/tmp` and the chosen install path are executable, and that the user’s home path is writable and executable. citeturn22view1turn23view1  
5. Verify outbound HTTPS to Microsoft endpoints from either the remote host or the local machine, depending on which download mode you force. citeturn14view1  
6. If Remote-SSH still stalls, pivot to Remote Tunnels temporarily so you can keep working while you isolate the bootstrap bug. citeturn29view0

## Strategic Recommendations

The **keep-or-close public port 22 verdict** is: **close it** at the Hetzner Cloud firewall for normal operation. Keep standard OpenSSH running on the VPS, but reachable only over Tailscale. This preserves compatibility with:
- Claude Desktop SSH on Macs and Windows PCs that have Tailscale,
- VS Code Remote-SSH using a tailnet IP / MagicDNS / SSH alias,
- plain CLI on all laptops,
- mobile SSH apps on a phone with Tailscale. citeturn10view3turn19view0turn25search2turn25search9turn25search4

Do **not** make Tailscale Funnel part of your shell-access path. Funnel is for intentionally exposing HTTPS services to the public internet. If you want a web terminal, put `ttyd` behind **Tailscale Serve** only, or behind a stronger app-specific gateway with its own auth. For an actual shell, tailnet-only exposure is the correct baseline. citeturn26view1turn24search2

For the SSH plane, keep **ordinary OpenSSH over Tailscale** as your default, not Tailscale SSH. You already have an ed25519 key-only setup. Tailscale SSH is powerful, but it changes the auth model and has caveats on multi-user machines and `authorized_keys`-based command restrictions. If you want an extra-high-assurance path later, consider Tailscale SSH only for a narrow admin use case such as `root` with check mode, not as the universal replacement for your existing OpenSSH setup. citeturn37view0turn37view1

For operations during travel, standardize two fallback planes:
- **Hetzner Console / Rescue** as your infrastructure break-glass path.
- **Tailscale SSH Console** as your identity-based browser fallback when you have no good SSH client handy. citeturn27search1turn28search2turn28search6turn26view0

For daily work, keep tmux, but reduce fragile prompt injection. Use tmux as the durable process container and out-of-band operator interface; use Anthropic’s native background sessions and push notifications for long Claude tasks; use VS Code either through fixed Remote-SSH once repaired or through Tunnels if the repair cost remains too high. citeturn33view0turn35view0turn35view1turn29view0

## Action Plan

**Phase one: stabilize the transport and access model**

- Put every laptop and phone that matters into the Tailscale tailnet. Use MagicDNS names for the VPS. citeturn25search2turn25search16
- In Hetzner Cloud Firewall, restrict inbound SSH so that public 22 is closed by default. Keep a disabled emergency rule template if you want a one-click fallback. citeturn36search9turn36search12
- Keep OpenSSH on the VPS exactly as your key-only server of record. Do **not** switch the whole box to Tailscale SSH as step one. citeturn37view0turn37view1
- Document the Hetzner break-glass procedure: VNC Console login, Rescue activation, root-password reset, and SSH-key replacement through Rescue. citeturn27search1turn28search2turn28search3turn28search6

**Phase two: normalize Claude Desktop SSH**

- Create one canonical SSH alias in local `~/.ssh/config` for the VPS using its Tailscale/MagicDNS target.
- Point Claude Desktop “Add SSH connection” at that host or alias, not at ad hoc raw IPs, so you can swap transport details in one place later. Anthropic explicitly supports SSH-config resolution and ProxyJump/ProxyCommand patterns via `ssh -G`. citeturn10view3
- Treat the VPS as the Claude config source of truth for SSH sessions: remote «внутренний путь лаборатории», `.mcp.json`, hooks, skills, and project memory should be curated there. citeturn11view3
- If the Desktop app is used heavily, test whether your remote sessions rely on any env vars that disable Remote Control or mobile push features, especially if you mix Claude.ai login with API-key environments. citeturn9view1turn8view2

**Phase three: fix VS Code or decisively replace the failing path**

- On the VPS, verify Ubuntu version and the presence of `bash`, `tar`, and `curl` or `wget`. citeturn14view2
- In VS Code, set:
  ```json
  {
    "remote.SSH.showLoginTerminal": true,
    "remote.SSH.useLocalServer": false,
    "remote.SSH.localServerDownload": "always"
  }
  ```
  Then retry and inspect the Remote-SSH output. citeturn14view0turn14view1
- On the host, inspect:
  ```bash
  ls -la ~/.vscode-server
  ls -la ~/.vscode-server/cli/servers || true
  ls -la ~/.vscode-server/bin || true
  ls -la ~/.vscode-server/*.log || true
  ```
  because current installs may use either `cli/servers/Stable-<commit>` or `bin/<commit>`. citeturn20view1turn21search10turn21search4
- If nothing changes, verify that neither `/tmp` nor the home/install path is mounted `noexec`, and confirm `AllowTcpForwarding yes` in `sshd_config`. citeturn22view1turn23view1turn14view0
- If Remote-SSH remains flaky after one disciplined pass, switch the team to **Remote Tunnels** as the fallback/editor path rather than wasting more time on a brittle bootstrap. citeturn29view0

**Phase four: harden and modernize the tmux farm**

- Apply Anthropic’s tmux config:
  ```tmux
  set -g allow-passthrough on
  set -s extended-keys on
  set -as terminal-features 'xterm*:extkeys'
  ```
  and reload the server. citeturn33view0
- Stop driving every session with raw `send-keys` only. Use `pipe-pane` for logging, `remain-on-exit` for postmortem visibility, and `respawn-pane` for recovery. citeturn39view0turn39view1turn39view2turn39view3
- For long tasks, prefer native Claude background sessions or `claude agents` where possible, and enable mobile push notifications when appropriate. citeturn35view0turn35view2turn35view3turn8view2
- For SSH clients, set keepalives such as `ServerAliveInterval` and `ServerAliveCountMax` on roaming clients; OpenSSH explicitly supports encrypted server-alive probes for this purpose. citeturn32search0turn32search15

**Phase five: mobile-first fallbacks**

- Standardize on **Blink** for iOS operators who want serious keyboard/Mosh workflows. citeturn38view2
- Standardize on **Termius** where cross-platform host-sync and one shared client workflow matter more than iOS-terminal purity. citeturn38view0turn24search13
- Keep **Tailscale SSH Console** enabled for admins as the browser fallback. citeturn26view0
- If you need URL-based shell access, deploy `ttyd` behind **Serve**, not Funnel. citeturn24search2turn26view1

## Sources and dates

Primary sources prioritized for this report were current official documentation and 2026 changelog material from Anthropic, Microsoft, Tailscale, and Hetzner, plus recent GitHub issue reports for concrete failure analysis:

- Anthropic Claude Code Desktop docs and enterprise SSH configuration, current page accessed July 2026. citeturn10view0turn10view1turn10view3turn11view3
- Anthropic Claude Code changelog with 2026 versioned entries, especially February–June 2026 remote/session changes. citeturn8view1turn8view2turn9view0turn9view1
- Anthropic terminal configuration docs for tmux passthrough and keyboard behavior. citeturn33view0
- Anthropic environment-variable and agent-view docs for background session behavior. citeturn35view0turn35view1turn35view2turn35view3
- Microsoft VS Code Remote-SSH, troubleshooting, FAQ, Tunnels, and Linux prerequisites docs; Linux prerequisites and Tunnels pages show a 2026 refresh date. citeturn14view0turn14view1turn14view2turn19view0turn29view0turn18view0
- Recent VS Code Remote-SSH GitHub issues from 2025–2026 covering wrong platform selection, local-download copy failures, and glibc regressions. citeturn20view2turn20view3turn22view3turn23view3turn21search17turn21search19
- Tailscale SSH, SSH Console, SSH-over-Tailscale, client CLI, and VS Code extension docs, validated January 2026 or later where indicated. citeturn37view0turn37view1turn26view0turn25search8turn26view3
- Hetzner Cloud docs for firewalling, console, rescue, password reset, and SSH-key recovery via rescue. citeturn36search9turn36search12turn27search1turn27search2turn28search2turn28search3turn28search6
- Official or project-home sources for mobile and reconnect tools: Blink, Termius, Mosh, ttyd, Eternal Terminal. citeturn38view2turn38view0turn24search13turn31search0turn24search2turn38view3

### Open questions and limitations

Anthropic’s public docs still leave some SSH implementation questions undocumented: exact remote bootstrap sequence, host-key storage behavior, whether Desktop shells out to the platform SSH binary, and whether a remote Claude runtime is auto-installed or must preexist. I did **not** find an official page that answers those precisely. citeturn10view3turn11view3

On the VS Code side, I found strong evidence for the main bootstrap failure classes, but I did **not** find a current official or maintainer-confirmed source elevating `ControlMaster` conflicts to a leading cause for your exact symptom. I would treat that as a lower-confidence hypothesis unless your logs specifically point there. citeturn14view0