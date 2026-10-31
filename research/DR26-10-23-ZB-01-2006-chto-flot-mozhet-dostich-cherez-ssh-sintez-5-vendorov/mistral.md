# DR26-10-23-ZB-01-2006 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.



Show more
12:41pm
Thought for 7s

Приступаю к исследованию. План: 6 тематических блоков (видимые сессии, оркестрация, бэкапы, GPU-хаб, безопасность ключей, надёжность), по каждому — поиск первоисточников, открытие ключевых страниц, затем отчёт на русском в canvas.

Блок 1: удалённые AI-сессии и инструменты

Ran a web search

Хорошие результаты по блоку 1. Продолжаю — блоки по видимости GUI-сессий (macOS/Windows) и оркестрации.

Ran a web search

Продолжаю — оркестрация, MCP-транспорт, бэкапы.

Ran a web search

Теперь GPU-хаб и безопасность ключей.

Ran a web search

Последний блок поисков: надёжность, prompt injection для агентов с SSH-доступом, step-ca/Teleport.

Ran a web search

Теперь открываю ключевые первоисточники для верификации.

Ran a web search

Финальная проверка: поддержка платформ в VS Code Remote-SSH и ещё несколько точечных фактов.

Ran a web search

Проверю ещё один важный факт — платформы Tailscale SSH.

Searched web

Материала достаточно. Загружаю canvas-skill и пишу отчёт.

Edited a canvas
Что флот из 8 машин может достичь через SSH — deep research 2025–2026
Edit
Copy
Что частный флот из 8 машин может достичь через SSH (практика 2025–2026)
Вопрос

Какие возможности сверх «удалённого shell» реально даёт SSH небольшому флоту (2 Windows 11, 3 macOS 26, 1 Linux VPS, 3 машины коллег; всё в одном Tailscale tailnet; Syncthing + Telegram-шина; агенты Claude Code и Codex CLI на каждой машине), и каковы лучшие практики для каждой: удалённые видимые AI-сессии, оркестрация, бэкапы, разделение GPU Windows-хаба, безопасность ключей, надёжность sshd.

Краткое резюме

SSH — достаточный транспорт почти для всего флота, кроме «видимости GUI» на Windows. Оркестрация (Ansible официально поддержал Windows over SSH с версии 2.18), бэкапы (restic/kopia/rclone поверх SFTP), распределение GPU (SSH-туннели к llama.cpp/Ollama) и персистентные AI-сессии (tmux + claude --resume / codex resume) — всё это зрелые, документированные паттерны. [established]

Персистентность сессий — самая дешёвая победа: tmux-сервер переживает обрыв SSH; сессии Claude Code хранятся на диске как JSONL и возобновляются флагами --continue/--resume, Codex CLI — через codex resume. Remote Control в Claude Code (из research preview ~февраля 2026) позволяет продолжать локальную CLI-сессию с телефона/браузера и подтверждённо работает поверх SSH + tmux на headless-машине. [established / emerging]

«Видимая» сессия на macOS решается вашим текущим приёмом (AppleScript через SSH + Accessibility-грант), с оговорками TCC; на Windows SSH-сессия живёт в Session 0 и десктоп не видит — легальный путь это запланированная задача «run only when user is logged on» с последующим schtasks /run, а PsExec -i из SSH-сессии работает плохо. [established]

Главная новая угроза — не SSH, а агент с SSH-ключом: комбинация «частные данные + недоверенный текст + способность действовать наружу» (lethal trifecta) означает, что ключи для автоматизации обязаны быть ограничены forced command (restrict,command=...), а agent forwarding — выключен. [established]

Бэкапы: обычный rsync живой SQLite-базы даёт неконсистентную копию; с SQLite 3.47 (октябрь 2024) существует официальный sqlite3_rsync, делающий консистентный снапшот прямо по SSH. Syncthing оставить для синхронизации рабочих заметок, restic/kopia — для бэкапов. [established]

GPU-хаб: Ollama/llama.cpp слушают на localhost — SSH-портфорвардинг (ssh -N -L 11434:127.0.0.1:11434) делает их доступными с ноутбуков без открытия портов; WSL2 официально поддерживает CUDA, так что Linux-стек inference можно держать на Windows-хабе. [established]

