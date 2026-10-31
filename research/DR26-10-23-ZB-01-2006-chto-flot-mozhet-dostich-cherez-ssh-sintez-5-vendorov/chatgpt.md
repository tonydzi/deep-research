# DR26-10-23-ZB-01-2006 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR26-10-23-ZB-01-2006 — Что частный флот из восьми машин реально может получить через SSH в 2025–2026

## Поисковый журнал и открытый корпус

Исследование выполнено по состоянию на **23 октября 2026 года, Europe/Lisbon**. Основной корпус ограничен **25 первичными источниками**: официальная документация Anthropic, OpenAI, Microsoft, Apple, Ansible, MCP/A2A, SQLite, Tailscale, Smallstep, OpenBSD/OpenSSH и официальные проектные документы. Там, где лучшая официальная документация существенно старше 12–18 месяцев, это указано явно.

### Какие поиски были выполнены

Повторные уточняющие варианты и `site:`-фильтры ниже сведены в нормализованный журнал:

| Область | Поисковые запросы |
|---|---|
| AI-сессии | `Claude Code Remote Control SSH remote sessions official`; `Claude Code desktop SSH official`; `OpenAI Codex CLI resume remote app-server official`; `Codex remote TUI app-server WebSocket`; `tmux attach new-session persistence official`; `Zellij sessions attach official`; `VS Code Remote SSH official`; `JetBrains Gateway SSH remote development 2026` |
| macOS / Windows GUI | `macOS Remote Login Accessibility Automation TCC sshd official`; `launchctl asuser Apple official`; `Windows OpenSSH GUI interactive session`; `Windows Session 0 interactive services official`; `Task Scheduler interactive token official`; `PsExec -i interactive session official` |
| Оркестрация | `Ansible Windows SSH ansible-core 2.18 official`; `parallel-ssh official`; `Fabric SSH official`; `pyinfra SSH idempotent official`; `Salt-SSH official`; `Nix deploy remote SSH official` |
| Протоколы агентов | `MCP transports stdio SSH official 2026`; `A2A protocol JSON-RPC HTTP SSE official` |
| Бэкапы | `restic SFTP SSH official`; `Borg SSH Windows support official`; `Kopia SFTP Windows macOS Linux official`; `rclone SFTP official`; `SSHFS official`; `Syncthing conflicts official` |
| SQLite | `SQLite live backup SSH sqlite3_rsync official`; `SQLite WAL copying live database corruption official`; `VACUUM INTO live backup official` |
| GPU | `Ollama Windows NVIDIA API official`; `vLLM Windows WSL official`; `llama.cpp CUDA server official`; `whisper.cpp CUDA server official`; `CUDA WSL Windows official` |
| SSH security | `OpenSSH authorized_keys restrict forced command official`; `OpenSSH agent forwarding risks official`; `Smallstep step-ca SSH certificates official`; `Tailscale SSH official`; `Teleport SSH certificates session recording official` |
| Agent security | `Claude Code prompt injection security official`; `Codex prompt injection security official`; `OpenAI designing agents resist prompt injection 2026` |
| Reliability | `Tailscale MagicDNS official 2026`; `macOS systemsetup remote login sleep official`; `Windows OpenSSH service official 2025` |

### Страницы, которые были фактически открыты и составляют доказательную базу

Открыты и используются далее как S1–S25:

S1 Anthropic Claude Code Remote Control; S2 Anthropic Claude Code Desktop; S3 OpenAI Codex Developer Commands; S4 OpenAI Codex App Server; S5 tmux Getting Started; S6 VS Code Remote SSH; S7 JetBrains Gateway; S8 Apple Privacy & Security; S9 Apple `systemsetup`; S10 Microsoft Interactive Services; S11 Microsoft Task Scheduler `LogonType`; S12 Microsoft Sysinternals PsExec; S13 Ansible Windows SSH; S14 MCP transports specification 2026-07-28; S15 A2A specification; S16 restic SFTP repository docs; S17 SQLite Backup API; S18 Syncthing synchronization/conflicts; S19 Ollama Windows; S20 vLLM GPU installation; S21 OpenSSH `sshd(8)`; S22 OpenSSH `ssh(1)`; S23 Tailscale SSH; S24 Smallstep `step-ca` SSH certificates; S25 OpenAI prompt-injection architecture.

Инструменты `parallel-ssh`, Fabric, pyinfra, Salt-SSH, Nix deploy, Borg, Kopia, rclone, SSHFS, llama.cpp, whisper.cpp и Teleport также были найдены на этапе поиска. Чтобы не выйти за требование **15–25 primary sources**, их страницы не включены в доказательный корпус; поэтому сравнительные выводы о них ниже помечены как **архитектурная оценка**, а не как отдельные документированные продуктовые факты.

## Архитектурный вывод для именно этого флота

### Главный тезис

[established] Для восьми частных машин SSH разумно рассматривать не как «remote shell», а как **универсальную authenticated transport/control plane** для пяти разных классов задач:

1. запуск и присоединение к долгоживущим AI-процессам;
2. диспетчеризация команд и idempotent configuration;
3. безопасная передача данных и создание backup snapshots;
4. туннелирование локальных GPU/API-сервисов;
5. транспорт для узких машинных API — в том числе MCP-over-stdio и HTTP/A2A через SSH forwarding.

Но SSH **не должен становиться** ни универсальным GUI-remoting протоколом, ни очередью задач, ни системой распределённых блокировок, ни backup format, ни identity-management системой. Эти функции лучше строить поверх него отдельными минимальными слоями. Это особенно важно для флота, где часть машин — intermittently-online laptops, а часть принадлежит другим людям.

Практическая архитектура, которую я бы выбрал:

```text
                         ┌──────────────────────────┐
                         │ Hetzner VPS coordinator │
                         │                          │
                         │ inventory / policy      │
                         │ Ansible                 │
                         │ job queue               │
                         │ audit receipts          │
                         │ health state            │
                         └─────────────┬────────────┘
                                       │ SSH over Tailscale
                  ┌────────────────────┼────────────────────┐
                  │                    │                    │
             macOS nodes          Windows nodes         GPU hub
             tmux/Claude          OpenSSH              Ollama
             Codex/GUI broker     task broker          WSL2/vLLM
                  │                    │                    │
                  └────────── Syncthing working bus ──────┘
                                       │
                                restic snapshots
                                       │
                              independent backup
```

[established] Основное преимущество такого разделения — coordinator хранит **intent и audit**, SSH исполняет, а узел остаётся authoritative за собственным состоянием. Это значительно лучше, чем разрешить каждому AI-агенту держать unrestricted ключи ко всем остальным компьютерам.

### Где SSH выигрывает, а где нет

| Задача | SSH-подход | Вердикт для 8 узлов |
|---|---|---|
| Rare/bursty remote commands | прямой SSH | **идеально** |
| Persistent terminal agents | SSH + tmux / native agent resume | **строить сейчас** |
| Idempotent config | Ansible over SSH | **строить сейчас** |
| GPU API | localhost service + `ssh -L` | **строить сейчас** |
| Backup transport | SFTP/SSH под restic | **строить сейчас** |
| SQLite live backup | SQLite snapshot first, SSH second | **обязательно** |
| Human-visible macOS GUI | local GUI-session broker triggered over SSH | **полезно, но узко** |
| Human-visible Windows GUI | pre-created interactive Scheduled Task | **полезно, но узко** |
| Job queue | coordinator queue → SSH workers | **да; не использовать SSH как queue** |
| Continuous metrics/telemetry | daemon/agent | **SSH хуже** |
| Millisecond RPC | HTTP/gRPC/A2A directly over tailnet | **SSH хуже** |
| Team identity at scale | cert/identity layer | **переходить постепенно** |

