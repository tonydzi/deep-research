# DR26-10-23-ZB-01-2006 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Собираю первичные источники по шести областям: видимые сессии, оркестрация, бэкапы, GPU, ключи и живучесть sshd.Открываю официальные страницы и добираю пробелы: GUI на Windows, оркестрация, бэкапы, сертификаты и живучесть sshd.A. Журнал поисков и открытых страниц

Поиски этого прогона: Claude Code remote control SSH; Codex CLI remote SSH; VS Code Remote SSH; Windows OpenSSH session isolation; Tailscale SSH; OpenSSH authorized_keys command=; restic SFTP; Ollama remote; Ansible Windows OpenSSH; OpenBSD sshd/sshd_config; SQLite backup API; macOS TCC sshd AppleScript; JetBrains Gateway; Smallstep SSH certificates; OpenSSH agent forwarding и security advisories; Microsoft Session 0; MagicDNS; MCP stdio; pyinfra SSH; borg SSH; kopia SFTP; A2A; llama.cpp server; schtasks /IT; Syncthing sync.

Страницы, текст которых прочитан в этом прогоне: https://code.claude.com/docs/en/remote-control ; https://claude.com/docs/third-party/claude-desktop/ssh-remote-sessions ; https://learn.chatgpt.com/docs/remote-connections ; https://tailscale.com/docs/features/tailscale-ssh ; https://tailscale.com/docs/features/magicdns.md ; https://docs.ansible.com/projects/ansible/latest/os_guide/windows_ssh.html ; https://man.openbsd.org/sshd.8 ; https://man.openbsd.org/?query=sshd_config&apropos=0&sec=5&arch=default&manpath=OpenBSD-7.8 ; https://www.sqlite.org/backup.html ; https://www.sqlite.org/rsync.html ; https://www.openssh.org/security.html ; https://docs.ollama.com/faq ; https://learn.microsoft.com/en-us/sysinternals/downloads/psexec ; https://learn.microsoft.com/en-us/windows-hardware/drivers/wdf/session-zero-guidelines-for-umdf-drivers ; https://learn.microsoft.com/en-us/archive/blogs/hmahrt/communication-between-windows-services-and-applications-in-windows-vista-and-higher ; https://code.visualstudio.com/docs/remote/ssh ; https://learn.microsoft.com/en-us/windows/wsl/tutorials/gpu-compute ; https://genai.owasp.org/llmrisk/llm01-prompt-injection/ ; https://modelcontextprotocol.io/specification/2025-11-25/basic/transports ; https://borgbackup.readthedocs.io/en/master/deployment/hosting-repositories.html ; https://docs.pyinfra.com/connectors/ssh.html ; https://kopia.io/docs/reference/command-line/common/repository-connect-sftp/ ; https://docs.syncthing.net/users/syncing.html ; https://github.com/ggml-org/llama.cpp/blob/master/tools/server/README.md ; https://a2a-protocol.org/llms.txt ; https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html (выдача поиска, дата страницы 2026-07-05); https://learn.microsoft.com/en-us/windows/win32/taskschd/schtasks (выдача поиска, 2026-08-26); https://smallstep.com/docs/tutorials/ssh-certificate-login/ (выдача поиска, 2026-02-03). Прямое открытие https://developer.apple.com/forums/thread/806187 упёрлось в проверку браузера. Текст ниже про macOS 26 и sshd-keygen-wrapper взят из выдачи поиска по этому URL и помечен [single-source].

Не открыты в этом прогоне, поэтому рецептов нет: Salt-SSH, Fabric, parallel-ssh, Teleport, rclone, vLLM, whisper.cpp, zellij, потребительская (не 3P) страница SSH-сессий Claude Desktop. Где ниже назван инструмент без страницы, стоит [no source found].

B. Что SSH здесь реально даёт

Для флота из 8 личных машин SSH уже является транспортом команды, файла и туннеля. Зрелые вещи 2025-2026: постоянный CLI агента на машине, которая не спит; UI на одной машине и движок на другой (Codex SSH, Claude Desktop 3P SSH, VS Code Remote-SSH, JetBrains Gateway); HTTP к GPU хаба через туннель на localhost; снимок SQLite и шифрованный репозиторий по SFTP; ключ автоматизации с принудительной командой. Не зрелая замена рабочему столу: «SSH-сессия видит экран Windows». Сервис sshd живёт в Session 0, пользовательский рабочий стол в Session 1 и дальше. [established] https://learn.microsoft.com/en-us/windows-hardware/drivers/wdf/session-zero-guidelines-for-umdf-drivers (страница обновлена 2026-08-26, правило с Vista) и https://learn.microsoft.com/en-us/archive/blogs/hmahrt/communication-between-windows-services-and-applications-in-windows-vista-and-higher (архив, обновлён 2026-08-05, текст про Vista и новее).

1. Удалённые и видимые сессии агентов

Что возможно. Три разных механизма, их нельзя смешивать.

