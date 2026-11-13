---
dr_id: DR26-10-23-ZB-01-2006
title: "Что флот может достичь через SSH: синтез 5 вендоров"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Что флот может достичь через SSH (DR26-10-23-ZB-01-2006)

Заказ: Антон 23.10.2026 19:58, голосом: «Запусти ДР: что флот может достичь через SSH». Ноги собраны на Mac16 сессией, поднятой с ZBook по SSH дверью `mac_app_fire.py`. Гейт `dr_leg_gate.py --prompt`: chatgpt 55 источников, claudeai 54, gemini 67, grok 116, mistral 186. Ноги: «внутренний архив лаборатории».

Связано: fleet-ssh-mesh-2026-10-23 · mac-app-fire-remote-visible-session · insight-fleet-ssh-rail-is-transport-not-asks-2026-10-11 · insight-DR-DR26-07-06-HUB-01-ssh-access-to-hetzner-ubuntu-vps-from-claude-deskt

## Рекомендация
Первым шагом перевести всех ИИ-агентов флота на отдельные ключи с forced command (restrict,command="fleet-dispatch" + whitelist + квитанции в формате fleet_ssh.py, без agent forwarding), и только потом расширять возможности SSH (GPU-туннель, снимки SQLite, tmux + Remote Control, interactive-задача на хабе). Уровень уверенности: established, 5 из 5 вендоров, опора на man sshd(8) и задокументированный класс атак prompt injection → RCE. Главное допущение: агенты регулярно читают недоверенный текст (Telegram, почта, веб, чужие репо), а текущий mesh 20/20 даёт ключам полный shell, значит любой ключ в среде агента надо считать доступным атакующему.

## Что строить следующим
Уже есть и не повторяем: mesh 20/20, fleet_ssh.py run с квитанциями, hubrun.py, mac_app_fire.py.

1) Запертые ключи агентов: отдельная identity агента на каждом узле, в authorized_keys restrict,from="адрес узла/10",command="fleet-dispatch"; диспетчер разбирает SSH_ORIGINAL_COMMAND по whitelist (без sh -c), пишет квитанцию в формат fleet_ssh.py; человеческий ключ и ключи mesh из среды агента убрать; ForwardAgent no. Усилия 1-2 дня. Риск средний: узкий whitelist сломает текущие скрипты, катить канарейкой. Польза критическая. Поддержка 5/5.
2) Снимки SQLite + история бэкапов: sqlite3 db ".backup snap.db" или VACUUM INTO перед отправкой; restic -r sftp:backup@vps:/srv/restic/<node> через rclone serve restic --append-only; живые .db вынести из папок Syncthing; копия бэкапа не только на VPS. Усилия 1 день. Риск низкий (потеря пароля репозитория). Польза высокая. Поддержка 5/5 (Kopia вместо restic: gemini).
3) GPU-хаб как localhost API: ollama serve задачей при старте (не трей), OLLAMA_KEEP_ALIVE=-1; отдельный туннельный ключ restrict,port-forwarding,permitopen="127.0.0.1:11434",command="/bin/false"; клиенты ssh -N -L 11434:127.0.0.1:11434 hub; powercfg /change standby-timeout-ac 0; vLLM позже в WSL2 с бенчмарком двух карт. Усилия 0,5 дня. Риск низкий. Польза высокая. Поддержка 5/5.
4) Постоянные сессии агентов на Mac/Linux/VPS: ssh -t node 'tmux new -A -s ai-<proj>' + claude remote-control --name <node> --spawn worktree (доверие папки один раз через ssh -t); Codex через codex exec --json и resume по ID. Дополняет mac_app_fire.py без AppleScript. Усилия 1 день. Риск низкий (спящая машина = мёртвое окно). Польза высокая. Поддержка 5/5.
5) Видимость на Windows-хабе: одна заранее созданная задача schtasks /create /tn \Fleet\OpenClaude /it /ru <user>, hubrun.py делает только schtasks /run и schtasks /query /v; PsExec не использовать. Усилия 0,5 дня. Риск низкий-средний (нужен вход пользователя). Польза средняя. Поддержка 5/5.
6) Проба здоровья с VPS + smoke после обновлений: каждые 5 мин ssh -o BatchMode=yes -o ConnectTimeout=5 node fleet-health (forced command отдаёт JSON: версия sshd, диск, GPU, TCC, наличие задачи, ACL administrators_authorized_keys), состояния с SLEEP_EXPECTED, алерт в Telegram после 2-3 промахов, имена MagicDNS. Усилия 0,5 дня. Риск низкий. Польза средняя. Поддержка 4/5.
7) Патч-план OpenSSH: Linux и macOS до 10.4, Windows в штатном Windows Update плюс мониторинг; если когда-нибудь CA, то TrustedUserCAKeys + AuthorizedPrincipalsFile на ≥10.3. Усилия 0,5 дня. Риск средний. Польза высокая. Поддержка 2/5 (claudeai, grok).
Отложено: Ansible (5/5 упоминают, но fleet_ssh.py уже закрывает ad-hoc), Tailscale SSH и step-ca (нужны при росте команды).