[established] Для вашего размера флота **не нужен Kubernetes, Nomad или полноценный Salt master** только ради dispatch. Восемь узлов достаточно малы, чтобы coordinator + Ansible + SQLite job ledger оставались понятнее и легче аудировались. Это уже архитектурная оценка, а не утверждение из документации.

## Удалённые и видимые AI-сессии

### Terminal-first: SSH + tmux остаётся базовым примитивом

[established] [single-source] `tmux` позволяет создать session, отсоединить client, оставить процессы работающими и затем подключить другой client; `new-session -A` либо присоединяется к существующей сессии, либо создаёт её. Официальный проект перечисляет Linux и macOS среди поддерживаемых платформ, но не native Windows. Источник S5, дата публикации страницы не указана: <https://github.com/tmux/tmux/wiki/Getting-Started>. citeturn22view4

Минимальный стандарт для всех Unix/macOS узлов:

```bash
# Человек входит на удалённый узел и создаёт/возвращает persistent workspace:
ssh -t mac-studio 'cd ~/src/project && tmux new-session -A -s project-agent'

# Либо создать session detached:
ssh mac-studio \
  'tmux new-session -d -s project-agent -c "$HOME/src/project"'

# Позже присоединиться:
ssh -t mac-studio 'tmux attach -t project-agent'
```

Для fleet policy я бы ввёл единый namespace:

```text
tmux session:
  ai-<project>-<agent>

examples:
  ai-vault-claude
  ai-backend-codex
  ai-infra-shell
```

и никогда не управлял бы активным агентом через необработанный `tmux send-keys`, если у продукта есть собственный resume/remote-control API. `send-keys` годится для emergency automation, но слишком легко отправить Enter не в тот prompt, подтвердить опасную операцию или попасть в shell вместо AI TUI.

### Claude Code: Remote Control уже лучше многих SSH-хаков

[established] [single-source] Claude Code Remote Control в текущей документации запускает Claude **на исходной машине**, а web/mobile interface становится удалённым окном в эту локальную сессию. Команда `claude --remote-control` создаёт обычную интерактивную terminal session, к которой одновременно можно обращаться удалённо; после sleep/network interruption она reconnects, когда машина возвращается, но сам компьютер и процесс Claude должны снова быть доступны. Источник S1, дата публикации на странице не указана: <https://code.claude.com/docs/en/remote-control>. citeturn22view0

Практически:

```bash
ssh -t mac1 'cd ~/src/foo && tmux new-session -A -s ai-foo-claude'
```

Внутри:

```bash
claude --remote-control "foo@mac1"
```

Получается одновременно три control surfaces:

```text
SSH → tmux → local Claude process
          ↘ browser / claude.ai
          ↘ Claude mobile app
```

Это лучше, чем искусственно пытаться «проецировать» TUI через нестабильный SSH connection: tmux обеспечивает process lifetime, а Claude Remote Control — удобное steering UI.

[emerging] [single-source] Anthropic Desktop теперь также прямо документирует возможность запускать Claude Code **на локальной машине, в cloud или over SSH**. Это отдельный путь от AppleScript-клика по уже открытой desktop session и потенциально может убрать часть вашей самодельной GUI-автоматизации. Источник S2, дата публикации не указана: <https://code.claude.com/docs/en/desktop>. citeturn22view1

**Стоит ли:** да, очень высокий ROI. Для macOS/Linux я бы сделал `tmux + Claude Remote Control` default session model, а desktop AppleScript оставил только там, где действительно требуется *видимое на физическом экране* окно.

### Codex: resume, headless exec и remote TUI

[established] [single-source] Текущий Codex CLI умеет возобновлять сохранённые sessions по UUID/имени, `--last`, а non-interactive `exec resume` может немедленно получить новый prompt; JSONL event output предназначен для машинной обработки. Источник S3, дата публикации страницы не указана: <https://learn.chatgpt.com/docs/developer-commands>. citeturn22view2

Для одноразовой headless работы:

```bash
ssh mac1 '
  cd ~/src/foo &&
  codex exec resume --last --json \
    "Continue the pending task; stop after tests complete"
'
```

Для человека — persistent TUI:

```bash
ssh -t mac1 'cd ~/src/foo && tmux new-session -A -s ai-foo-codex'
```

затем:

```bash
codex resume --last
```

[emerging] [single-source] Codex App Server документирует настоящий **remote terminal UI mode**: server на одной машине слушает WebSocket, а Codex CLI на другой подключается через `--remote`. Источник S4, дата публикации не указана: <https://learn.chatgpt.com/docs/app-server>. citeturn22view3

Безопасный вариант — не выставлять WebSocket в tailnet вообще:

```bash
# На hub:
codex app-server --listen ws://127.0.0.1:4500
```

На laptop:

```bash
ssh -N \
  -L 4500:127.0.0.1:4500 \
  hub
```

В другом терминале laptop:

```bash
codex --remote ws://127.0.0.1:4500
```

Так SSH остаётся authentication/encryption layer, а App Server видит только loopback.

**Стоит ли:** `codex resume` — да немедленно. App Server remote TUI — [emerging], я бы сначала использовал на 1–2 узлах и не делал fleet-critical API, пока interface продолжает развиваться.

### VS Code и JetBrains: это remote development, а не «GUI на удалённом дисплее»

[established] [single-source] VS Code Remote-SSH открывает remote folder на машине с SSH server и запускает remote-side components, в то время как UI остаётся на локальном компьютере. Источник S6, дата публикации не указана: <https://code.visualstudio.com/docs/remote/ssh>. citeturn22view5

Это хороший режим для:

```text
MacBook UI
    ↓ SSH
Windows/Linux/macOS workspace
    ↓
local files / build / tests / agents on remote node
```

но **не** делает IDE видимой человеку, сидящему перед удалённым монитором.

[established] [single-source] JetBrains Gateway аналогично создаёт SSH connection, устанавливает/использует backend на remote server и показывает проект через JetBrains Client. Официальная страница обновлена **14 августа 2026 года**. Источник S7: <https://www.jetbrains.com/help/idea/remote-development-a.html>. citeturn22view6

Для этого флота я бы стандартизировал **VS Code Remote-SSH**, если команда уже живёт в VS Code; Gateway имеет смысл только для проектов, где IDE-specific indexing/refactoring JetBrains действительно окупает ещё один remote backend.

### macOS: как сделать результат видимым физическому пользователю

[established] [single-source] Apple отдельно контролирует **Accessibility** — разрешение приложению выполнять scripts/system commands для управления Mac — и **Automation** — доступ одного приложения к управлению другими приложениями. То есть наличие SSH login само по себе не означает разрешение управлять GUI. Источник S8, дата публикации страницы не указана: <https://support.apple.com/en-vn/guide/mac-help/mchl211c911f/mac>. citeturn22view7

Ваш текущий путь:

```text
SSH
  → osascript
    → Accessibility / Automation
      → Claude Desktop
```

реален, поскольку вы уже экспериментально подтвердили его на своих Mac. Но я бы **не делал `sshd` универсальным GUI-controller**.

Лучший operational pattern:

```text
SSH command
   │
   ▼
narrow request
   │
   ▼
GUI broker running as LaunchAgent
inside logged-in user's GUI session
   │
   ├─ allowlisted action: open-claude
   ├─ allowlisted action: focus-session <id>
   └─ allowlisted action: open-terminal <project>
             │
             ▼
        osascript / app API
```

Пример request, который SSH имеет право создать:

```json
{
  "schema": 1,
  "request_id": "01J...",
  "action": "open-claude",
  "project": "foo",
  "session": "ai-foo-claude",
  "requested_by": "vps-coordinator"
}
```

GUI broker должен отвергать произвольные AppleScript strings и принимать лишь enum действий.

[speculative] Для `launchctl asuser <uid> ...`: он полезен как механизм запуска в пользовательском bootstrap namespace, но я **не нашёл в открытом официальном Apple-корпусе источник**, подтверждающий, что `launchctl asuser` переносит или обходит TCC/Accessibility identity. Поэтому предположение «достаточно обернуть `osascript` в `launchctl asuser` и TCC исчезнет» считать нельзя. **[no source found]**.

Это важный практический вывод: воспринимайте `launchctl asuser` как **session-routing tool, не TCC bypass**.

[established] [single-source] Apple продолжает документировать включение Remote Login через `systemsetup -setremotelogin on`; официальная страница не содержит даты публикации. Источник S9: <https://support.apple.com/tr-tr/guide/remote-desktop/apd95406b8d/mac>. citeturn22view8

**Стоит ли:** да для 2–3 конкретных visible actions. Нет для универсального «remote GUI automation framework».

### Windows: почему SSH-процесс не появляется на desktop

[established] Windows deliberately separates service/non-interactive execution from logged-in interactive desktop. Microsoft по-прежнему документирует, что services по умолчанию не должны работать interactively; это фундаментальная причина, почему процесс, стартованный через service-hosted control path, нельзя считать процессом текущего desktop user. Источник S10 — старый, **7 января 2021 года**, но это лучший открытый официальный архитектурный документ в исследованном наборе: <https://learn.microsoft.com/en-us/windows/win32/services/interactive-services>. citeturn22view9

Поэтому:

```powershell
ssh hub "notepad.exe"
```

может создать процесс, но это **не обещает видимое окно на текущем пользовательском desktop**.

#### Предпочтительный bridge: Scheduled Task с interactive token

[established] [single-source] `TASK_LOGON_INTERACTIVE_TOKEN` требует, чтобы пользователь **уже был logged on**, и task тогда выполняется в существующей interactive session. Документ Microsoft датирован **25 августа 2021 года**, то есть старше целевого окна, но механизм остаётся официально документированным. Источник S11: <https://learn.microsoft.com/en-us/windows/win32/taskschd/principal-logontype>. citeturn22view10

Создайте task один раз локально:

```text
\Fleet\OpenClaude
Security option:
    Run only when user is logged on

Action:
    powershell.exe -NoProfile -ExecutionPolicy Bypass
      -File C:\Fleet\Start-ClaudeVisible.ps1
```

Пример `Start-ClaudeVisible.ps1`:

```powershell
$repo = 'C:\src\foo'

Start-Process `
    -FilePath 'wt.exe' `
    -ArgumentList @(
        'new-tab',
        'powershell.exe',
        '-NoExit',
        '-Command',
        "Set-Location '$repo'; claude"
    )
```

Из SSH:

```powershell
schtasks.exe /Run /TN "\Fleet\OpenClaude"
```

Это лучше, чем пытаться из каждого remote command вручную получать user token: security boundary один раз создаётся человеком/администратором, а SSH имеет только право **trigger** заранее определённой операции.

#### PsExec `-i`: рабочий fallback, не default

[established] [single-source] Sysinternals PsExec официально поддерживает `-i` для взаимодействия с desktop указанной session; официальный материал датирован **11 апреля 2023 года**, то есть также старше 18 месяцев. Источник S12: <https://learn.microsoft.com/en-us/sysinternals/downloads/psexec>. citeturn22view11

Например:

```powershell
query user
```

затем:

```powershell
PsExec.exe -accepteula -i 1 -d `
    powershell.exe -NoProfile -File C:\Fleet\Start-ClaudeVisible.ps1
```

Я бы использовал это как break-glass/debug path. Для постоянной fleet automation Scheduled Task лучше: меньше произвольной cross-session magic и проще audit.

### Linux GUI

Для CLI-агентов Linux проще всех: SSH + tmux полностью решает persistence и visibility через локальный terminal attach. Для настоящего Wayland/X11 GUI я бы **не пытался унифицировать это через SSH**: запускайте GUI-событие через process уже внутри пользовательской graphical session либо используйте remote-desktop protocol. Для вашего Linux VPS GUI вообще не нужен.

**Итог по visible sessions:** [established] общий abstraction должен быть не `remote_start_process()`, а:

```text
start_headless_job()
start_persistent_terminal_session()
request_visible_gui_action()
```

Это три разных capability с разными security boundaries.

## Оркестрация, dispatch и agent-to-agent protocols

### Ansible — наиболее рациональный control plane

[established] [single-source] Ansible официально поддерживает Windows targets через SSH начиная с **ansible-core 2.18**; документация прямо позиционирует SSH как альтернативу WinRM. Источник S13, дата публикации текущей страницы не указана: <https://docs.ansible.com/projects/ansible/latest/os_guide/windows_ssh.html>. citeturn22view12

Для вашего флота это меняет картину: один VPS может управлять Linux, macOS и Windows через уже имеющуюся SSH infrastructure.

Минимальная структура:

```ini
# inventory.ini

[linux]
vps

[mac]
macbook
macmini
imac

[windows]
hub
win-laptop

[owned:children]
linux
mac
windows
```

Отдельные host vars:

```yaml
# host_vars/hub.yml
ansible_connection: ssh
ansible_shell_type: powershell
ansible_user: anton
```

Playbook должен выражать **state**, а не 20 строк shell:

```yaml
- name: Ensure fleet directories exist on Windows
  hosts: windows
  gather_facts: false
  tasks:
    - name: Create fleet directories
      ansible.windows.win_file:
        path: C:\Fleet
        state: directory
```

И Unix:

```yaml
- name: Ensure fleet directories exist on Unix
  hosts: linux:mac
  gather_facts: false
  tasks:
    - name: Create fleet directory
      ansible.builtin.file:
        path: "{{ ansible_env.HOME }}/.fleet"
        state: directory
        mode: "0700"