Remote Control у Claude Code. Сессия и файлы остаются на той машине, где запущен `claude`. Телефон, браузер claude.ai/code и второй компьютер смотрят в это окно. Это не SSH. Компьютер должен быть включён, процесс `claude` должен жить. После сна или обрыва сеть поднимается сама, когда машина снова онлайн. [established] https://code.claude.com/docs/en/remote-control (прочитано 2026-10-23). Подписка Pro, Max, Team, Enterprise. Ключ API не подходит. На Team и Enterprise владелец включает тумблер. Не работает с Bedrock, Google Agent Platform, Microsoft Foundry и с чужим `ANTHROPIC_BASE_URL`. Старт: `claude remote-control` (сервер), `claude --remote-control` (интерактив плюс пульт), `/remote-control` из уже идущей сессии. `--continue` и `--session-id` с v2.1.200. `--spawn worktree` даёт отдельный git worktree на сессию. Ёмкость по умолчанию 32. Флаги `--sandbox` / `--no-sandbox` сняты в v2.1.300, сервер с ними не стартует. Пока каталог не доверен, без терминала на stdin команда выходит с ошибкой workspace not trusted.

SSH-сессии Claude Desktop на 3P. Интерфейс на устройстве человека, движок на SSH-хосте. Бета, нужен Claude Desktop 1.40609.0 или новее и ключ `sshHostAllowlist`, потому что на хост уезжает inference credential. [established] https://claude.com/docs/third-party/claude-desktop/ssh-remote-sessions (прочитано 2026-10-23). Ходы: клиент OpenSSH на macOS и Linux, встроенный клиент на Windows; на хост кладётся сервер и движок в «внутренний путь лаборатории»; каждый read, edit, shell и git идёт на хосте от имени SSH-пользователя, sudo не нужен. Движок переживает обрыв SSH, сон устройства и выход из Desktop и доделывает текущий ход либо встаёт на запросе разрешения. На Linux, где `KillUserProcesses=yes`, процессы SSH-пользователя умирают вместе с сессией. Лечение из той же страницы: `KillExcludeUsers`. После выхода из приложения Windows-клиент движок не подхватывает. На Linux и macOS клиент 2.42304.0 и новее пытается подхватить движок той же версии. Простой движок на Linux/macOS гасится через 30 дней без соединения (Desktop 1.52386.0+). Буфер отложенного вывода 16 МиБ. Хуки `Notification` / `Stop` / `StopFailure` в «внутренний путь лаборатории» на хосте могут пинговать человека, пока Desktop закрыт. Потребительский Desktop вне 3P в этом прогоне отдельной официальной страницей не подтверждён. Вторичные обзоры 2026-07-15 и 2026-10-14 говорят, что у десктопного приложения есть вход по SSH. [low-authority]

Codex. Пульт «Control this Mac or PC» в ChatGPT desktop спаривает телефон с бодрствующим Mac или Windows. Это не SSH. Сон, обрыв сети или закрытое приложение останавливают доступ. На Mac с закрытой крышкой нужны питание и внешний дисплей. Выбор Sleep доступ останавливает. На Windows Computer Use идёт на переднем плане, сессия должна быть разблокирована. [established] https://learn.chatgpt.com/docs/remote-connections (прочитано 2026-10-23). Отдельный путь: проект на SSH-хосте. Приложение читает конкретные алиасы из `~/.ssh/config` (выдача поиска по тому же документу: шаблон `Host *` игнорируется), стартует удалённый Codex app server через логин-шелл SSH-пользователя. `codex` должен быть в PATH этого шелла. Команды, чтение и запись идут на хосте. Телефон при этом всё равно говорит с desktop-хостом, а тот уже с SSH. [established] тот же URL, раздел Connect to an SSH host.

tmux. Для любого CLI (claude, codex) на Linux и macOS рабочая схема: `ssh -t user@host 'tmux new -s agent -A'` и внутри CLI. Обрыв SSH не убивает tmux, если процесс не в зоне KillUserProcesses. Официальный man tmux в этом прогоне не открыт. [no source found] на конкретные флаги tmux. Практика «процесс в мультиплексоре переживает обрыв клиента» согласуется с тем, как Claude описывает выживание движка и как sshd выполняет команду через логин-шелл. [established] для модели сессии sshd: https://man.openbsd.org/sshd.8 (OpenBSD-current, дата в подвале 2 сентября 2026).

VS Code Remote-SSH. На хост ставится VS Code Server, локальный VS Code показывает UI. Хосты: Linux glibc, Windows 10 / Server 2016/2019 (1803+) с официальным OpenSSH, macOS 10.14+ с Remote Login. Рекомендация: от 2 ГБ RAM. Ключи, не пароль. Проброс портов: `LocalForward` в ssh config или команда Forward a Port. Страница одобрена 10/21/2026. [established] https://code.visualstudio.com/docs/remote/ssh . С 1.99 (март 2025) сервер требует glibc 2.28+, то есть Debian 10, Ubuntu 20.04, RHEL 8. [established] https://code.visualstudio.com/docs/remote/faq (прочитано через поиск). Агентные сессии VS Code по SSH описаны на https://code.visualstudio.com/docs/agents/remote-agent-sessions (поиск): на удалённой машине отдельный агент не ставится, строка `user@hostname`, папка на хосте. С версии 1.139 ещё и dev container по SSH, если на хосте Docker.

JetBrains Gateway. Отдельное приложение или вход из IDE. SSH, на сервере нужен subsystem sftp, Gateway сам ставит IDE backend. Дата правки страницы Rider: 14 августа 2026. [established] https://www.jetbrains.com/help/rider/Remote_development-a.html и обзор https://www.jetbrains.com/remote-development/gateway/ . Серверу нужен выход на download.jetbrains.com и соседние хосты (выдача поиска по справке IDEA). Для флота из 8 это тяжелее, чем tmux плюс CLI, и имеет смысл только тем, кто уже живёт в IDE JetBrains.