## Красные флаги безопасности
1) Полный shell в mesh 20/20. Инъекция в одной сессии превращается в команды на всех 5 узлах. Митигация: отдельный ключ агента restrict,from="адрес узла/10",command="fleet-dispatch"; человеческие и админские ключи вне окружения агента.
2) Дырявый диспетчер. Обёртка с sh -c "$SSH_ORIGINAL_COMMAND" сводит forced command к нулю. Митигация: точный разбор по enum (agent.start, gpu.submit, backup.snapshot, health.read), аргументы по схеме, отказ по умолчанию, квитанция на каждый вызов.
3) Приватные ключи на Syncthing-шине. Если по шине ездят приватные ed25519, компрометация любого узла или ошибочное удаление раздаёт их всем. Митигация: по шине только публичные ключи, authorized_keys генерировать из реестра, приватные ключи и пароли restic только в secrets\ локально. Проверить, что именно сейчас лежит на шине.
4) Агент правит authorized_keys. Это постоянная закладка. Митигация: authorized_keys только для чтения агентом (на Windows administrators_authorized_keys с ACL только SYSTEM и Administrators), правка лишь генератором из реестра; в whitelist запрета на запись в ~/.ssh.
5) Agent forwarding. ssh -A даёт агенту чужие ключи на промежуточном узле (CVE-2023-38408; DisableForwarding в 7.4-9.9 не выключал X11 и агента). Митигация: ForwardAgent no, AllowAgentForwarding no, ProxyJump, OpenSSH ≥10.0.
6) GUI-мост как RCE. mac_app_fire.py и будущая задача на хабе не должны принимать произвольный AppleScript или командную строку. Митигация: LaunchAgent-брокер и schtasks только с фиксированными действиями (open-claude, focus-session); PsExec -s (SYSTEM) не использовать.
7) Утечка кредов на хосты. SSH-сессии Claude Desktop увозят inference credential и MCP_* секреты на удалённый хост. Митигация: sshHostAllowlist только на свои узлы, секреты MCP не в переменных окружения агента.
8) Открытый Ollama. Своей аутентификации нет. Митигация: bind 127.0.0.1, доступ только туннелем с ключом permitopen="127.0.0.1:11434",command="/bin/false"; GPU-раннер с allowlist моделей.
9) Агент стирает бэкапы. Взломанный агент удаляет restic-репозиторий, а удаление в Syncthing синкается везде. Митигация: rclone serve restic --append-only, отдельный ключ backup с forced command, копия вне VPS.
10) Машины команды (3 не подключены). Митигация: не подключать к mesh; на каждого отдельная учётка или ключ, письменное согласие со списком команд, только restrict,command=, отзыв одной строкой у владельца, их узлы не инициируют SSH к хабу и VPS (ACL tag:team-member).
11) Сертификаты при переходе на CA. CVE-2026-35414 обходит principals= при cert-authority в authorized_keys. Митигация: TrustedUserCAKeys + AuthorizedPrincipalsFile, OpenSSH ≥10.3, короткие сроки (ssh-keygen -s ca -V +8h).
12) Человек в цепочке. Для действий вне whitelist (sudo, запись в конфиги, доступ к хранилищам паролей) нужен подтверждающий шаг человека, а не решение классификатора.

Проверено 23.10 после синтеза: флаг 3 (приватные ключи на шине) снят. В `_deploy/sshkeys/*.json` у всех 5 узлов только поле `pubkey`; поиск `BEGIN OPENSSH PRIVATE KEY` по `_deploy` и `_transit` пуст.