```

Практически Ansible должен отвечать за:

```text
sshd configuration
authorized principals / keys
agent wrapper versions
health scripts
backup config
GUI broker deployment
GPU service config
scheduled tasks / launch agents
```

а не за собственно очередь AI-jobs.

### Что делать с pssh, Fabric, pyinfra, Salt-SSH и Nix

Ниже — **архитектурная оценка fit**, а не отдельные factual claims из 25-source corpus:

| Инструмент | Где имеет смысл | Мой выбор для этого флота |
|---|---|---|
| `parallel-ssh` / `pssh` | быстрый blast одной команды | оставить как emergency utility |
| Fabric | orchestration логика естественно пишется на Python | удобно для bespoke workflows, но не второй config-management layer |
| pyinfra | нужен более лёгкий Pythonic declarative layer | разумная замена Ansible, но не одновременно с ним |
| Salt-SSH | уже существует Salt ecosystem | для 8 личных машин слишком много второго operational model |
| Nix deploy tools | машины уже Nix/NixOS-managed | отлично для Nix nodes, плохая universal abstraction для Win/mac fleet |

**Рекомендация:** выбрать ровно один declarative control system. Здесь — Ansible, поскольку Windows-over-SSH теперь официально поддерживается и ваша транспортная инфраструктура уже существует. citeturn22view12

### SSH сам по себе не является очередью

[established] Это важное архитектурное ограничение. Команда:

```bash
ssh hub long-running-job
```

даёт remote execution, но не даёт вам durable semantics:

```text
queued
claimed
running
cancel-requested
completed
failed
retryable
owner
deadline
dependency graph
```

Для этого я бы держал **одну SQLite queue на always-on Hetzner VPS**, а workers представлял не daemon-ами на laptop, а SSH executors.

Минимальная таблица:

```sql
CREATE TABLE jobs (
    id TEXT PRIMARY KEY,
    created_at TEXT NOT NULL,
    requested_by TEXT NOT NULL,
    agent_session TEXT,
    target TEXT NOT NULL,
    capability TEXT NOT NULL,
    args_json TEXT NOT NULL,
    state TEXT NOT NULL,
    not_before TEXT,
    started_at TEXT,
    finished_at TEXT,
    exit_code INTEGER,
    receipt_path TEXT
);
```

Coordinator делает:

```text
BEGIN IMMEDIATE
  find queued job for online target
  mark claimed
COMMIT

ssh target fleet-run <job-id> <capability> ...
```

Таким образом laptop может быть asleep шесть часов, а job не исчезает.

### Locking

Для каждого ресурса блокировка должна быть **локальна его authoritative owner**:

```text
GPU model-loader lock       → hub
Obsidian index rebuild lock → node owning index
backup prune lock           → backup repository owner
GUI action lock             → GUI broker
```

Не делайте Syncthing-файл `LOCK` универсальной distributed lock: eventual file synchronization не даёт вам нужной atomic acquisition semantics.

На Unix wrapper может использовать:

```bash
flock /var/lock/fleet/gpu.lock fleet-gpu-job ...
```

На Windows аналогичную роль лучше выполнять mutex/lock внутри PowerShell/runner process.

### Audit: записывать надо не только «кто SSH-пользователь»

Для AI fleet полезный receipt:

```json
{
  "schema": 1,
  "request_id": "01J...",
  "requested_by_human": "anton",
  "requested_by_node": "vps",
  "agent": "codex",
  "agent_session_id": "019...",
  "target_node": "hub",
  "capability": "gpu.transcribe",
  "argv_sha256": "sha256:...",
  "cwd": "C:\\Fleet\\jobs\\01J...",
  "git_commit": "abc1234",
  "policy_version": "2026-10-23.3",
  "started_at": "2026-10-23T17:30:21Z",
  "finished_at": "2026-10-23T17:31:02Z",
  "exit_code": 0,
  "stdout_sha256": "sha256:...",
  "stderr_sha256": "sha256:..."
}
```

Ключевой принцип: audit должен различать:

```text
human identity
      ↓
agent/session identity
      ↓
coordinator identity
      ↓
SSH credential
      ↓
target capability
```

Иначе после инцидента вы увидите только «key from vps ran PowerShell», что слишком мало.

### MCP over SSH: да, но это композиция, а не стандартный MCP transport

[established] [single-source] Текущая MCP specification от **28 июля 2026 года** определяет transport как binding, отделённый от protocol semantics. SSH не надо объявлять новым MCP protocol: можно использовать SSH как carrier для stdio-oriented server process. Источник S14: <https://modelcontextprotocol.io/specification/2026-07-28/basic/transports>. citeturn22view13

Практический config:

```json
{
  "mcpServers": {
    "hub-fleet-tools": {
      "command": "ssh",
      "args": [
        "-T",
        "hub",
        "/opt/fleet/bin/mcp-server"
      ]
    }
  }
}
```

Плюсы:

```text
no new listening TCP port
existing SSH identity
existing authorization
stdio naturally crosses SSH channel
easy forced-command restriction
```

Минус: каждый MCP session зависит от SSH process lifetime.

[emerging] Я бы использовал это именно для **low-rate privileged tools**:

```text
gpu_submit
backup_snapshot
fleet_status
agent_start
agent_status
```

а не для high-throughput token streaming между всеми агентами.

### A2A: туннелировать, а не переделывать в stdio

[established] [single-source] Текущая A2A specification определяет JSON-RPC binding как **JSON-RPC 2.0 over HTTP(S)** и SSE для streaming. Поэтому «A2A over raw SSH stdio» уже будет нестандартным transport adaptation. Источник S15, дата публикации страницы не указана: <https://a2a-protocol.org/latest/specification/>. citeturn22view14

Правильная композиция:

```bash
ssh -N \
  -L 8787:127.0.0.1:8787 \
  remote-agent-host
```

а A2A client обращается:

```text
http://127.0.0.1:8787
```

То есть:

```text
A2A stays HTTP
SSH supplies encrypted/authenticated tunnel
```

Это лучше, чем придумывать собственный framing и затем терять interoperability.

## Бэкапы, перемещение данных и SQLite

### Syncthing и backup решают разные задачи

[established] [single-source] Syncthing умеет обнаруживать concurrent edits и создаёт `sync-conflict-*` copy, если один файл был независимо изменён на двух устройствах. Это полезная file-sync semantics, но не является snapshot/versioned backup semantics само по себе. Источник S18, дата публикации страницы не указана: <https://docs.syncthing.net/users/syncing.html>. citeturn22view17

Поэтому для вашего флота:

```text
Syncthing = current working state / replication
restic    = historical recovery / snapshots
SSH/SFTP  = transport
```

Нельзя считать наличие трёх Syncthing replicas полноценным backup: ошибочное удаление или изменение — тоже состояние, которое система синхронизирует.

### Restic через SFTP — самый простой единый backup layer

[established] [single-source] restic официально поддерживает repository через SFTP/SSH; repository содержит собственные encrypted backup objects и metadata. Источник S16, дата публикации текущей страницы не указана: <https://restic.readthedocs.io/en/latest/030_preparing_a_new_repo.html>. citeturn22view15

На VPS:

```bash
sudo install -d -m 0700 -o backup -g backup /srv/restic/fleet
```

На node:

```bash
export RESTIC_REPOSITORY='sftp:backup@vps:/srv/restic/fleet'
export RESTIC_PASSWORD_FILE="$HOME/.config/restic/fleet.password"

restic init
```

Backup:

```bash
restic backup \
    "$HOME/Documents" \
    "$HOME/src" \
    "$HOME/.config/fleet"
```

Проверка:

```bash
restic snapshots
restic check
```

Я бы сделал разные repositories либо как минимум разные credentials для:

```text
personal machines
team machines
VPS infrastructure
GPU/model cache metadata
```

и **не синхронизировал repository password через общий Syncthing bus**.

### Что делать с 130k-note Obsidian vault

Для большого vault:

```text
live vault
   ↓