Видимость человеку на рабочем столе.

macOS. `osascript` из SSH может нажать меню, только если ответственный процесс имеет Accessibility, а для части диска ещё и Full Disk Access. Системный Remote Login ходит через `/usr/libexec/sshd-keygen-wrapper`. На macOS 26.1 (сборка 25B78) выдача форума Apple описывает, что Full Disk Access этому бинарю не выдаётся обычным кликом. Обход из того же треда: перетащить бинарь в список. DTS Apple пишет, что похожая поломка Accessibility в 26.1 и 26.2 Beta починена в 26.3 Beta. [single-source] https://developer.apple.com/forums/thread/806187 (ноябрь 2025, прямое открытие не удалось). Пользовательский sshd на 2222 под launchd это другой бинарь, грант TCC на системный wrapper его не покрывает. sshd без root обязан получить ключ хоста флагом `-h`. [established] https://man.openbsd.org/sshd.8 . Рецепт «показать человеку»: CLI в tmux в его Terminal, либо официальный Remote Control / Codex pairing. AppleScript по SSH оставить узким обходом и ждать, что очередное обновление macOS снимет грант.

Windows. Голый `ssh user@win command` выполняется не в интерактивном сеансе рабочего стола. Два легальных моста, оба требуют, чтобы человек уже был залогинен.

Планировщик. `schtasks /IT` запускает задачу интерактивно только если пользователь `/RU` залогинен в этот момент. [established] https://learn.microsoft.com/en-us/windows/win32/taskschd/schtasks (выдача, 2026-08-26). Старый документ Task Security Context говорит то же словами интерфейса: «Run only when user is logged on» даёт интерактив, «Run whether user is logged on or not» интерактива не даёт. Документ помечен Vista / Server 2008, страница обновлена 2024-11-01. [single-source] на формулировку радиокнопки, модель совпадает с живым флагом `/IT`. https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-vista/cc722152(v=ws.10)

PsExec. `-i [session]` сажает процесс на рабочий стол указанной сессии. Без номера берётся консольная сессия. Флаг обязателен для интерактивной консоли. Документ v2.43, опубликован 11 апреля 2023, страница обновлена 30 марта 2023. Старше 18 месяцев, механизм тот же. [established] https://learn.microsoft.com/en-us/sysinternals/downloads/psexec . Антивирусы помечают PsTools как remote admin. Пароль в команде PsExec по сети шифруется, но из SSH-командной строки он виден в истории. На своей машине номер сессии берётся у `query user`, затем `psexec -accepteula -i N -d ...`. Сессия после перелогина меняет номер.

Linux. Интерактив: `ssh -t`. Графику процесса пользователя даёт его сессия (DBus, `XDG_RUNTIME_DIR`), не X11-форвардинг. X11 forwarding по умолчанию в sshd выключен. [established] https://www.openssh.org/security.html (заметка 9 марта 2016, актуальность дефолта подтверждена заметкой 9 апреля 2025).

Стоит ли. Для этого флота да, но в таком порядке: tmux на VPS и на Mac, которые не спят; Remote Control или Codex pairing, когда человеку нужно рулить с другого экрана; VS Code Remote-SSH, когда правит человек в редакторе; AppleScript и PsExec только чтобы вытащить окно на уже открытый рабочий стол. Поднимать Claude Desktop на Windows через SSH как основной пульт не стоит.

2. Оркестрация

Ansible по SSH на Windows поддержан официально с Ansible 2.18. Раньше плагин работал с 2.8, но поддержки не было. [established] https://docs.ansible.com/projects/ansible/latest/os_guide/windows_ssh.html . Поддерживается только OpenSSH из состава Windows, не пакет Win32-OpenSSH. Пол версии 7.9.0.0. В тексте Ansible это «фактически с Windows Server 2022», потому что Server 2019 несёт 7.7.2.1. Соседняя страница говорит, что тесты идут на серверных SKU, а Windows 10 и 11 «должны быть совместимы». [established] https://docs.ansible.com/projects/ansible/latest/os_guide/intro_windows.html (поиск, 2025-11-27). Расхождение источников: пол версии жёсткий, пригодность десктопного Windows 11 названа совместимостью, не сертификацией. Рецепт: `ansible_connection=ssh`, `ansible_shell_type=powershell`. Дефолтный шелл Windows sshd это cmd.exe. PowerShell задаётся так:

```
New-ItemProperty -Path HKLM:\SOFTWARE\OpenSSH -Name DefaultShell -Value C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -PropertyType String -Force
```

Перезапуск sshd для смены шелла не нужен, нужно `meta: reset_connection`. Ключи администраторов лежат не в профиле, а в `C:\ProgramData\ssh\administrators_authorized_keys`, пока в `sshd_config` жив блок `Match Group administrators`. Ключ не делегирует учётные данные на сетевые ресурсы (double-hop). Пароль делегирует, и Ansible его не рекомендует.

pyinfra по умолчанию ходит по SSH, файлы через SFTP, ретраи соединения есть. [established] https://docs.pyinfra.com/connectors/ssh.html (2026-06-15). Для восьми машин на Python это легче Ansible, модулей под Windows меньше. Ansible выигрывает, когда нужны идемпотентные модули и уже есть Windows.