## Консенсус (3+ вендора)
Консенсус (3+ вендоров из 5), по областям.

VISIBLE_SESSIONS
1) tmux держит агента живым после обрыва SSH; управление через attach, send-keys, capture-pane (5/5: chatgpt, claudeai, gemini, grok, mistral). Рецепт: ssh -t host 'tmux new -A -s agent'; ssh host 'tmux capture-pane -pt agent'. На нативной Windows tmux нет (chatgpt, claudeai). URL: https://github.com/tmux/tmux/wiki/Getting-Started, https://maude.pilotdev.fr/guides/claude-code-laptop-closed/, https://man.openbsd.org/sshd.8, https://tmux.app/doc/claude-code-tmux/, https://codeongrass.com/blog/how-to-keep-claude-code-running-after-terminal-close/, https://dev.to/gabrielanhaia/keep-claude-code-working-after-you-close-the-terminal-tmux-ssh-and-reattach-1a5g
2) Claude Code Remote Control: исполнение остаётся на узле, телефон и браузер служат окном; нужен claude.ai-логин (Pro/Max/Team/Enterprise), с API-ключом, Bedrock и чужим ANTHROPIC_BASE_URL не работает; машина должна бодрствовать (4/5: chatgpt, claudeai, grok, mistral). Рецепт: tmux + claude remote-control --name hub --spawn worktree. URL: https://code.claude.com/docs/en/remote-control, https://github.com/anthropics/claude-code/issues/29479, https://aicatchup.com/news/claude-code-remote-control-phone-start
3) Codex CLI скриптуется и возобновляется: codex exec --json, codex exec resume, codex resume --last (4/5: chatgpt, claudeai, mistral, grok через remote connections). URL: https://learn.chatgpt.com/docs/developer-commands, https://developers.openai.com/codex/cli/reference, https://computingforgeeks.com/codex-cli-cheat-sheet/, https://codex.danielvaughan.com/2026/03/26/codex-cli-cicd-non-interactive/, https://developers.openai.com/codex/cli/features, https://github.com/openai/codex/discussions/1076, https://inventivehq.com/knowledge-base/openai/how-to-resume-sessions, https://learn.chatgpt.com/docs/remote-connections
4) Windows: sshd живёт в Session 0, окно из SSH не видно. Законный мост: заранее созданная задача Планировщика с InteractiveToken (schtasks /create ... /it /ru <user>), SSH лишь делает schtasks /run; работает только при залогиненном пользователе; PsExec -i запасной и нестабильный путь (5/5). URL: https://learn.microsoft.com/en-us/windows/win32/services/interactive-services, https://learn.microsoft.com/en-us/windows/win32/taskschd/principal-logontype, https://learn.microsoft.com/en-us/sysinternals/downloads/psexec, https://github.com/PowerShell/openssh-portable/pull/875, https://learn.microsoft.com/en-us/answers/questions/27517/is-there-any-workaround-in-win10-to-allow-service, https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/schtasks-create, https://learn.microsoft.com/en-us/answers/questions/805807/unable-to-use-psexec-i-to-launch-into-interactive, https://stackoverflow.com/questions/59880794/starting-gui-programs-via-openssh-on-windows, https://learn.microsoft.com/en-us/windows/win32/taskschd/schtasks, https://learn.microsoft.com/en-us/windows-hardware/drivers/wdf/session-zero-guidelines-for-umdf-drivers, https://superuser.com/questions/1297211/applications-executed-using-ssh-dont-show-on-remote-machine, https://stackoverflow.com/questions/64809328/create-interactive-scheduled-task-with-powershell
5) macOS: вход по SSH не даёт права на GUI; TCC приписывает действия ответственному процессу (sshd-keygen-wrapper); свой sshd на 2222 это другой бинарь, гранты не наследует (5/5). URL: https://support.apple.com/en-vn/guide/mac-help/mchl211c911f/mac, https://developer.apple.com/forums/thread/806187, https://eclecticlight.co/2018/11/12/who-put-that-in-my-full-disk-access-list-ssh-and-mojaves-privacy-protection/, https://scriptingosx.com/2018/08/user-interaction-from-bash-scripts/, https://thekodelab.com/en/posts/macos-gui-terminal-vs-ssh-session/
6) VS Code Remote-SSH: стандарт удалённой разработки, окно на удалённом мониторе не появляется (4/5: chatgpt, gemini, grok, mistral). URL: https://code.visualstudio.com/docs/remote/ssh