Syncthing between machines that actively edit it
   ↓
periodic restic snapshot from one or more nodes
```

Не надо rsync-ить vault по кругу одновременно с Syncthing. Один replication authority на класс данных сильно снижает число конфликтных механизмов.

Для 130k mostly-small files backup system с content-defined deduplication гораздо полезнее, чем создавать очередную full copy вручную. Конкретную performance-оценку вашего vault без измерения размера, inode/file distribution и churn дать нельзя.

### SQLite — особый случай: никогда не считать живой `.db` обычным файлом

[established] [single-source] SQLite Online Backup API специально предназначен для live databases и формирует consistent snapshot. Официальная SQLite документация от **13 ноября 2025 года** также прямо перечисляет `VACUUM INTO` и `sqlite3_rsync`; последний копирует live SQLite database между системами через SSH. Источник S17: <https://www.sqlite.org/backup.html>. citeturn22view16

Правильная pipeline:

```text
live SQLite
    │
    ├─ Online Backup API / .backup
    │
    └─ VACUUM INTO
          ↓
consistent standalone snapshot
          ↓
restic / SFTP / Syncthing
```

Например:

```bash
sqlite3 app.db \
  ".backup '/var/tmp/app.snapshot.db'"
```

или:

```bash
sqlite3 app.db \
  "VACUUM INTO '/var/tmp/app.snapshot.db';"
```

Только **после** этого:

```bash
restic backup /var/tmp/app.snapshot.db
```

или:

```bash
scp /var/tmp/app.snapshot.db backup@vps:/srv/snapshots/
```

Это особенно важно при WAL mode: резервировать active database через generic file synchronizer — плохая abstraction. Используйте SQLite-aware snapshot first.

### SSH/SFTP, rsync, rclone, Borg, Kopia, SSHFS — где место каждого

Здесь сравнительный verdict — архитектурная оценка; отдельные страницы этих инструментов не входят в ограниченный 25-source corpus.

| Инструмент | Для этого флота |
|---|---|
| `scp` / SFTP | разовая передача небольшого числа файлов |
| `rsync` | прекрасный one-way delta copy на Unix; на Windows потребуется дополнительная Unix-compatible среда **[no source found в финальном корпусе]** |
| restic | мой default для backup |
| Borg | не вводить параллельно restic без конкретной причины |
| Kopia | рассматривать вместо restic, не вместе с ним |
| rclone | хорошая transport/sync utility при появлении object/cloud storage; не нужен только ради SSH |
| SSHFS | ad-hoc browsing/mount; не делать authoritative location для SQLite |
| Syncthing | active distributed working set |

Для model files, которые по сути immutable blobs:

```text
model-v1.gguf
model-v2.gguf
```

я предпочёл бы one-way distribution:

```bash
rsync / SFTP / rclone
```

а не full-mesh Syncthing, если каждой машине не нужна полная копия.

### Backup topology

Для небольшого флота я бы сделал:

```text
Mac/Windows/Linux
       │
       │ restic over SFTP
       ▼
Hetzner / dedicated backup target
       │
       ├── snapshot retention
       └── optional second independent/offsite replica
```

Но если Hetzner VPS содержит coordinator secrets, **не делайте его единственной копией backup repository и единственным holder backup credentials**. Иначе compromise coordinator превращается одновременно в compromise control plane и recovery plane.

**Стоит ли:** [established] да, это одна из наиболее полезных SSH-возможностей во всём проекте.

## Windows GPU hub как общий вычислительный узел

### Самый простой слой: persistent model server + SSH tunnel

[established] [single-source] Ollama текущей документацией поддерживается как native Windows application с NVIDIA/AMD GPU и по умолчанию предоставляет API на `http://localhost:11434`. Источник S19, дата публикации страницы не указана: <https://docs.ollama.com/windows>. citeturn22view18

Это почти идеально под ваш hub.

На Windows hub:

```powershell
ollama serve
```

На laptop:

```bash
ssh -N \
    -L 11434:127.0.0.1:11434 \
    hub
```

Локальный клиент ноутбука обращается к:

```text
http://127.0.0.1:11434
```

С точки зрения приложения GPU теперь выглядит локальным:

```text
Laptop app
   │ localhost:11434
   ▼
SSH encrypted tunnel
   │
   ▼
Windows hub:11434
   │
   ▼
NVIDIA GPU(s)
```

Ключевой security benefit — Ollama не надо bind-ить на весь Tailscale interface; control остается через SSH authentication.

### Не запускайте новую model process на каждый запрос

Для interactive inference лучше:

```text
one persistent service
+ warmed models
+ HTTP requests
```

чем:

```text
ssh hub "ollama run ..."
ssh hub "ollama run ..."
ssh hub "ollama run ..."
```

SSH здесь должен быть **control/tunneling plane**, а inference protocol — API самого inference server.

Для batch jobs — Whisper/transcoding/indexing — coordinator queue может запускать отдельные headless workers через SSH.

### vLLM: WSL2 — официальный путь на Windows, native Windows — нет

[established] [single-source] Документация vLLM v0.12.0 от **2 декабря 2025 года** указывает Linux как поддерживаемую OS для GPU path, прямо говорит, что native Windows не поддерживается, и рекомендует WSL как способ запуска vLLM на Windows. Источник S20: <https://docs.vllm.ai/en/v0.12.0/getting_started/installation/gpu/>. citeturn22view19

Следовательно:

```text
Native Windows:
    Ollama
    llama.cpp-class workloads
    whisper.cpp-class workloads

WSL2:
    vLLM
    Linux-first CUDA stacks
    Linux containers
```

Конкретные llama.cpp/whisper.cpp capabilities в этой строке — архитектурная классификация; их официальные repos были найдены поиском, но не включены в 25-source evidence corpus.

Запуск из SSH session:

```powershell
wsl.exe -d Ubuntu -- bash -lc '
  cd ~/services/vllm &&
  ./start-vllm.sh
'
```

После запуска проверить с Windows:

```powershell
curl.exe http://127.0.0.1:8000/v1/models
```

и только после успешного local test туннелировать:

```bash
ssh -N \
    -L 8000:127.0.0.1:8000 \
    hub
```

### Две GPU: не предполагать автоматическую агрегацию

[established] Для RTX 5060 Ti-class dual-GPU hub нельзя считать, что любой inference engine автоматически превратит две карты в «одну большую GPU». Multi-GPU allocation зависит от конкретного backend/model/runtime. В текущем 25-source корпусе я не собирал достаточно источников по каждой версии Ollama/llama.cpp/vLLM для общего утверждения о policy распределения, поэтому точная multi-GPU стратегия здесь **[no source found]**.

Практически я бы сделал benchmark matrix:

```text
job type          GPU0 only   GPU1 only   both
------------------------------------------------
LLM interactive
LLM batch
embeddings
Whisper
image model
```

и предпочёл один из двух patterns:

```text
A. one large model spans both GPUs
```

или, часто проще операционно,

```text
B. GPU0 → LLM service
   GPU1 → ASR/embedding/background queue
```

если конкретные модели помещаются в VRAM одной карты.

### GPU job queue