parallel-ssh, Fabric, Salt-SSH в этом прогоне не открывались. [no source found] на их флаги. Для восьми узлов отдельный демон-оркестратор слабее, чем SSH: демон надо ставить, обновлять и отзывать на личных машинах. SSH выигрывает, пока команда редкая, узел и так в tailnet, а квитанция пишется на вызывающей стороне. Демон или очередь выигрывают, когда задание должно пережить обрыв вызывающего и дождаться, пока ноутбук проснётся. Очередь на всегда включённом VPS плюс `ssh` с VPS на узел это и есть эта граница, без Salt.

Идемпотентность и замок. Ansible-модуль сам. Для своих скриптов: ключ идемпотентности в команде, `flock` на хосте, квитанция (кто, кому, команда, код выхода, время) в журнал на VPS. Аудит OpenSSH: без `-q` sshd пишет начало, аутентификацию и конец сессии. [established] https://man.openbsd.org/sshd.8 . Текст команды в этот журнал сам не попадает. Его пишет обёртка forced command из `SSH_ORIGINAL_COMMAND` (раздел 5).

MCP. Спецификация 2025-11-25 задаёт два транспорта: stdio и Streamable HTTP. stdio: клиент запускает сервер подпроцессом, сообщения это строки JSON-RPC. [established] https://modelcontextprotocol.io/specification/2025-11-25/basic/transports . Отдельного «MCP over SSH» в спецификации нет. Рабочая склейка: клиент запускает `ssh host mcp-server` как свой stdio-подпроцесс. Секрет в аргументе или в переменной `MCP_*` при SSH-сессии Claude уезжает на хост. [established] страница Claude Desktop SSH, раздел Where MCP servers run.

A2A. Открытый протокол, начат Google, отдан Linux Foundation. Привязки JSON-RPC, gRPC и HTTP+JSON, не SSH. [established] https://a2a-protocol.org/llms.txt (2026-10-06). SSH здесь только туннель или способ поднять HTTP-эндпоинт. Тащить A2A внутрь SSH-канала для восьми своих машин не стоит.

Стоит ли. Один тонкий fan-out (`ssh` циклом или Ansible ad-hoc) стоит. Полный Ansible имеет смысл на Windows-хабе, где уже больно от cmd.exe и от ключей администраторов. Salt, Fabric и отдельная шина агентов для этого размера не окупаются.

3. Бэкапы и перевозка данных

restic. Бэкенд SFTP: `restic -r sftp:user@host:/srv/restic-repo init`, затем `backup`. Без пароля на SSH автоматический прогон не живёт, потому что restic не отвечает на запрос пароля. Порт 2222 задаётся в `~/.ssh/config` или URL `sftp://user@[::1]:2222//srv/restic-repo` (два слэша: настройки, потом абсолютный путь). Документация stable, в выдаче дата 2026-07-05, версия 0.19.1. [established] https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html . Репозиторий шифруется паролем restic. Потеря пароля необратима. Один бинарь на Windows, macOS и Linux.

Borg. Репозиторий по SSH, на сервере ключ клиента заперт:

```
command="borg serve --restrict-to-repository /home/<user>/repository",restrict ssh-ed25519 AAAA...
```

Это страница Borg 2.0.0b (master), дата выдачи 2026-10-20. Флаг `--rest` относится к текущему протоколу Borg 2. Стабильный 1.x в этом прогоне не открыт, его строка forced command может отличаться. [single-source] на точную строку 2.x. https://borgbackup.readthedocs.io/en/master/deployment/hosting-repositories.html . Смысл `restrict` и `command=` подтверждён man sshd независимо от Borg.

Kopia. `kopia repository connect sftp --path=... --host=... --username=...`, порт по умолчанию 22, есть `--keyfile` и внешняя команда ssh. [established] https://kopia.io/docs/reference/command-line/common/repository-connect-sftp/

Что куда. Syncthing режет файл на блоки 128 КиБ–16 МиБ, SHA256, watcher копит 10 секунд, полное сканирование по умолчанию раз в час. Одновременная правка двух сторон даёт `<filename>.sync-conflict-<date>-<time>-<modifiedBy>.<ext>`, и этот файл сам разъезжается по кластеру. Регистр имён на Windows и macOS конфликтует с регистром Linux. [established] https://docs.syncthing.net/users/syncing.html . Вольт на ~130 тысяч заметок, который люди правят с нескольких машин, это класс Syncthing. Точечный снимок «как было во вторник» и шифрованная копия вне кластера это класс restic или kopia по SSH на VPS. Тащить вольт ещё и непрерывным rsync поверх Syncthing значит получить вторую историю конфликтов.

SQLite. Обычный rsync живого файла может склеить страницы разных транзакций. Если за время копирования никто не подключён и нет hot journal, копия совпадёт. Иначе она может быть битой. [established] https://www.sqlite.org/rsync.html (страница обновлена 2025-11-13). Online Backup API копирует в снимок, источник залочен только на время чтения страниц, писатели не стоят всю копировку. По завершении назначение является снимком. [established] https://www.sqlite.org/backup.html (обновлено 2025-11-13). Там же названы `VACUUM INTO` и `sqlite3_rsync`. `sqlite3_rsync ORIGIN REPLICA`: одна сторона может быть `USER@HOST:PATH`, обе базы могут быть живыми, реплика становится снимком на момент старта. Ограничение WAL и одинакового размера страницы снято в 3.50.0 (2025-05-29). На macOS ssh PATH узкий, бинарь кладут в `$HOME/bin`, `/usr/local/bin` или `/opt/homebrew/bin`, либо дают `--exe`. Автор sqlite3_rsync прямо пишет, что завести SSHD на Windows ему не удалось, и инструкции для Windows нет. Для хаба: снимок локально (`VACUUM INTO` или backup API), затем restic/kopia везёт уже закрытый файл. Копировать живой `leads.db` через Syncthing или rsync нельзя. Это вывод из правила консистентности SQLite, не отдельная страница Syncthing.