ORCHESTRATION
7) Ansible 2.18+ официально управляет Windows по SSH: ansible_connection=ssh, ansible_shell_type=powershell, DefaultShell в реестре (5/5). URL: https://docs.ansible.com/projects/ansible/latest/os_guide/windows_ssh.html, https://docs.ansible.com/projects/ansible/latest/os_guide/intro_windows.html, https://www.automatesql.com/blog/how-to-configure-ssh-on-windows-server-2025-for-use-with-ansible
8) MCP поверх SSH = композиция: клиент запускает ssh -T host mcp-server как stdio-подпроцесс; в спеке отдельного транспорта нет; A2A оставить на HTTP и туннелировать (5/5 на stdio-склейку; chatgpt и grok про A2A). URL: https://modelcontextprotocol.io/specification/2026-07-28/basic/transports, https://modelcontextprotocol.io/specification/2025-11-25/basic/transports, https://github.com/tufantunc/ssh-mcp, https://a2a-protocol.org/latest/specification/

BACKUPS
9) Живую SQLite файлом не копировать: Backup API (.backup), VACUUM INTO или sqlite3_rsync, затем везти закрытый снимок (5/5). URL: https://www.sqlite.org/backup.html, https://www.sqlite.org/rsync.html, https://www.sqlite.org/howtocorrupt.html, https://devclass.com/2024/10/02/the-sqlite-team-is-preparing-an-efficient-remote-replication-tool/, https://litestream.io/
10) Syncthing не бэкап (удаление тоже синкается); история = restic или kopia по SFTP на отдельный репозиторий (5/5). URL: https://docs.syncthing.net/users/syncing.html, https://restic.readthedocs.io/en/latest/030_preparing_a_new_repo.html, https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html, https://kopia.io/docs/reference/command-line/common/repository-create-sftp/, https://rclone.org/sftp/

GPU_HUB
11) Ollama слушает 127.0.0.1:11434, своей аутентификации нет; ноутбуки ходят через ssh -N -L 11434:127.0.0.1:11434 hub, порт в tailnet не открывать (5/5). URL: https://docs.ollama.com/windows, https://docs.ollama.com/faq, https://4sysops.com/archives/access-remote-ollama-ai-models-through-an-ssh-tunnel/
12) vLLM на Windows только через WSL2 (4/5: chatgpt, claudeai, gemini, mistral). URL: https://docs.vllm.ai/en/v0.12.0/getting_started/installation/gpu/, https://docs.vllm.ai/en/stable/getting_started/installation/gpu/, https://docs.nvidia.com/cuda/wsl-user-guide/index.html, https://learn.microsoft.com/en-us/windows/wsl/tutorials/gpu-compute
13) На хабе отключить сон от сети (powercfg /change standby-timeout-ac 0); процесс модели не должен зависеть от консоли (4/5: chatgpt, claudeai, gemini, grok). Источников нет.

KEY_SECURITY
14) Ключ автоматизации: restrict,command="/usr/local/bin/fleet-dispatch" (+from=, permitopen для туннеля); ForwardAgent no, вместо него ProxyJump; агенту не давать ключ с полным shell (5/5). URL: https://man.openbsd.org/sshd.8, https://man.openbsd.org/ssh.1, https://www.openssh.org/security.html, https://www.simplified.guide/ssh/restrict-authorized-keys-command, https://vincent.bernat.ch/en/blog/2020-safer-ssh-agent-forwarding
15) Prompt injection у агента с SSH-ключом = RCE и боковое перемещение по флоту; границу ставит ОС и SSH, не классификатор (5/5). URL: https://openai.com/index/designing-agents-to-resist-prompt-injection/, https://www.microsoft.com/en-us/security/blog/2026/05/07/prompts-become-shells-rce-vulnerabilities-ai-agent-frameworks/, https://cymulate.com/blog/zero-click-rce-prompt-injection-ai-tools/, https://genai.owasp.org/llmrisk/llm01-prompt-injection/, https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/