Не давайте каждому laptop unrestricted:

```bash
ssh hub powershell.exe ...
```

Вместо этого coordinator должен видеть capability:

```json
{
  "capability": "gpu.infer",
  "model": "qwen...",
  "priority": 50,
  "max_runtime_s": 900
}
```

или:

```json
{
  "capability": "gpu.transcribe",
  "input": "job://01J.../audio.wav"
}
```

На hub:

```text
gpu-runner
  ├─ validates model allowlist
  ├─ checks free disk/VRAM
  ├─ takes GPU lock
  ├─ starts job
  ├─ emits receipt
  └─ releases lock
```

Это особенно важно, если command requester — AI agent.

### Windows SSH session isolation здесь скорее плюс

CLI/CUDA inference **не нужно быть visible**. Поэтому Windows non-interactive SSH execution, мешающий GUI automation, для GPU worker фактически нормален: heavy compute должен быть headless.

GUI-bound tools следует выводить в отдельный Scheduled Task bridge; CUDA/API workers — не смешивать с ним.

### Sleep — главный reliability killer GPU hub

GPU hub должен иметь отдельный power policy:

```text
AC power → no automatic sleep
display sleep → allowed
hibernation → deliberate only
automatic reboot/update → controlled window
```

Конкретные Windows 11 power-policy значения в текущий 25-source corpus не входят, поэтому OS-specific claim о каждом setting помечаю **[no source found]**. Operationally это нужно проверить N=1 после каждого крупного Windows update.

**Стоит ли:** очень высокий ROI. Из всех SSH-функций после remote agents это, вероятно, самая заметная отдача от имеющегося hardware.

## Безопасность ключей и эксплуатационная надёжность

### Нынешняя схема per-machine Ed25519 разумна — но не давать каждому ключу full mesh authority

При восьми машинах полный directed mesh потенциально имеет:

```text
8 × 7 = 56
```

отношений `source → target`. Это ещё управляемо технически, но становится плохо обозримо, когда добавляются:

```text
humans
AI agents
backup identities
GUI automation
GPU tunnel identities
team-owned nodes
```

Проблема уже не в количестве ключей, а в смешении полномочий.

Я бы разделил identities:

```text
id_human_anton
id_coordinator_config
id_agent_dispatch
id_backup_writer
id_gpu_tunnel
id_gui_trigger
```

а не:

```text
id_macbook_has_access_to_everything
```

### Forced commands и restricted keys — ключевой security primitive

[established] [single-source] OpenSSH `authorized_keys` поддерживает `restrict`, который отключает PTY, agent forwarding, X11 forwarding, port forwarding и `~/.ssh/rc`; также OpenSSH имеет forced-command/access restriction primitives. Текущая OpenBSD manual page не публикует отдельную дату. Источник S21: <https://man.openbsd.org/sshd.8>. citeturn22view20

Например ключ AI coordinator:

```text
restrict,command="/usr/local/bin/fleet-agent-gateway" ssh-ed25519 AAAA... coordinator-agent
```

Вместо arbitrary shell gateway получает:

```text
stdin:
{
  "capability": "backup.snapshot",
  "args": {...}
}
```

и разрешает только:

```text
agent.start
agent.status
gpu.submit
gpu.status
backup.snapshot
health.read
```

Это гораздо сильнее, чем prompt-level инструкция:

> «не выполняй опасные SSH-команды».

### Туннельный ключ должен быть отдельным от command key

`restrict` блокирует port forwarding, что правильно для command key.

Для GPU tunnel нужен отдельный credential с максимально узким `PermitOpen`/аналогичным restriction:

```text
gpu-tunnel-key
    may connect only to hub
    may forward only 127.0.0.1:11434
    no shell
    no PTY
    no agent forwarding
```

Не используйте один и тот же unrestricted private key для:

```text
remote shell
GPU forwarding
backup
GUI trigger
```

### Agent forwarding: по умолчанию выключить

[established] [single-source] OpenSSH `ssh -A` включает forwarding connection к authentication agent. Это создаёт дополнительную trust dependency на remote host; для AI-agent fleet нет причины делать его default. Источник S22, дата manual page не указана: <https://man.openbsd.org/ssh.1>. citeturn22view21

Default:

```sshconfig
Host *
    ForwardAgent no
```

Если узлу B надо затем обратиться к C, лучше:

```text
coordinator → C directly
```

или SSH jump/proxy routing, а не:

```text
coordinator → B
               ↳ borrowed agent → C
```

### Prompt injection превращает unrestricted SSH key в fleet-wide RCE capability

[established] OpenAI в материале от **11 марта 2026 года** подчёркивает, что сложные prompt-injection attacks нельзя надёжно решить только классификатором входа; архитектура должна ограничивать **impact даже при успешной манипуляции agent**. Это ровно ваш threat model: агент читает README, issue, web page или файл из чужого repo и затем обладает SSH authority к другим узлам. Источник S25: <https://openai.com/index/designing-agents-to-resist-prompt-injection/>. citeturn22view24

Поэтому самый важный security boundary:

```text
untrusted text
     ↓
LLM
     ↓
policy decision           ← may fail
     ↓
OS capability boundary    ← must still hold
     ↓
SSH forced-command key
     ↓
allowlisted operation
```

Не:

```text
untrusted text
     ↓
LLM
     ↓
ssh root@all-nodes
```

#### Для AI-agent identity я бы запрещал

```text
sudo / Administrator
arbitrary remote shell
write to authorized_keys
agent forwarding
access to password stores
access to other users' home dirs
arbitrary port forwarding
direct access to team-member PCs
```

а разрешал отдельными capabilities только то, что реально нужно.

Это, вероятно, важнее, чем переход с Ed25519 static keys на certificates.

### Tailscale SSH: очень привлекательный human-access layer, но не полный replacement

[established] [single-source] По состоянию на страницу, last validated **5 января 2026 года**, Tailscale SSH интегрирует SSH authorization с tailnet access controls, автоматически управляет ephemeral WireGuard-related identity и поддерживает SSH session recording. Источник S23: <https://tailscale.com/docs/features/tailscale-ssh>. citeturn22view22

Но для вашего флота есть важная граница: Tailscale SSH server support не является универсальной заменой Windows OpenSSH. Поэтому архитектуру вида:

```text
turn off all OpenSSH
use only Tailscale SSH everywhere
```

я бы не выбирал.

Лучшее применение:

```text
human access to eligible Mac/Linux nodes → Tailscale SSH
automation / forced-command identities  → OpenSSH
Windows                                 → existing OpenSSH
```

Но два параллельных auth models увеличивают cognitive load. Для 8 машин это оправдано только если Tailscale policy substantially упрощает team access.

### step-ca / SSH certificates

[established] [single-source] Smallstep документирует `step-ca` как SSH certificate authority для user и host OpenSSH certificates; tutorial обновлён **3 февраля 2026 года**. Источник S24: <https://smallstep.com/docs/tutorials/ssh-certificate-login/>. citeturn22view23

SSH certificates меняют управление с:

```text
N users × N hosts authorized_keys entries
```

на:

```text
hosts trust CA
CA issues short-lived identity assertions
```

Для вашего флота переход оправдан, когда появляется хотя бы один из факторов:

```text
regular team-member churn
many temporary identities
role-based access
need for short-lived credentials
revocation pain
```