SSHFS. В документации Borg pull-режима: inode по SSHFS нестабильны, детектор изменений Borg надо переключать на `ctime,size`. [established] https://borgbackup.readthedocs.io/en/latest/deployment/pull-backup.html (выдача 2026-10-20). Постоянно монтировать вольт или SQLite по SSHFS не стоит. Разовый забор дерева на VPS терпим.

Модели. Крупные редко меняющиеся файлы лучше в restic/kopia (дедуп, шифрование, одна копия на VPS), не в кольце Syncthing между ноутбуками. Отдельной страницы «модели против Syncthing» в прогоне нет. Это вывод из размера блока и конфликт-копий Syncthing.

Стоит ли. Да, один репозиторий. Для разношёрстных ОС проще restic или kopia, чем связка клиент/сервер Borg. Borg оправдан, если уже есть навык и нужен `restrict` на ключе. Вольт не переезжает с Syncthing на SSH.

4. GPU хаба

Ollama слушает `127.0.0.1:11434`. Наружу его выводят переменной `OLLAMA_HOST`. На Windows приложение наследует переменные пользователя: выйти из трея, задать переменную учётной записи, снова запустить из меню Пуск. [established] https://docs.ollama.com/faq (прочитано 2026-10-23). `ollama ps` показывает долю GPU. Модель держится 5 минут (`OLLAMA_KEEP_ALIVE`, параметр `keep_alive`, `-1` значит не выгружать). Перегруз очереди даёт 503, размер очереди крутит `OLLAMA_MAX_QUEUE`. Модели на Windows: «внутренний путь лаборатории»%username%\.ollama\models`.

Туннель, сервер остаётся на localhost:

```
ssh -N -L 11435:127.0.0.1:11434 user@hub
```

Локальный порт 11435, чтобы не столкнуться со своим Ollama. Это обычный LocalForward, тот же приём описан у VS Code. [established] механизм форвардинга: https://code.visualstudio.com/docs/remote/ssh . Страницы-блоги про туннель к Ollama в источники рецепта не ставлю: официальный FAQ туннель не описывает, он описывает bind.

llama.cpp. Официальный README сервера: HTTP API, маршруты chat completions совместимы с OpenAI и есть совместимость с Anthropic Messages, плюс слои на GPU (`-ngl` / `--n-gpu-layers`, `auto` или `all`), несколько GPU (`--split-mode layer|row`, `--tensor-split`). [established] https://github.com/ggml-org/llama.cpp/blob/master/tools/server/README.md (прочитано начало файла, 2026-10-23). Флаг `--host` в прочитанном куске не попался. Блоговые команды с `--host 127.0.0.1 --port 8080` не цитирую как документацию. Тот же рисунок, что у Ollama: слушать localhost, с ноутбука забирать туннелем.

vLLM и whisper.cpp в этом прогоне не открывались. [no source found]. На Windows-хабе vLLM разумно ожидать внутри WSL2, не как нативный сервис. Это ожидание без страницы vLLM.

WSL2 и GPU. Нужны Windows 11 или Windows 10 21H2, драйвер NVIDIA на стороне Windows, дальше CUDA в дистрибутиве. Документ обновлён 2024-11-19, старше 18 месяцев, новее официальной замены в прогоне нет. [established] https://learn.microsoft.com/en-us/windows/wsl/tutorials/gpu-compute . Фраза «несколько GPU, но только один за раз» стоит в разделе про `MESA_D3D12_DEFAULT_ADAPTER_NAME` после настройки DirectML. Её нельзя переносить на CUDA в WSL: соседний раздел отсылает к руководству NVIDIA CUDA on WSL, которое здесь не открыто.

Session 0 и CUDA. Изоляция сессии запрещает UI и оконные сообщения. Она не запрещает сокет на localhost. Процесс в сессии пользователя (приложение Ollama из меню Пуск) слушает порт, пока жив этот вход. После выхода пользователя процесс меню умирает, туннель попадает в connection refused. Служба Windows может слушать порт из Session 0. Утверждение «CUDA из службы Windows всегда работает» или «никогда не работает» в этом прогоне не подтверждено. [no source found]. Практический критерий: `ollama ps` по SSH показывает `100% GPU` или нет.

Сон. Пока хаб спит, туннеля нет. Точные команды `powercfg` в прогоне не открыты. [no source found] на флаги. Факт для Codex и Claude тот же: спящая машина недоступна.

Очередь. `OLLAMA_MAX_QUEUE` это предел, не планировщик на две карты. Для пачки задач хватает одной очереди на VPS: следующий `ssh` стартует, когда предыдущий вернул код. Отдельный Slurm на две GPU не нужен.

Стоит ли. Да, это самая высокая отдача SSH на этом флоте. Слушать 127.0.0.1, пускать ноутбуки туннелем по tailnet, не открывать 11434 в tailnet без нужды. Две карты: у llama.cpp есть `--split-mode` и `--tensor-split`. У Ollama выбор GPU отдельной официальной страницей здесь не зафиксирован.