RELIABILITY
16) Проба с VPS: ssh -o BatchMode=yes -o ConnectTimeout=5 host fleet-health, алерт в Telegram после 2-3 провалов, имена MagicDNS вместо IP (4/5: chatgpt, claudeai, gemini, grok). URL: https://tailscale.com/docs/features/magicdns.md, https://tailscale.com/docs/features/magicdns
17) После обновлений ОС гонять smoke: Get-Service sshd, sshd_config, TCC и osascript, порт 2222 (5/5). URL: https://learn.microsoft.com/en-us/troubleshoot/windows-server/system-management-components/error-1053-1067-7034-after-update-openssh-doesnt-start, https://mjtsai.com/blog/2021/06/01/macos-11-4-breaks-full-disk-access-for-helper-tools/

## Расхождения (по критериям вендоров)
1) Tailscale SSH. gemini: перевести на него всю межузловую аутентификацию, пункт 1 плана (критерий: централизованный отзыв, нет приватных ключей на диске). grok: только VPS и Mac с открытым tailscaled, сетку ed25519 не снимать (критерий: сервер не работает на Windows и App Store tailscaled, включение --ssh рвёт текущие сессии). chatgpt: годится для входа людей на Mac/Linux, OpenSSH на Windows не заменяет. claudeai: check mode недоступен тегированным устройствам, tsrecorder снят, Windows не проверял. mistral: не работает на Windows и обычном Mac, остаются ключи или step-ca.
2) Бэкап-движок. gemini: Kopia для волта 130k файлов (критерий: скорость параллельного скана, бенчмарк onidel) + Litestream для SQLite. chatgpt, claudeai, mistral: restic по SFTP. grok: restic или kopia одинаково. claudeai отдельно: append-only через rclone serve restic.
3) Координация и блокировки. chatgpt: SQLite-очередь на VPS (BEGIN IMMEDIATE), flock у владельца ресурса (критерий: надёжность состояния). claudeai: push-модель SSH, демон только для GUI на Windows и очереди GPU (критерий: простота при 8 машинах). grok: ssh-цикл с VPS + flock + журнал квитанций. gemini: flock на lock-файле в папке Syncthing.
4) Инструмент управления конфигом. chatgpt: ровно один Ansible. gemini: pyinfra или Ansible. grok: pyinfra легче для Python, Ansible не блокирует. mistral: Ansible, pssh, pyinfra, Salt-SSH как равные.
5) MCP поверх SSH. gemini: ssh-mcp-rs как критичная шина агентов (пункт 2). claudeai: только для пары узлов, общая шина остаётся своей. chatgpt: для редких привилегированных инструментов. grok: в спеке такого нет, MCP_* секреты уезжают на хост.
6) Окно на macOS. gemini и mistral: sudo launchctl asuser <UID> osascript поднимает окно. chatgpt: asuser = маршрутизатор сессии, обход TCC не подтверждён; строить LaunchAgent-брокер с enum-действиями. grok: людям рулить через Remote Control или Codex pairing, не AppleScript.
7) PsExec. gemini: PsExec -s -i, риск низкий. claudeai, mistral: из SSH без пароля падает или чёрное окно, брать schtasks. chatgpt, grok: только аварийный путь.
8) Сервер моделей на хабе. gemini: держать в постоянной сессии пользователя. claudeai: ollama serve задачей при старте или автологин. grok: CUDA из службы Session 0 не подтверждена, приложение из Пуска умирает при выходе пользователя.
9) Патчи OpenSSH на Windows. claudeai: ставить MSI 10.4 Preview (критерий: восемь CVE-фиксов), хотя Microsoft пишет «не для продакшена». grok: штатный путь Windows Update.