Надёжность — реальная боль, задокументированная вендорами: обновления Windows от октября 2024 ломали запуск службы OpenSSH (error 1053/1067, починено с марта 2025); на macOS TCC-гранты (Full Disk Access/Accessibility) привязаны к подписи ответственного процесса и обновления могут их сбрасывать. Нужен автоматический health-check sshd после патчей. [established]

Методология

Поисковые запросы (сводно): Claude Code headless/tmux/resume; Codex CLI resume; VS Code Remote-SSH и JetBrains Gateway; macOS launchctl asuser/osascript/TCC для SSH; Windows OpenSSH Session 0/WinSta0, scheduled task interactive, PsExec -i; Ansible Windows over SSH, parallel-ssh, pyinfra, Salt-SSH; MCP over SSH/stdio; restic/kopia/rclone/rsync + SQLite; Syncthing vs rsync; Ollama/llama.cpp по SSH-туннелю, CUDA on WSL2; step-ca/Tailscale SSH/Teleport, agent forwarding, forced commands; prompt injection / lethal trifecta; отказы OpenSSH после обновлений Windows/macOS; MagicDNS; caffeinate/powercfg.

Открытые и проверенные страницы (полный текст): docs.ansible.com (Windows SSH), code.visualstudio.com (Remote-SSH), docs.anthropic.com (Claude Code: sessions, cli-reference, interactive-mode), tmux.app (Claude Code в tmux), github.com (claude-code issue #29479; tufantunc/ssh-mcp; llama.cpp server README; anthropics/claude-code), sqlite.org/rsync, restic.readthedocs.io, kopia.io, rclone.org/sftp, tailscale.com/blog/tailscale-ssh, smallstep.com (SSH CA tutorial), learn.microsoft.com (troubleshooting OpenSSH 1053/1067; Q&A про PsExec -i), simonwillison.net (lethal trifecta, 16.06.2025), vincent.bernat.ch (agent forwarding, 2020), docs.nvidia.com (CUDA on WSL), docs.saltproject.io (Salt SSH), jetbrains.com (Gateway).

Ограничения. (1) Часть страниц документации Claude Code (code.claude.com) не открылась целиком — отдельные детали процитированы по поисковым сниппетам и помечены. (2) Темы «A2A over SSH» и «SSHFS на современном macOS» подтверждены слабо — помечены [no source found] или [single-source]. (3) Практика macOS 26 (Tahoe-линейка) покрыта единичными источниками. (4) Оценки «стоит ли для 8 машин» — моя интерпретация, а не источник.

Найденное
1. Удалённые и видимые AI-сессии

Что возможно. Три слоя: (а) переживание обрыва связи (tmux/screen/zellij), (б) возобновление беседы самим инструментом (claude --continue/--resume, codex resume), (в) удалённый «пульт» (Claude Code Remote Control, VS Code Remote-SSH, JetBrains Gateway).

Зрелые инструменты и факты.

Claude Code — интерактивный TUI: при закрытии терминала процесс получает SIGHUP и умирает; nohup не подходит, tmux — да (Keep Claude Code running after SSH disconnects, [low-authority]). Официальный гайд: tmux new -A -s claude затем claude (tmux.app).

Сессии Claude Code непрерывно пишутся на диск (JSONL в «внутренний путь лаборатории»), claude --continue открывает последнюю, claude --resume <id|name> — выбранную; resume восстанавливает и permission mode (кроме bypass-permissions, который из соображений безопасности не восстанавливается) (Manage sessions — Claude Code Docs). С версии v2.1.285 claude --resume <id> с промптом на командной строке отправляет промпт в фоновую сессию (background dispatch) (Claude Code Docs: sessions).

Codex CLI: сессии пишутся в ~/.codex/sessions/; codex resume (пикер), codex resume --last, codex resume --all — история фильтруется по рабочей директории; официальный список фич упоминает resume/продолжение сессий (Codex CLI — ChatGPT Learn, Codex Resume [low-authority], GitHub-дискуссия openai/codex).

Claude Code Remote Control (research preview c ~февраля 2026, к концу сентября 2026 без метки preview): позволяет продолжать локальную сессию из claude.ai/code и мобильного приложения; требует claude.ai-логин, не работает с API-ключом и при CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC (Claude Code remote control docs, AI Catchup [low-authority]). Подтверждено работающим на headless Debian 12 VM через SSH + tmux: «Remote Control active» в статус-баре, та же сессия видна в claude.ai/code (GitHub issue #29479). Это [emerging]: фича молодая, политика Team/Enterprise может её отключать.

VS Code Remote-SSH: ставит VS Code Server на удалённую машину по SSH; официально поддерживаемые хосты — Linux x86_64/ARM, macOS 10.14+, Windows 10/Server 2016/2019 (1803+) с официальным OpenSSH Server (Remote Development using SSH, Remote Development with Linux). Известные грабли: ключ с passphrase подвешивает git pull/sync в удалённом окне; PuTTY не поддерживается; socket-режим сервера не работает на Windows-хостах (Tips and Tricks).

JetBrains Gateway: лёгкий клиент, подключается по SSH, ставит IDE-бэкенд удалённо; официально — Linux-хосты (+ WSL2-бэкенд, с IntelliJ 2025.3 WSL-проекты открываются в том же окне) (JetBrains Gateway, IntelliJ Docs).

Минимальный рецепт (персистентная сессия агента на любой машине флота).

ssh hub-mac 'tmux new -A -s agents "claude"'   # поднять/подключиться
ssh hub-mac 'tmux capture-pane -t agents -p | tail -40'  # посмотреть, что происходит
ssh hub-mac 'tmux send-keys -t agents "продолжай: прогони тесты" Enter'  # рулить

Возобновление после падения: claude --continue (в том же каталоге) или claude --resume <session-id>; для Codex — codex resume --last.

Как сделать сессию ВИДИМОЙ на десктопе той машины.

macOS. SSH-сессия живёт в Background-домене launchd, а GUI — в Aqua; osascript из «голого» SSH не достаёт до GUI-сессии — команду нужно отправлять в домен залогиненного пользователя: sudo launchctl asuser 501 osascript -e '...' (thekodelab [low-authority], Apple StackExchange). Ваш текущий приём (AppleScript + Accessibility) — рабочий, но TCC-грант в Accessibility должен получить ответственный процесс SSH-цепочки; в практике упоминается, что им оказывается sshd-keygen-wrapper (Reddit r/shortcuts [low-authority, single-source]). Гранты AppleScript/Automation можно сбрасывать и отлаживать через tccutil reset AppleEvents (Scripting OS X, 2020). Если пользователь не залогинен в GUI, launchctl asuser целиться не во что — нужен autologin (thekodelab [low-authority]).

Windows. Служебный sshd работает вне window station WinSta0: запущенные им GUI-процессы не имеют доступа к десктопу пользователя — это архитектурное свойство Session 0 isolation, а не баг (Super User, Stack Overflow). Легальные пути:

Запланированная задача с «Run only when user is logged on» (interactive token): создаётся под залогиненного пользователя и запускается по требованию schtasks /run; в Microsoft Q&A именно Task Scheduler рекомендуют для запуска интерактивного процесса по SSH, а вариант с psexec -i из SSH-сессии падает по-разному (notepad падает, cmd рисует чёрный бокс; стабильно работает только с явным -u/-p, т.е. с паролем) (Microsoft Q&A: PsExec -i over SSH). Пример регистрации задачи: SCHTASKS /Create /tn "AgentUI" /tr "..." /ru interactive /sc onlogon — флаг /ru interactive привязывает задачу к сессии залогиненного пользователя (Microsoft Q&A); в PowerShell это Register-ScheduledTask с principal LogonType Interactive (Stack Overflow).

PsExec -i — возможен, но из SSH-сессии глючен (см. выше); использовать как fallback.

Запуск sshd прямо в интерактивной сессии — возможен, но «не выглядит хорошей идеей» и ломает сценарий службы (Super User, Super User: GUI over SSH).

Linux (VPS). Проблемы нет: tmux-сессия агента видна через tmux attach откуда угодно; для «десктопа» в headless-контексте просто нет пользователя перед экраном.




Content truncated. Click "Expand" to see the complete document.

Expand
Thought for 1s

Готово — отчёт открыт в canvas «Что флот из 8 машин может достичь через SSH — deep research 2025–2026».

Главный вывод: SSH покрывает почти всё, что нужно флоту, кроме «видимости GUI» на Windows — и три самые дешёвые победы это: (1) tmux + claude --resume / codex resume для персистентных сессий агентов, (2) restic поверх SFTP + новый официальный sqlite3_rsync (SQLite 3.47) для консистентных бэкапов живых БД, и (3) restrict,command= на всех ключах автоматизации — потому что агент с unrestricted-ключом на 7 машин это готовая prompt-injection траектория (lethal trifecta).

Другие ключевые находки:

На Windows SSH-сессия архитектурно живёт в Session 0 и не видит десктоп; легальный путь — scheduled task «run only when user is logged on» + schtasks /run (PsExec -i из SSH нестабилен).
Claude Code Remote Control подтверждённо работает поверх SSH + tmux на headless-машине — частично заменяет ваш AppleScript-приём.
Обновления Windows (окт 2024 – март 2025) реально ломали службу OpenSSH — нужен автоматический health-check после патчей.
Tailscale SSH не работает как сервер на Windows/обычном Mac — только Linux и open-source tailscaled, так что пер-ноудные ключи или step-ca остаются вашими вариантами.

В отчёте: 6 тематических разделов с рецептами и режимами отказа, таблица ~38 источников с оценками достоверности, теги [single-source]/[low-authority], уровни уверенности по каждому ключевому выводу и ранжированный план из 7 следующих шагов.

12:41pm
Knowledge automatically saves key insights from your chats
Set up now




Think

Vibe can make mistakes. Check answers. Learn more

## Sources (links)
- https://codeongrass.com/blog/how-to-keep-claude-code-running-after-terminal-close/
- https://dev.to/gabrielanhaia/keep-claude-code-working-after-you-close-the-terminal-tmux-ssh-and-reattach-1a5g
- https://tmux.app/doc/claude-code-tmux/
- https://lumadock.com/tutorials/run-claude-code-on-vps
- https://ovidiueftimie.substack.com/p/the-command-line-in-your-pocket-remote
- https://github.com/anthropics/claude-code/issues/29479
- https://code.claude.com/docs/en/sessions
- https://code.claude.com/docs/en/agent-sdk/sessions
- https://www.ssdnodes.com/learn/lang/pcm/claude-code-resume-and-session-history
- https://www.ssdnodes.com/learn/claude-code-resume-and-session-history
- https://lumadock.com/tutorials/claude-code-sessions
- https://platform.claude.com/docs/en/agent-sdk/sessions
- https://code.visualstudio.com/docs/remote/ssh
- https://code.visualstudio.com/docs/remote/ssh-tutorial
- https://edvaldoguimaraes.com.br/2024/10/28/remote-development-over-ssh-with-vs-code-a-comprehensive-guide/
- https://code.visualstudio.com/docs/remote/troubleshooting
- https://code.visualstudio.com/docs/remote/remote-overview
- https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh
- https://www.jetbrains.com/remote-development/gateway/
- https://www.jetbrains.com/help/idea/remote-development-a.html
- https://www.jetbrains.com/help/idea/remote-development-overview.html
- https://www.jetbrains.com/help/pycharm/remote-development-a.html
- https://www.jetbrains.com/help/idea/remote-development-starting-page.html
- https://coder.com/docs/user-guides/workspace-access/jetbrains/gateway
- https://inventivehq.com/knowledge-base/openai/how-to-resume-sessions
- https://github.com/openai/codex/discussions/1076
- https://www.verdent.ai/guides/codex-cli-resume-continue-save-chat
- https://developers.openai.com/codex/cli/features
- https://www.codeagentswarm.com/en/guides/codex-cli-conversation-history
- https://www.reddit.com/r/ChatGPTPro/comments/1na1wrk/i_built_a_small_tool_to_add_resume_session/
- https://apple.stackexchange.com/questions/100829/how-to-launch-a-gui-application-in-another-users-graphical-session
- https://thekodelab.com/en/posts/macos-gui-terminal-vs-ssh-session/
- https://github.com/webcoyote/sandvault/issues/113
- https://aahlenst.dev/blog/accessing-the-macos-gui-in-automation-contexts/
- https://ss64.com/mac/launchctl.html
- https://stackoverflow.com/questions/32907909/allow-applescript-script-to-run-without-asking-for-permission
- https://stackoverflow.com/questions/69548089/privacy-tab-using-applescript
- https://www.reddit.com/r/shortcuts/comments/nja7ni/osascript_is_not_allowed_assistive_access/
- https://scriptingosx.com/2020/09/avoiding-applescript-security-and-privacy-requests/
- https://hacktricks.wiki/en/macos-hardening/macos-security-and-privilege-escalation/macos-security-protections/macos-tcc/macos-tcc-bypasses/macos-apple-scripts.html
- https://superuser.com/questions/1297211/applications-executed-using-ssh-dont-show-on-remote-machine
- https://superuser.com/questions/1532575/how-to-run-windows-openssh-server-interactively
- https://stackoverflow.com/questions/59880794/starting-gui-programs-via-openssh-on-windows
- https://github.com/elixir-crab/gpui/issues/3
- https://github.com/PowerShell/Win32-OpenSSH/issues/690
- https://www.reddit.com/r/PowerShell/comments/qc469s/create_a_scheduled_task_to_run_as_logged_on_user/
- https://stackoverflow.com/questions/13965997/powershell-set-a-scheduled-task-to-run-when-user-isnt-logged-in
- https://forums.powershell.org/t/create-scheduled-task-run-as-logged-on-user/6442
- https://learn.microsoft.com/en-us/answers/questions/259563/run-scheduled-task-as-system-only-when-user-is-log
- https://stackoverflow.com/questions/64809328/create-interactive-scheduled-task-with-powershell
- https://learn.microsoft.com/en-us/answers/questions/805807/unable-to-use-psexec-i-to-launch-into-interactive
- https://www.reddit.com/r/PowerShell/comments/182fgn4/executing_remote_ps_to_run_inside_local_session/
- https://stackoverflow.com/questions/9096757/psexec-to-remotely-run-a-gui-application
- https://stackoverflow.com/questions/71789829/issues-using-psexec-i-to-launch-interactive-app
- https://theitbros.com/using-psexec-to-run-commands-remotely/
- https://docs.ansible.com/projects/ansible/latest/os_guide/windows_ssh.html
- https://docs.ansible.com/projects/ansible/latest/os_guide/intro_windows.html
- https://gist.github.com/letajmal/0ac50ead52a4e80d96b52ef22c391666
- https://gist.github.com/gpduck/b073835ba33e37803edb5a8df484ba81
- https://docs.ansible.com/projects/ansible-core/devel/os_guide/intro_windows.html
- https://parallel-ssh.readthedocs.io/en/latest/alternatives.html
- https://gist.github.com/gnat/7cb6f4734eb53a416759fd7728b633af
- https://pypi.org/project/parallel-ssh/
- https://serverfault.com/questions/17931/what-is-a-good-modern-parallel-ssh-tool
- https://www.saashub.com/pssh-alternatives
- https://github.com/tufantunc/ssh-mcp
- https://skywork.ai/skypage/en/The-Ultimate-Guide-to-MCP-Server-SSH-Empowering-AI-with-Secure-Remote-Infrastructure-Access/1972852349963268096
- https://lobehub.com/mcp/tufantunc-ssh-mcp
- https://mcp.pub/servers/ssh-remote-mcp/
- https://certified-architect.com/cca-f-study-guide/foundations/f3-5-mcp-server-transport-methods
- https://restic.readthedocs.io/en/latest/030_preparing_a_new_repo.html
- https://restic.readthedocs.io/en/v0.13.1/030_preparing_a_new_repo.html
- https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html
- https://restic.readthedocs.io/en/v0.8.1/030_preparing_a_new_repo.html
- https://restic.readthedocs.io/en/v0.3.3/Manual/
- https://kopia.io/docs/reference/command-line/common/repository-connect-sftp/
- https://kopia.io/docs/repositories/
- https://kopia.io/docs/reference/command-line/common/repository-create-sftp/
- https://kopia.io/docs/reference/command-line/common/repository-sync-to-sftp/
- https://kopia.io/docs/reference/command-line/advanced/repository-repair-sftp/
- https://serverfault.com/questions/89329/rsync-sqlite-database
- https://akmatori.com/blog/sqlite3_rsync_guide
- https://www.sqlite.org/rsync.html
- https://lobste.rs/s/xopqmh/sqlite_rsync_database_remote_copy_tool
- https://devclass.com/2024/10/02/the-sqlite-team-is-preparing-an-efficient-remote-replication-tool/
- https://rosettadigital.com/syncthing-vs-rsync/
- https://www.reddit.com/r/Syncthing/comments/1n9sajy/is_syncthing_supposed_to_be_faster_than_rsync/
- https://www.reddit.com/r/selfhosted/comments/1fsxjz5/need_a_good_software_recommendation_for_file/
- https://forum.syncthing.net/t/syncthing-vs-rsync-vs-unison/6298
- https://sourceforge.net/software/compare/Syncthing-vs-rsync/
- https://4sysops.com/archives/access-remote-ollama-ai-models-through-an-ssh-tunnel/
- https://note.com/hokuto_tech/n/n183d09bc665a?hl=en
- https://pinggy.io/blog/forward_ollama_port_11434_online_access/
- https://medium.com/@nisat19/how-to-use-a-self-hosted-ollama-model-on-a-remote-vm-from-your-local-pc-58010591dca7
- https://dev.to/lightningdev123/access-your-local-ollama-models-from-anywhere-port-11434-made-simple-2a79
- https://www.runpod.io/articles/guides/deploy-llama-cpp-cloud-gpu-hosting-headaches
- https://wiki.seeedstudio.com/ai_robotics_distributed_llama_cpp_rpc_jetson/
- https://github.com/ggml-org/llama.cpp/blob/master/tools/server/README.md
- https://www.spheron.network/blog/deploy-llama-cpp-server-gpu-cloud/
- https://github.com/gpustack/llama-box
- https://docs.nvidia.com/cuda/wsl-user-guide/index.html
- https://learn.microsoft.com/en-us/windows/ai/directml/gpu-cuda-in-wsl
- https://developer.nvidia.com/cuda/wsl
- https://docs.nvidia.com/cuda/archive/13.1.2/wsl-user-guide/index.html
- https://www.freecodecamp.org/news/how-to-set-up-cuda-and-wsl2-for-windows-11-including-pytorch-and-tensorflow-gpu/
- https://tailscale.com/docs/reference/ssh-over-tailscale
- https://tailscale.com/blog/tailscale-ssh
- https://www.reddit.com/r/VPS/comments/1v5h04y/tailscale_vs_ssh/
- https://github.com/tailscale/tailscale/issues/8732
- https://tech.stonecharioteer.com/posts/2026/tailscale-ssh/
- https://security.stackexchange.com/questions/101783/are-there-any-risks-associated-with-ssh-agent-forwarding
- https://www.reddit.com/r/sysadmin/comments/xls6et/am_i_the_only_one_whos_nervous_when_sshagent/
- https://vincent.bernat.ch/en/blog/2020-safer-ssh-agent-forwarding
- https://termique.app/blog/ssh-agent-forwarding-security-risks-explained/
- https://nhimg.org/faq/why-does-ssh-agent-forwarding-increase-the-risk-of-credential-misuse/
- https://www.simplified.guide/ssh/restrict-authorized-keys-command
- https://www.thomas-krenn.com/en/wiki/Restrict_executable_SSH-commands_via_authorized_keys
- https://stackoverflow.com/questions/402615/how-to-restrict-ssh-users-to-a-predefined-set-of-commands-after-login
- https://serverfault.com/questions/749474/ssh-authorized-keys-command-option-multiple-commands
- https://www.n0tes.fr/2023/11/18/Restrict-User-to-SSH-Forced-Command/
- https://danielcosenza.com/posts/mac-fix-permission-denied-update/
- https://github.com/tim-codes/dotfiles/issues/137
- https://github.com/stablyai/orca/issues/29476
- https://developer.apple.com/forums/thread/679979
- https://mjtsai.com/blog/2021/06/01/macos-11-4-breaks-full-disk-access-for-helper-tools/
- https://learn.microsoft.com/en-us/troubleshoot/windows-server/system-management-components/error-1053-1067-7034-after-update-openssh-doesnt-start
- https://serverfault.com/questions/1166382/openssh-ssh-server-wont-start-on-windows-server-2019-after-windows-updates
- https://borncity.com/win/2024/10/17/windows-october-2024-updates-cause-openssh-issues/
- https://github.com/PowerShell/Win32-OpenSSH/issues/2299
- https://jpwinsup.github.io/blog/2024/11/12/OpenSSH/OpenSSH_update_oct_2024/
- https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/
- https://www.mindstudio.ai/blog/lethal-trifecta-prompt-injection
- https://simonw.substack.com/p/the-lethal-trifecta-for-ai-agents
- https://medium.com/@itpro677/the-lethal-trifecta-how-indirect-prompt-injection-is-breaking-agentic-ai-and-what-security-teams-c2ecba874ed1
- https://dev.to/max_quimby/your-agent-has-a-lethal-trifecta-nobody-shipped-a-fix-for-5233
- https://smallstep.com/docs/tutorials/ssh-certificate-login/
- https://github.com/smallstep/certificates
- https://smallstep.com/docs/step-ca/
- https://smallstep.com/docs/ssh/how-it-works/
- https://smallstep.com/docs/step-cli/reference/ssh/certificate/
- https://en.wikipedia.org/wiki/Teleport_(software)
- https://en.wikipedia.org/wiki/OpenSSH
- https://en.wikipedia.org/wiki/Comparison_of_SSH_servers
- https://dev.to/piyushjajoo/how-teleport-works-a-deep-dive-into-modern-infrastructure-access-14m5
- https://www.reddit.com/r/sysadmin/comments/1nkqorq/teleport_for_ssh/
- https://www.reddit.com/r/Tailscale/comments/1j343as/using_service_name_instead_of_hostname_with/
- https://tailscale.com/docs/reference/dns-in-tailscale
- https://tailscale.com/docs/concepts/machine-names
- https://tailscale.com/blog/magicdns-why-name
- https://tailscale.com/docs/features/magicdns
- https://docs.anthropic.com/en/docs/claude-code/
- https://code.claude.com/docs/en/claude-code/
- https://docs.anthropic.com/en/docs/claude-code/cli-reference
- https://docs.anthropic.com/en/docs/claude-code/interactive-mode
- https://code.claude.com/docs/en/interactive-mode
- https://tenvoai.com/blog/remote-desktop-power-management
- https://slack.green/en/blog/keep-computer-awake-powershell
- https://blog.comodo.com/pc-security/how-to-stop-computer-from-sleeping/
- https://www.itarian.com/blog/how-to-stop-computer-from-sleeping/
- https://unix.stackexchange.com/questions/1786/os-x-how-to-keep-the-computer-from-sleeping-during-a-ssh-session
- https://github.com/nkunievsky/ssh-caffeinate
- https://apple.stackexchange.com/questions/71524/can-i-prevent-a-remote-mac-from-entering-sleep-via-a-remote-ssh-connection
- https://lidrun.com/blog/prevent-mac-sleep-during-terminal-command
- https://code.visualstudio.com/docs/remote/linux
- https://deepwiki.com/microsoft/vscode-docs/2.3-remote-ssh-development
- https://docs.pyinfra.com/en/2.x/connectors/ssh.html
- https://github.com/pyinfra-dev/pyinfra
- https://docs.pyinfra.com/en/3.x/getting-started.html
- https://docs.pyinfra.com/en/2.x/operations/ssh.html
- https://fabianlee.org/2018/07/01/saltstack-salt-ssh-for-agentless-automation-on-ubuntu/
- https://medium.com/devsecops-community/salt-ssh-agentless-monitoring-no-minions-no-hassle-just-pure-ssh-magic-7416a83587a5
- https://docs.saltproject.io/salt/user-guide/en/latest/topics/salt-ssh.html
- https://study-ccnp.com/saltstack-salt-ssh-server-only-mode/
- https://code.claude.com/docs/en/remote-control
- https://claudefa.st/blog/guide/development/remote-control-guide
- https://aicatchup.com/news/claude-code-remote-control-phone-start
- https://www.reddit.com/r/ClaudeCode/comments/1rdr7ga/claude_code_just_got_remote_control/
- https://docs.anthropic.com/en/docs/claude-code/tutorials#resume
- https://docs.anthropic.com/en/docs/claude-code/sessions
- https://rclone.org/sftp/
- https://tailscale.com/docs/features/tailscale-ssh
- https://tailscale.com/docs/reference/tailscale-cli
- https://tailscale.com/blog/tailscale-web-ssh-console
- https://harshalranjhani.in/blog/ssh-home-server-with-tailscale
- https://tailscale.com/docs/install/linux
- https://learn.microsoft.com/en-us/answers/questions/805807/