5. Ключи и доступ

Что уже есть (ключ ed25519 на машину, публичная часть по шине, authorized_keys только для машин реестра) совпадает с базовой моделью OpenSSH. [established] https://man.openbsd.org/sshd.8 . Слабое место не алгоритм, а то, что полный шелл на чужой машине и агент, который читает чужой текст, это одна и та же учётка.

Принудительная команда. В `authorized_keys`:

```
restrict,command="/usr/local/bin/fleet-exec" ssh-ed25519 AAAA... automation@vps
```

Клиентская команда игнорируется. Исходный текст лежит в `SSH_ORIGINAL_COMMAND`. `restrict` выключает проброс портов, агента, X11, PTY и `~/.ssh/rc`. Образец из man: `restrict,command="dump /home"`. [established] https://man.openbsd.org/sshd.8 (2 сентября 2026). Тот же эффект даёт `ForceCommand` в sshd_config, он перекрывает и команду из ключа. [established] https://man.openbsd.org/?query=sshd_config&apropos=0&sec=5&arch=default&manpath=OpenBSD-7.8 (OpenBSD 7.8, дата страницы 2025-10-04). Обёртка должна белить список команд, писать квитанцию и не вызывать шелл через `sh -c "$SSH_ORIGINAL_COMMAND"`. Иначе forced command ничего не ограничивает.

Сертификаты OpenSSH. Сервер хранит ключ CA (`TrustedUserCAKeys`), а не каждый публичный ключ. Срок и principals в сертификате. Вход root только с forced command: `PermitRootLogin forced-commands-only`. [established] sshd_config(5), та же страница. Smallstep `step-ca`: учебник показывает `TrustedUserCAKeys` и `HostCertificate`, срок пользовательского сертификата по умолчанию 16 часов. [single-source] https://smallstep.com/docs/tutorials/ssh-certificate-login/ (выдача, 2026-02-03). Для восьми машин свой CA на VPS имеет смысл, когда надоело раскатывать отзыв ключа по authorized_keys. Пока отзыв это «убрать строку из файла на шине», CA добавляет сервис, который сам должен быть жив.

Tailscale SSH. Сервер только Linux и macOS с открытым `tailscaled`, не вариант из App Store и не Windows. Клиент с любой ОС. Команда `tailscale set --ssh` перехватывает порт 22 на Tailscale-IP, обычный sshd и authorized_keys не трогает. Доступ режет ACL. Смена политики обрывает уже открытые сессии за секунды. Check mode требует повторный вход в IdP, по умолчанию на 12 часов. Запись сессий есть. Ключи WireGuard на сессию, не вечные authorized_keys. [established] https://tailscale.com/docs/features/tailscale-ssh (last validated 5 января 2026). Для этого флота Tailscale SSH закрывает VPS и те Mac, где стоит открытый tailscaled. Windows-хаб и остальные Mac остаются на обычном sshd. Включать `--ssh` на всех подряд нельзя: существующие сессии на Tailscale-IP зависают в момент переключения, это написано в той же странице.

Проброс агента. По умолчанию выключен. Сокет `SSH_AUTH_SOCK` на удалённой стороне может подписывать этим ключом, пока сессия жива. CVE-2023-38408: проброшенный агент и PKCS#11 давали удалённое выполнение кода, OpenSSH 5.5–9.3p1, исправлено в 9.3p2. 9 апреля 2025: в sshd 7.4–9.9 `DisableForwarding` не выключал X11 и агента, вопреки документации. Исправление в заметках OpenSSH 10.0. [established] https://www.openssh.org/security.html . Агентам флота `ForwardAgent` не давать. Прыжок на третью машину делать `ProxyJump`, не агентом. Отдельной страницы man ssh_config в прогоне нет. Поведение «дефолт не пробрасывать» подтверждено той же заметкой OpenSSH («agent forwarding is not requested by default»).

Другие дыры, которые касаются флота, если sshd торчит хотя бы в tailnet. CVE-2024-6387 (1 июля 2024): гонка в portable OpenSSH 8.5p1–9.7p1, возможное выполнение кода от root не на OpenBSD. Относится ли сборка Microsoft к portable в этом прогоне не установлено. [no source found] на вердикт по Windows OpenSSH. CVE-2025-26465 (18 февраля 2025): подмена сервера при включённом `VerifyHostKeyDNS`, опция по умолчанию выключена, клиенты 6.8p1–9.9p1. CVE-2025-26466: pre-auth DoS sshd 9.5p1–9.9p1, смягчается `PerSourcePenalties`. [established] https://www.openssh.org/security.html .

Prompt injection. OWASP LLM01:2025: прямая и непрямая инъекция (файл, страница, письмо) может довести до выполнения команд в подключённых системах. Полной защиты нет. Рабочие меры из той же страницы: минимальные права, человек на опасном действии, недоверенный текст отдельно от инструкции. [established] https://genai.owasp.org/llmrisk/llm01-prompt-injection/ . Для флота это значит: у агента нет ключа с полным шеллом на другие узлы. У автоматизации свой ключ, `restrict` и белый список. Человеческий ключ интерактивный и не лежит в среде агента. Чтение вольта, почты и тредов это как раз непрямой канал.