## Опровержения внутри веера
1) gemini «Tailscale SSH закрывает весь флот и убирает приватные ключи» опровергают grok и mistral: сервер только Linux и macOS с open-source tailscaled, Windows-хаб и Windows-машины команды им не закрыть. chatgpt и claudeai тоже не подтверждают Windows.
2) gemini «flock на lock-файле в общей папке Syncthing гасит гонки» противоречит chatgpt: блокировки локально у владельца ресурса, не Syncthing-файл LOCK (Syncthing замки между машинами не передаёт).
3) gemini «launchctl asuser поднимает окно, обновления сбрасывают TCC» оспаривает chatgpt: обход TCC через asuser официально не подтверждён, сброс TCC обновлением тоже не подтверждён. claudeai и grok: решает грант ответственному процессу, а не asuser.
4) gemini «PsExec -s -i, риск низкий» опровергают claudeai и mistral (падения и чёрные окна из SSH) и сам факт прав SYSTEM.
5) sqlite3_rsync: claudeai требует WAL на обеих сторонах с одинаковым page size; grok пишет, что WAL-ограничение снято в 3.50.0. Решать по версии sqlite3 на узлах.
6) WSL2 и две GPU: claudeai приводит сообщения, что в WSL2 видна одна карта; grok уточняет, что «один GPU за раз» относится к DirectML, не к CUDA.
7) Запись сессий Tailscale: chatgpt и grok перечисляют запись как возможность; claudeai пишет, что tsrecorder больше не предлагается, вместо него Tailscale PAM.
8) gemini «MCP over SSH = established-стандарт» опровергает grok: в спеке MCP только stdio и Streamable HTTP, SSH лишь носитель stdio.
9) claudeai «ставить Win32-OpenSSH 10.4 Preview» против grok (рекомендуемый путь Windows Update) и собственной оговорки claudeai, что Microsoft считает Preview непродакшеном.

## Уникальное у одного вендора
[single-vendor] claudeai: CVE-2026-35414 (OpenSSH <10.3) обход principals= при cert-authority в authorized_keys; путь TrustedUserCAKeys + AuthorizedPrincipalsFile не затронут.
[single-vendor] claudeai: claude remote-control без TTY в недоверенной папке падает «Workspace not trusted»; доверие один раз через ssh -t; несовместим с CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC (последнее также у mistral).
[single-vendor] claudeai: при двух заданиях в одной папке возобновлять Codex по ID из события thread.started, не --last.
[single-vendor] claudeai: Win32-OpenSSH issue #2474, в SSH-сессиях пропадает пользовательский PATH (HKCU); поставил MSI, значит патчи дальше сам.
[single-vendor] claudeai: rclone serve restic --append-only, взломанный узел или агент не может удалить бэкапы.
[single-vendor] chatgpt: отдельные SSH-identity под функции: config, dispatch, backup, GPU-туннель, GUI-триггер; квитанции различают человека, агента, координатора, ключ и capability.
[single-vendor] chatgpt: состояния пробы ONLINE / TAILNET_OFFLINE / SSH_PORT_DOWN / AUTH_FAILED / SLEEP_EXPECTED, ноутбук спит законно.
[single-vendor] chatgpt: GPU0 = LLM, GPU1 = ASR/эмбеддинги вместо надежды на автосложение; GPU-раннер с allowlist моделей, проверкой VRAM и локом.
[single-vendor] chatgpt: codex app-server --listen ws://127.0.0.1:4500 + ssh -N -L 4500 + codex --remote, сначала на 1-2 узлах.
[single-vendor] chatgpt: отдельные restic-репозитории и ключи для личных, командных и инфраструктурных машин; VPS не единственный держатель копии и ключей.
[single-vendor] grok: SSH-сессии Claude Desktop (3P бета): на хост уезжает inference credential, нужен sshHostAllowlist; на Linux KillUserProcesses=yes убивает движок (лечение KillExcludeUsers).
[single-vendor] grok: Codex читает конкретные алиасы ~/.ssh/config, Host * игнорирует; codex должен быть в PATH логин-шелла.
[single-vendor] grok: обёртка forced command НЕ должна делать sh -c "$SSH_ORIGINAL_COMMAND"; ForceCommand в sshd_config перекрывает ключ.
[single-vendor] grok: Ollama OLLAMA_MAX_QUEUE (перегруз = 503), OLLAMA_KEEP_ALIVE=-1; llama-server --split-mode layer|row, --tensor-split для двух карт; критерий здоровья ollama ps = 100% GPU.
[single-vendor] grok: Ansible на Windows 10/11 лишь «должен быть совместим», жёсткая поддержка только серверных SKU.
[single-vendor] mistral: обновления Windows с октября 2024 ломали запуск sshd (error 1053/1067), починено к марту 2025.
[single-vendor] mistral: claude --resume <id> "prompt" (с v2.1.285) шлёт промпт в фоновую сессию; JetBrains Gateway официально только Linux-хосты.
[single-vendor] gemini: Windows-обновления сбивают ACL administrators_authorized_keys, чинить Set-Acl (только SYSTEM и Administrators); Mac-ноут бодрить LaunchAgent с caffeinate -s -i; Set-NetAdapterPowerManagement -AllowSleepFromPowerDown Disabled.