Пока всё принадлежит 1–3 стабильным людям, правильно генерируемый `authorized_keys` из registry остаётся проще.

### Tailscale SSH vs step-ca vs Teleport

| Подход | Для вашего флота | Основная цена |
|---|---|---|
| per-node Ed25519 + generated `authorized_keys` | **сейчас самый простой** | manual/static credential lifecycle |
| Tailscale SSH | сильный human access для поддерживаемых hosts | mixed model из-за Windows |
| step-ca | лучший следующий шаг для SSH-native PKI | надо эксплуатировать CA |
| Teleport | рассматривать только при жёсткой потребности в централизованном access governance/session audit | для 8 private nodes, вероятно, слишком большой operational layer |

Строка Teleport — архитектурная оценка; detailed 2026 Teleport documentation не вошла в ограниченный 25-source corpus, поэтому дальнейшие конкретные feature claims о нём здесь намеренно не делаются.

### Team-owned personal machines: trust должен быть односторонне отзывным

Правильный consent model:

```text
team member owns machine
        │
        ├─ explicitly opts into fleet access
        ├─ knows exactly which principal is authorized
        ├─ knows which capabilities are exposed
        └─ can revoke locally without asking fleet admin
```

Я бы **не включал team PCs в ваш existing owner full mesh**.

Лучше:

```text
VPS coordinator
       │
       ├─ restricted role → teammate-win-1
       ├─ restricted role → teammate-win-2
       └─ restricted role → teammate-mac-1
```

а ваши personal nodes не получают туда прямые keys.

Для GUI actions дополнительное правило:

```text
remote command may REQUEST visible action
local user/session remains authority to permit it
```

### Revocation

Для static keys:

```text
remove key from registry
regenerate authorized_keys
Ansible deploy
terminate existing sessions if necessary
```

Для Tailscale SSH access — policy layer централизует authorization. Для step-ca — short-lived certs уменьшают lifetime украденного credential. citeturn22view22turn22view23

Не задавал бы искусственный «rotate every 30/90 days» interval без threat model. Намного важнее:

```text
short-lived human credentials
immediate revoke on compromise
separate machine and capability identities
no shared private keys
```

### Public keys можно синхронизировать; private keys — нет

Ваш Syncthing bus пригоден для:

```text
public keys
node registry
host fingerprints
policy files
signed job receipts
```

Не для:

```text
private SSH keys
restic repository password
CA signing key
API secrets
Tailscale auth keys
```

CA private key особенно не должен лежать на generic coordinator filesystem вместе с произвольными AI workspaces.

### Reliability: считать laptop intermittently available по определению

[established] SSH не исправляет sleep. На macOS Apple документирует Remote Login как системную capability, но устройство всё равно должно быть в состоянии принимать connection; управление power state — отдельная проблема. Источник S9, дата страницы не указана: <https://support.apple.com/tr-tr/guide/remote-desktop/apd95406b8d/mac>. citeturn22view8

Coordinator должен различать минимум:

```text
ONLINE
TAILNET_OFFLINE
SSH_PORT_DOWN
AUTH_FAILED
HEALTH_COMMAND_FAILED
BUSY
SLEEP_EXPECTED
UNKNOWN
```

а не просто `up/down`.

### Health check с VPS

Каждые 1–5 минут для owned always-expected nodes:

```bash
ssh \
  -o BatchMode=yes \
  -o ConnectTimeout=5 \
  -o ServerAliveInterval=3 \
  -o ServerAliveCountMax=2 \
  hub 'echo ok'
```

Не надо alert после одного missed probe. Laptops естественно sleep.

Сохраняйте:

```json
{
  "node": "hub",
  "observed_at": "2026-10-23T17:44:00Z",
  "dns_ok": true,
  "tcp22_ok": true,
  "ssh_auth_ok": true,
  "command_ok": true,
  "latency_ms": 43
}
```

Для hub/VPS failure threshold может быть 2–3 probes; для ноутбуков — presence/state, а не incident.

### macOS update risk

Ваш user-level `sshd :2222` + launchd + Accessibility path не является обычным Apple-supported Remote Login topology. Поэтому после major macOS update я бы автоматически прогонял smoke suite:

```text
port 2222 reachable?
key auth works?
tmux launch works?
GUI broker alive?
Accessibility action succeeds?
Automation action succeeds?
Claude visible action succeeds?
```

Утверждение, что конкретный macOS 26 update обязательно сбрасывает TCC permission для вашего custom user sshd, в официальном корпусе не подтверждено: **[no source found]**. Это следует рассматривать как operational risk to test, а не как установленный факт.

### Windows update risk

После Windows/OpenSSH updates:

```powershell
Get-Service sshd
```

и smoke test из VPS должны быть обязательны. Не привязывайте fleet health к тому, что Windows desktop app случайно запущен; headless services должны иметь собственный startup lifecycle.

### MagicDNS vs IP

Для конфигурации рекомендую abstract hostnames:

```sshconfig
Host hub
    HostName hub.<tailnet>.ts.net
    User anton

Host mac1
    HostName mac1.<tailnet>.ts.net
    Port 2222
```

а в inventory хранить stable logical node ID:

```yaml
node_id: hub
hostname: hub.адрес частной сети
```

не IP как identity.

Tailscale SSH itself integrates with tailnet naming/access model, но exact MagicDNS operational details не включены в финальные 25 источников; поэтому преимущество имени над IP здесь — архитектурная рекомендация, а не отдельный vendor claim. citeturn22view22

## Приоритеты внедрения и полный список источников

### Что строить следующим

| Ранг | Capability | Усилие | Риск | Payoff | Вердикт |
|---|---|---:|---:|---:|---|
| **1** | Persistent AI sessions: tmux + Claude Remote Control + Codex resume | низкое | низкий | **очень высокий** | делать сейчас |
| **2** | VPS coordinator + Ansible + structured receipts | среднее | низкий/средний | **очень высокий** | делать сейчас |
| **3** | Windows GPU hub as localhost API + SSH tunnels | низкое/среднее | низкий | **очень высокий** | делать сейчас |
| **4** | SQLite-aware snapshots + restic/SFTP backups | среднее | низкий | **очень высокий** | делать сейчас |
| **5** | Capability-restricted SSH identities for AI agents | среднее | снижает системный риск | **критический security payoff** | делать до расширения автономии агентов |
| **6** | macOS GUI broker + Windows interactive Scheduled Tasks | среднее | средний | высокий, но niche | после control plane |
| **7** | Human/team access via Tailscale SSH or step-ca certificates | среднее/высокое | migration risk | средний сейчас, высокий при росте команды | внедрять по мере churn |

### Почему именно такой порядок

**Persistent sessions** почти не требуют новой infrastructure: вы уже имеете SSH, Claude/Codex и tailnet. tmux устраняет зависимость lifetime агента от SSH connection, а native Claude/Codex remote functions убирают необходимость «печати в TUI через сеть». citeturn22view0turn22view2turn22view4

**Ansible/coordinator** превращает пять–восемь уникальных snowflake machines в inventory с воспроизводимым состоянием, причём Windows-over-SSH теперь официально поддерживается. citeturn22view12

**GPU hub** даёт всему fleet доступ к самому дорогому hardware без копирования inference stack на laptops; для Ollama достаточно native Windows + localhost tunnel. citeturn22view18