Чужие личные машины. Технический отзыв: убрать строку ключа или строку ACL Tailscale SSH (сессии рвутся сразу на Tailscale SSH). Согласие и право человека выключить sshd у себя документом не покрыто. [no source found] на юридическую форму. Технический минимум из разделов выше: отдельный ключ, не админский, forced command или хотя бы `from=` на IP tailnet, срок, если появится сертификат, и способ выключить одной строкой без переустановки ОС.

Логи. sshd по умолчанию пишет начало, вход и конец. Точный канал Event Log у Windows OpenSSH в этом прогоне не открыт. [no source found] на имя журнала. Квитанция флота всё равно нужнее журнала sshd, потому что текст команды туда сам не падает.

Стоит ли. Сначала разделить ключи и запереть автоматизацию. CA и Tailscale SSH подключать на VPS, не снося текущую сетку ed25519. Teleport в прогоне не открыт. [no source found]. Для семьи он тяжелее, чем step-ca или Tailscale SSH.

6. Живучесть

Сон и крышка. И Remote Control, и Codex прямо говорят: спящая или выключенная машина недоступна, процесс должен продолжать работать. [established] страницы Claude Remote Control и ChatGPT remote connections. Пользовательский sshd под launchd умирает вместе с выгрузкой пользователя, если нет фонового входа. Системный sshd на macOS (Remote Login) и служба Windows sshd переживают выход пользователя как службы, но ноутбук в сне их не слушает. Точные флаги pmset и powercfg не открыты. [no source found].

macOS обновления. Смена бинаря или подписи сбрасывает смысл старого гранта TCC. Поломка FDA у `sshd-keygen-wrapper` на 26.1 описана выше. [single-source]. Пользовательский sshd на 2222 от этого конкретного бага может не зависеть, потому что это другой процесс, и одновременно он не получает гранты системного wrapper. Проверка после каждого крупного обновления macOS: `ssh -p 2222 -o BatchMode=yes -o ConnectTimeout=5 host true` и отдельно пробный `osascript`, если этот путь ещё нужен.

Windows OpenSSH. Встроенная версия обслуживается Windows Update, это рекомендуемый путь, не сторонняя сборка. [established] https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse (поиск, 2026-09-08). Обзор: Server 2025 установлен, но не включён; клиентские выпуски с Windows 10 1809 ставятся как optional feature. [established] https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh-overview (поиск, 2026-08-25). sshd читает конфиг при старте службы, правка требует перезапуска. [established] https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh-server-configuration (поиск, 2026-08-25). Обновление может вернуть `Match Group administrators` и снова увести ключи админа в `administrators_authorized_keys`. Это вывод из поведения, описанного Ansible, плюс факта перечитывания конфига. После обновления сверять этот блок.

MagicDNS. Имя машины плюс имя tailnet, вид `monitoring.адрес частной сети`. Короткие имена работают через search domain. MagicDNS включён по умолчанию как возможность, не как обязанность. [established] https://tailscale.com/docs/features/magicdns.md (last validated 5 января 2026) и https://tailscale.com/docs/reference/dns-in-tailscale (поиск, 2025-12-22). Tailscale SSH при сломанном DNS падает, потому что known_hosts завязан на имена. [established] таблица ограничений на странице Tailscale SSH. В `~/.ssh/config` держать `HostName` как MagicDNS-имя. Стабильность конкретных 100.x адресов в прогоне не подтверждена. [no source found].

Проба. С VPS по расписанию:

```
ssh -o BatchMode=yes -o ConnectTimeout=5 -o ConnectionAttempts=1 host true
```

Код не ноль: узел спит, sshd мёртв или ключ отвергнут. Это не отличает сон от поломки. Второй сигнал: свежесть последней успешной пробы и отдельно «Tailscale online». Один SSH-фейл сон не лечит. `BatchMode` не даёт зависнуть на пароле. Флаг описан в клиентском ssh, man ssh здесь не открыт. Поведение «не спрашивать пароль в скрипте» использую как рецепт без цитаты man. [no source found] на текст man ssh.

Стоит ли. Да, одна проба с VPS и политика «хаб не спит, пока он GPU». Сторож, который сам включает sshd после каждого обновления, на личных ноутбуках окупается только после повторной поломки. Сейчас достаточно пробы и строки в квитанции.

C. Что строить дальше

Порядок по отдаче для этого флота. Усилие и риск оценены качественно, не по часам из источника.

1. Туннель к Ollama или llama-server на хабе, bind только 127.0.0.1. Усилие малое. Риск низкий, если порт не слушает tailnet. Отдача высокая: ноутбуки получают GPU без копирования моделей. [established]. Допущение: процесс на хабе жив, когда никто не сидит за консолью, и хаб не засыпает.

2. Отдельный ключ автоматизации с `restrict,command=` и белым списком. Человеческие ключи агенту не видны. Усилие малое. Риск: сломать текущие скрипты, если белый список узкий. Отдача высокая против непрямой инъекции из вольта и почты. [established]. Допущение: сегодня хотя бы один агент может запустить `ssh` на другой узел своим ключом.

3. Снимок живых SQLite (`VACUUM INTO` или backup API) и один репозиторий restic или kopia по SFTP на VPS. Вольт остаётся на Syncthing. Усилие среднее. Риск: потерять пароль репозитория. Отдача высокая для `leads.db` и соседей. [established]. Допущение: базы ещё едут файлом, а не снимком.