**Backup pipeline** закрывает опасную дыру «Syncthing = backup» и отдельно решает SQLite consistency. citeturn22view15turn22view16turn22view17

**Capability keys** должны появиться до того, как AI agents начнут автоматически переходить с одного компьютера на другой: prompt injection надо считать возможным failure mode, а blast radius ограничивать OS/SSH-level policy. citeturn22view20turn22view24

**GUI bridge** полезен, но не должен определять architecture: видимое окно — исключение, а не стандартная форма remote execution.

**Certificates/central identity** окупаются, когда credential churn становится больнее CA/policy layer; сегодня ваш существующий per-node Ed25519 design ещё вполне рационален. citeturn22view22turn22view23

### Итоговая рекомендация

[established] **Стройте SSH как capability-oriented control plane, а не как full-mesh remote shell.**

Конкретно:

```text
Hetzner coordinator
    ├── inventory + Ansible
    ├── durable SQLite job queue
    ├── structured audit receipts
    ├── health state
    └── separate SSH identities:
          ├── config
          ├── agent-dispatch
          ├── backup
          ├── GPU-tunnel
          └── GUI-trigger

Nodes
    ├── persistent terminal sessions
    ├── narrow fleet-runner
    ├── local GUI broker where necessary
    ├── backup snapshot producer
    └── local model/API services
```

Главное предположение, на котором держится эта рекомендация: **Tailscale tailnet остаётся вашим private network perimeter, большинство узлов являются personal/intermittently-online machines, а требования enterprise compliance/JIT approval пока отсутствуют.** При этом prompt injection считается реальным security failure mode, поэтому AI agent никогда не получает автоматически ту же SSH authority, что человек-администратор. citeturn22view24

При таком предположении добавлять сейчас полноценный Teleport/Salt/Nomad/Kubernetes layer принесёт меньше пользы, чем довести до строгого состояния уже имеющиеся primitives: SSH, Tailscale, Ansible, tmux, restic и узкие local brokers.

### Полный список использованных источников

**S1 — Anthropic, “Continue local sessions from any device with Remote Control”.** Дата публикации на странице не указана; доступ 23.10.2026.  
<https://code.claude.com/docs/en/remote-control> citeturn22view0

**S2 — Anthropic, “Desktop application — Claude Code Docs”.** Дата публикации на странице не указана; доступ 23.10.2026.  
<https://code.claude.com/docs/en/desktop> citeturn22view1

**S3 — OpenAI, “Codex Developer Commands”.** Дата публикации на странице не указана; доступ 23.10.2026.  
<https://learn.chatgpt.com/docs/developer-commands> citeturn22view2

**S4 — OpenAI, “Codex App Server”.** Дата публикации на странице не указана; доступ 23.10.2026.  
<https://learn.chatgpt.com/docs/app-server> citeturn22view3

**S5 — tmux project, “Getting Started”.** Дата публикации текущей wiki page не указана; доступ 23.10.2026.  
<https://github.com/tmux/tmux/wiki/Getting-Started> citeturn22view4

**S6 — Microsoft Visual Studio Code, “Remote Development using SSH”.** Дата публикации на странице не указана; доступ 23.10.2026.  
<https://code.visualstudio.com/docs/remote/ssh> citeturn22view5

**S7 — JetBrains, “Connect and work with JetBrains Gateway”.** **14.08.2026.**  
<https://www.jetbrains.com/help/idea/remote-development-a.html> citeturn22view6

**S8 — Apple, “Change Privacy & Security settings on Mac”.** Дата публикации на странице не указана; доступ 23.10.2026.  
<https://support.apple.com/en-vn/guide/mac-help/mchl211c911f/mac> citeturn22view7

**S9 — Apple, “About systemsetup in Remote Desktop”.** Дата публикации на странице не указана; доступ 23.10.2026.  
<https://support.apple.com/tr-tr/guide/remote-desktop/apd95406b8d/mac> citeturn22view8

**S10 — Microsoft, “Interactive Services”.** **07.01.2021** — источник старше предпочтительного 12–18-месячного окна, но это официальный документ об архитектурном механизме Windows.  
<https://learn.microsoft.com/en-us/windows/win32/services/interactive-services> citeturn22view9

**S11 — Microsoft, “Principal.LogonType property”.** **25.08.2021** — старше предпочтительного окна.  
<https://learn.microsoft.com/en-us/windows/win32/taskschd/principal-logontype> citeturn22view10

**S12 — Microsoft Sysinternals, “PsExec”.** **11.04.2023** — старше предпочтительного окна.  
<https://learn.microsoft.com/en-us/sysinternals/downloads/psexec> citeturn22view11

**S13 — Ansible, “Windows SSH”.** Дата публикации текущей страницы не указана; доступ 23.10.2026.  
<https://docs.ansible.com/projects/ansible/latest/os_guide/windows_ssh.html> citeturn22view12

**S14 — Model Context Protocol, “Transports”.** Specification dated **28.07.2026**.  
<https://modelcontextprotocol.io/specification/2026-07-28/basic/transports> citeturn22view13

**S15 — A2A Protocol, “Specification”.** Дата публикации текущей `latest`-страницы явно не указана; доступ 23.10.2026.  
<https://a2a-protocol.org/latest/specification/> citeturn22view14

**S16 — restic, “Preparing a new repository”.** Дата публикации текущей документации не указана; доступ 23.10.2026.  
<https://restic.readthedocs.io/en/latest/030_preparing_a_new_repo.html> citeturn22view15

**S17 — SQLite, “SQLite Backup API”.** **13.11.2025.**  
<https://www.sqlite.org/backup.html> citeturn22view16

**S18 — Syncthing, “Understanding Synchronization”.** Дата публикации страницы не указана; доступ 23.10.2026.  
<https://docs.syncthing.net/users/syncing.html> citeturn22view17

**S19 — Ollama, “Windows”.** Дата публикации страницы не указана; доступ 23.10.2026.  
<https://docs.ollama.com/windows> citeturn22view18

**S20 — vLLM, “GPU installation”, v0.12.0.** **02.12.2025.**  
<https://docs.vllm.ai/en/v0.12.0/getting_started/installation/gpu/> citeturn22view19

**S21 — OpenBSD/OpenSSH, `sshd(8)`.** Дата публикации current manual page не указана; доступ 23.10.2026.  
<https://man.openbsd.org/sshd.8> citeturn22view20

**S22 — OpenBSD/OpenSSH, `ssh(1)`.** Дата публикации current manual page не указана; доступ 23.10.2026.  
<https://man.openbsd.org/ssh.1> citeturn22view21

**S23 — Tailscale, “Tailscale SSH”.** Last validated **05.01.2026**.  
<https://tailscale.com/docs/features/tailscale-ssh> citeturn22view22

**S24 — Smallstep, “SSH certificate login advanced example: Run an SSH CA and connect to hosts using SSH certificates”.** **03.02.2026.**  
<https://smallstep.com/docs/tutorials/ssh-certificate-login/> citeturn22view23

**S25 — OpenAI, “Designing AI agents to resist prompt injection”.** **11.03.2026.**  
<https://openai.com/index/designing-agents-to-resist-prompt-injection/> citeturn22view24

## Citations (backend)