4. tmux на VPS и на тех Mac, где агент должен пережить обрыв. Руление человеком через Remote Control или Codex pairing, не через AppleScript. Усилие малое. Риск: машина уснула, сессия «жива» только на бумаге. Отдача высокая для видимости. [established] для Remote Control и Codex. Допущение: подписка Claude или аккаунт ChatGPT уже есть на этих машинах.

5. Проба SSH с VPS плюс запрет сна на хабе. Усилие малое. Риск низкий. Отдача средняя, пока сон редкий, и высокая в первый же потерянный вечер. Допущение: VPS и есть дежурный.

6. Показ окна на Windows только через `schtasks /IT` или `psexec -i N`, и только когда человеку надо увидеть GUI. Не делать из этого шину. Усилие среднее. Риск: антивирус, смена номера сессии, UAC. Отдача точечная. [established] на механизм, документ PsExec старше 18 месяцев.

7. Tailscale SSH на VPS и на Mac с открытым tailscaled. Сетку ed25519 не снимать, Windows ею и живёт. Свой step-ca позже, если отзыв ключей начнёт занимать вечер. Усилие среднее. Риск: `--ssh` обрывает текущие сессии на Tailscale-IP. Отдача средняя при восьми узлах. [established].

Рекомендация. Следующим шагом делать пункты 1, 2 и 3 вместе: GPU-туннель, запертый ключ автоматизации, снимок SQLite в restic или kopia. Уверенность [established]. Главное допущение: хаб можно удержать включённым и с живым процессом модели, а автоматизацию можно отсадить на отдельный ключ, не ломая человеческий вход. Ansible, сертификаты и Tailscale SSH на все узлы этот шаг не блокируют.

Покрытие. Сильнее всего закрыты официальные документы Claude, Codex, OpenSSH, Tailscale, Ansible, SQLite, Ollama, VS Code, OWASP, Syncthing, Borg 2, Kopia, MCP, llama.cpp README (частично). Слабее: точные флаги tmux, vLLM, whisper, Salt, Fabric, parallel-ssh, Teleport, журнал событий Windows OpenSSH, pmset/powercfg, потребительский SSH-режим Claude Desktop вне 3P, пригодность CUDA из службы Windows. Форум Apple по TCC 26 прочитан только через выдачу поиска.

Список источников

https://code.claude.com/docs/en/remote-control
https://claude.com/docs/third-party/claude-desktop/ssh-remote-sessions
https://learn.chatgpt.com/docs/remote-connections
https://code.visualstudio.com/docs/remote/ssh
https://code.visualstudio.com/docs/remote/faq
https://code.visualstudio.com/docs/agents/remote-agent-sessions
https://www.jetbrains.com/help/rider/Remote_development-a.html
https://www.jetbrains.com/remote-development/gateway/
https://man.openbsd.org/sshd.8
https://man.openbsd.org/?query=sshd_config&apropos=0&sec=5&arch=default&manpath=OpenBSD-7.8
https://www.openssh.org/security.html
https://tailscale.com/docs/features/tailscale-ssh
https://tailscale.com/docs/features/magicdns.md
https://tailscale.com/docs/reference/dns-in-tailscale
https://docs.ansible.com/projects/ansible/latest/os_guide/windows_ssh.html
https://docs.ansible.com/projects/ansible/latest/os_guide/intro_windows.html
https://docs.pyinfra.com/connectors/ssh.html
https://modelcontextprotocol.io/specification/2025-11-25/basic/transports
https://a2a-protocol.org/llms.txt
https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html
https://borgbackup.readthedocs.io/en/master/deployment/hosting-repositories.html
https://borgbackup.readthedocs.io/en/latest/deployment/pull-backup.html
https://kopia.io/docs/reference/command-line/common/repository-connect-sftp/
https://www.sqlite.org/backup.html
https://www.sqlite.org/rsync.html
https://docs.syncthing.net/users/syncing.html
https://docs.ollama.com/faq
https://github.com/ggml-org/llama.cpp/blob/master/tools/server/README.md
https://learn.microsoft.com/en-us/windows/wsl/tutorials/gpu-compute
https://learn.microsoft.com/en-us/windows-hardware/drivers/wdf/session-zero-guidelines-for-umdf-drivers
https://learn.microsoft.com/en-us/archive/blogs/hmahrt/communication-between-windows-services-and-applications-in-windows-vista-and-higher
https://learn.microsoft.com/en-us/sysinternals/downloads/psexec
https://learn.microsoft.com/en-us/windows/win32/taskschd/schtasks
https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-vista/cc722152(v=ws.10)
https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse
https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh-overview
https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh-server-configuration
https://smallstep.com/docs/tutorials/ssh-certificate-login/
https://genai.owasp.org/llmrisk/llm01-prompt-injection/
https://developer.apple.com/forums/thread/806187

Простыми словами

SSH для наших компьютеров это трубка, по которой можно сказать команду, забрать файл и протянуть провод к программе на другом конце. Показать человеку окно на Windows через эту трубку само не получается: служба сидит в служебной комнате, а рабочий стол в другой. На Маке нажимать меню скриптом можно, пока система после обновления не отберёт разрешение.

Самое полезное сейчас: с ноутбука тихо ходить на видеокарты хаба по проводу, не выставляя программу в сеть. Второе: ключ, которым робот ходит на другие машины, умеет только список разрешённых команд. Третье: базу данных сначала закрыть в снимок, потом уже копировать. Живой файл базы обычная синхронизация может собрать криво. Заметки вольта пусть как ехали через Syncthing, так и едут.
