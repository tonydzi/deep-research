# DR26-09-04-HUB-02-2326 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!-- DR-LEG DR26-09-04-HUB-02-2326 | rail=claudeai | account=почтовый ящик | model=Fable 5 High | mode=Research | chat=https://приватный чат | collected=2026-09-08T12:28:59.693Z | node=ноутбук -->

# Deep Research DR26-09-04-HUB-02-2326: два разных «bb» — переносимость харнеса Browserbase и целесообразность установки get-bb/bb

## TL;DR
- **bb #1 (Browserbase internal agent)** — внутренний закрытый Slack-агент; исходного кода нет, есть подробный архитектурный пост (browserbase.com/blog/internal-agents, 16.04.2026, Kyle Jeong и Derek Meegan). Его паттерны (эфемерный sandbox + integration proxy + scoped-permissions + skills) переносимы и напрямую бьют по вашим болям с plaintext-секретами и зависающими апрувами, но полная реализация — оверинжиниринг для флота одного человека. Берём три паттерна, не берём облачную инфраструктуру.
- **bb #2 (get-bb/bb, MIT, TypeScript)** — устанавливаемый продукт, оркестратор поверх Claude Code/Codex/Cursor/ACP. Ставить стоит осторожно и на один узел: он даёт таймлайн, фон-задачи, плагины и мульти-машинность, но его модель мульти-девайса — клиент-сервер поверх Tailscale, а НЕ синк файлов. **Синкать его каталог данных через Syncthing нельзя — это живые SQLite-базы, будет коррупция.**
- Числа bb #1 («100% покрытие feature-request», «99% первых ответов <24ч») исходят из маркетингового блога и подкаста, база сравнения и период не названы — считать их непроверяемыми.

## Key Findings

### bb #1 — Browserbase internal agent
- ✅ Это **внутренний инструмент**, не продукт. Живёт в Slack как `@bb`, пишет PR, расследует прод-сессии, ходит в Snowflake/HubSpot/Pylon/Grafana. Кода харнеса публично нет; есть только архитектурный блог и подкаст. Источник: browserbase.com/blog/internal-agents.
- ✅ Рантайм — **OpenCode** (opencode.ai), open-source агентный runtime (TypeScript/Bun, HTTP-сервер на Hono, OpenAPI 3.1 SDK). Browserbase «не трогает» core-loop, всё доменное — сверху.
- ✅ Sandbox — **эфемерная Linux-VM** с pre-warmed снапшотом, ребилдящимся каждые 30 минут через cron; idle-out через 30 минут. Внутри: клонированные репозитории в `/knowledge/`, agents monorepo, OpenCode, bun/git/gh/ripgrep/Tailscale.
- ✅ Агент получает ровно **6 инструментов**: `read`, `write`, `edit`, `exec` (исполнение JS через proxy), `safebash` (allowlisted shell), `skill`.
- ✅ **JIT-креды**: sandbox грузится только с `BB_PROXY_URL`, `BB_SESSION_TOKEN` (ротируемый, TTL в KV) и `AUTOMATION_BYPASS_TOKEN`. Реальные секреты живут в serverless integration proxy. Механизм — не Vault/STS, а **самописный serverless-proxy с проверкой токена в KV и glob-матчингом `service.method`**. Для узкого набора интеграций (провайдеры моделей и code hosting) — сетевой credential brokering: firewall перехватывает исходящий HTTP и подставляет ключ на egress, env-переменная — плейсхолдер `"credential-brokered"`.
- ✅ **«Permissions computed fresh»** технически = не policy-engine типа OPA/Cedar на каждый вызов, а **`PermissionConfig` на сессию** с двумя измерениями: список разрешённых `services` (glob-паттерны, напр. `["crm.*", "support.getIssue"]`) и булевы флаги на 6 инструментов. Интерактивные Slack-сессии — полный доступ; фоновые webhook-сессии — жёстко зашитые права в момент вызова. Это фильтрация по контексту события + service-level RBAC (например, Snowflake-роль только SELECT), а не пересчёт политик движком.
- 🤔/❌ **Метрики**: «feature request pipeline runs at 100% coverage with zero human effort» и «reduce 99% of our first response time to <24hrs» — прямые цитаты из блога; в подкасте (insecureagents.com/episodes/derek-meegan, эпизод «One Harness, Zero Standing Secrets») те же числа. Ни база сравнения, ни абсолютные объёмы, ни период не приведены. Расследование сессии «30–60 минут → одно сообщение в Slack» — тоже без выборки. Помечаю как непроверяемый маркетинг.

### Оригинальность bb #1
- ✅ **Code mode** — не идея Browserbase. Первым публично паттерн «модель пишет код против типизированного SDK вместо прямых tool-call» показал **Cloudflare** («Code Mode», demo Риты Козловой, VP AI & Developers, на MCP Night; blog.cloudflare.com/code-mode). **Anthropic** независимо описал то же в посте «Code execution with MCP: building more efficient AI agents» (авторы Adam Jones и Conor Kelly, ноябрь 2025, anthropic.com/engineering/code-execution-with-mcp). У Browserbase есть свой пост «Code Mode Is All You Need» (22.07.2026, Kyle Jeong), но это их изложение общего паттерна, не первоисточник.
- ✅ **Замеренные выигрыши code-mode** (из первоисточников):
  - Anthropic (Adam Jones, Conor Kelly, ноябрь 2025): на workflow Google Drive→Salesforce «token usage fall from 150,000 to 2,000, a 98.7% reduction».
  - Cloudflare (Matt Carey, «Code Mode: give agents an entire API in 1,000 tokens», 20.02.2026): Cloudflare API имеет свыше 2 500 эндпоинтов (>2 млн токенов), «With Code Mode, we collapsed all of it into two tools and roughly 1,000 tokens» — сокращение ~99,9%.
  - Независимо (Particula Tech, со ссылкой на AIMultiple): «AIMultiple saw a 78.5% input-token cut with GPT-4.1… per-call latency rose about 7% in controlled tests» — то есть выигрыш по токенам оплачивается ~7% ростом задержки на вызов.
- ✅ Sandbox+approval-режимы — уже есть у всех: OpenAI Codex, Cursor background agents, Claude Agent SDK, gVisor/Firecracker, E2B, Modal, Daytona. Эфемерная VM со снапшотами — стандарт индустрии.
- 🤔 **Что реально своё у bb #1**: не отдельные кирпичи, а их сборка — «один core-loop + lazy-load скиллов и прав + serverless credential broker + Slack как интерфейс». Сам блог это признаёт: «The architecture is simple enough to replicate… Build your own bb». Новизны на уровне алгоритма нет; ценность — в инженерной компоновке.

### bb #2 — get-bb/bb
- ✅ **MIT, open-source, порядка 3,2–3,4k звёзд, ~380–414 форков** (github.com/get-bb/bb; счётчик активно растёт — прямой съём страницы дал 3,4k/414, снапшот энричера — 3,2k/378), монорепо на pnpm/turbo, TypeScript. Авторы: начал @_ymichael, развивает Sawyer Hood.
- ✅ Это **оркестратор агентов**, работает поверх Claude Code, Codex, Cursor, Pi, OpenCode, Grok, omp, Hermes и любого ACP-агента — **на ваших подписках**. Четыре равноправных поверхности: desktop-app, web-app, CLI, HTTP API.
- ✅ **Windows — только через WSL2**; нативные PowerShell/CMD не поддерживаются (README прямо). Desktop-app — только macOS arm64; Linux x64 AppImage — alpha; иначе `npx bb-app@latest`, порт 38886.
- ✅ **Плагинная архитектура — ядро продукта**. Сам bb во многом состоит из плагинов: Automations, Side chat, bb connect, Custom instructions, Secrets — built-in enabled; GitHub, Docs, Memory, Tasks — из стора. Плагин может добавлять agent-tools и skills, CLI-субкоманду, sidebar-страницы, background-services и **scheduled jobs**, HTTP/RPC-эндпоинты и **свой SQLite-стор**.
- ✅ **Мульти-девайс — клиент-сервер, а не синк файлов**. Один bb-сервер хранит всё состояние в **одной SQLite-базе** (источник истины) и enroll-ит несколько машин через host-daemon по WebSocket. Удалённый доступ — через **bb connect** (до 20 серверов и 20 машин на аккаунт с 0.39.0) или **Tailscale Serve**; сервер с 0.36.0 биндится на loopback, его API неаутентифицирован и позволяет исполнение команд. Официальный гайд — docs/multiple-devices.md.
- ✅ **Секреты**: built-in-плагин Secrets (с 0.0.30, 14.07.2026) — «Agents can securely request API keys and other credentials without exposing their values in the conversation or transcript». CLI — `bb secret request`. ❌ Точный механизм хранения на диске (keychain / шифрование / plaintext) в первоисточниках не задокументирован — не нашёл (проверять `plugins/secrets` в репозитории).

## Details

### A. Фактология bb #1 (вопросы A1–A5)
Разбор выше в Key Findings полностью отвечает на A1–A5. Ключевое инженерное уточнение по A4 и A5: у Browserbase **нет** ни Vault, ни STS, ни OPA/Cedar. Их «JIT» — это ротируемый session-token + serverless-функция, держащая реальные ключи в своём окружении; «least-privilege на вызов» — это `PermissionConfig`, привязанный к источнику события (webhook несёт intent → узкий набор services/tools), плюс традиционный RBAC на уровне самих сервисов. Это дешевле и проще, чем policy-engine, и именно поэтому переносимо.

### C. Применимость к вам (главный блок)

**C9 — Секреты: с plaintext-стора на JIT-брокер без потери автономности.** Ваша модель (агент сам логинится, сам подставляет пароли в формы) несовместима с истинным network-brokering Browserbase (там агент вообще не видит ключ, а ходит через proxy к API — но у вас работа идёт через браузерные формы, куда пароль физически надо ввести). Поэтому реалистичный путь — не proxy, а **локальный секрет-брокер + шифрование на диске**, чтобы убрать plaintext, сохранив выдачу значения агенту в момент нужды.

Сравнение вариантов для гетерогенного флота Win+Mac+Linux без корпоративного IdP:

| Вариант | Цена | Сложность | Офлайн | Переживает Syncthing | Ремонт не-инженером |
|---|---|---|---|---|---|
| **SOPS + age** | бесплатно (оба OSS) | низкая | да, полностью офлайн (age — офлайн-метод) | да — шифртекст в синке безопасен, значения зашифрованы AES-256-GCM | средне: правишь `.sops.yaml`, ключ age в keychain |
| **OS keychain / DPAPI** | бесплатно | средняя (три ОС — три API) | да | нет (keychain не синкается) | сложно на 6 машинах |
| **HashiCorp Vault dev-mode** | бесплатно | высокая | да, но dev-mode теряет данные при рестарте | нет | плохо |
| **Infisical (self-host)** | бесплатно self-host; Cloud free до 5 identities, Pro $18/identity/мес | высокая (сервер) | нет (нужен сервер) | нет | плохо |
| **1Password Service Accounts / Connect** | платно (подписка) | средняя | частично | нет | средне |
| **Doppler** | платно; self-host отсутствует | низкая (лучший DX) | нет | нет | средне |

**Вердикт C9: SOPS+age.** Единственный вариант, который переживает Syncthing (зашифрованный файл — просто файл), работает офлайн на всех трёх ОС и не требует поднимать сервер. SOPS шифрует только значения, оставляя ключи читаемыми (нормальные diff-ы), приватный ключ age кладёте в OS-keychain каждой машины (руками, один раз), а зашифрованный `secrets.yaml` синкаете как обычно. Агент дешифрует в момент работы. Это убирает plaintext на 6 машинах — вашу главную дыру — за один вечер и без внешних зависимостей.

**C10 — Code Mode Sandbox на Windows.** На Windows 11 нет seccomp и нет gVisor. Доступное: WSL2, Windows Sandbox (только Pro/Enterprise), Docker Desktop, Firejail (на Linux-узлах). **Цена WSL2**: доступ к файлам на `/mnt/c` идёт через протокол 9P и в разы медленнее — по замерам сообщества до 10× на workload из многих мелких файлов (создание сотен файлов, `npm install`, `go build`), потому что каждый syscall переводится в 9P-round-trip; msize на Windows-клиенте не настраивается. Фикс — держать рабочие файлы внутри Linux-ФС WSL2 (`~/`), а не на диске Windows; паллиатив — `metadata,noatime` в `/etc/wsl.conf`. Смысл сандбокса **резко падает**, если агент и так работает под вашим пользователем на личной машине с доступом к вашим файлам: изоляция Browserbase нужна, потому что там исполняется недоверенный код с клиентскими данными и комплаенсом. У вас доверенный код на личном железе — **основную ценность даёт не изоляция процесса, а ограничение того, что агент может сделать** (least-privilege на вызов, C11), а не seccomp.

**C11 — Least-privilege на вызов поверх Claude Code hooks.** Минимальная рабочая реализация: `PreToolUse`-хук, который читает `tool_input` из stdin и возвращает `hookSpecificOutput.permissionDecision: deny/ask/allow` (или exit code 2 для блокировки). Это deterministic-слой, срабатывающий до каждого matching tool-call. Можно вычислять разрешённый набор по типу события/сессии (аналог `PermissionConfig` bb #1): например, ночная рутина деплоя получает allow только на git/файловые операции и deny на всё, что связано с деньгами/удалением/чужими секретами. **Две критичные засады из первоисточников (code.claude.com/docs/en/hooks)**: (1) exit code 1 НЕ блокирует — блокирует только exit 2; JSON парсится только на exit 0; (2) при инструменте в allow-list в `settings.json` решение хука `deny/ask` игнорируется (баг anthropics/claude-code #18312) — поэтому не полагайтесь на хук поверх широкого allow-list, гейт должен быть основным слоем. Существующие 100+ рутин не ломаются, если хук по умолчанию возвращает allow и только ужесточает для явно перечисленных опасных классов.

**C12 — Event-driven траектории против «поднять фоновую сессию».** У bb #1 запуск идёт от события (Slack-сообщение, закрытый Pylon-тикет, прилетевший транскрипт Circleback), и права зашиваются в момент диспетча (`dispatchWebhook` с явным `permissions`). Это чинит именно ваш класс «безлюдная сессия зависла на апрув-окне»: если траектория порождена событием с заранее зашитым узким scope, ей нечего спрашивать у человека — действие либо структурно разрешено, либо структурно запрещено (возвращается 403/deny, сессия не висит, а падает быстро и логируется). Ваши замеры (93 из 101 блока апрува — вызовы планировщика из рутинной сессии; 3 зависшие сессии остановили спавн на 6ч42м) — это ровно тот антипаттерн, который event-scoped-права устраняют.

**C13 — Автоматический PR из тикета: открытые реализации.**
- **Claude Code GitHub Action** (`anthropics/claude-code-action@v1`, GA с 26.08.2025): `@claude` в issue/PR → читает, реализует, открывает PR; либо headless по любому GitHub-событию (`prompt` в YAML). Ставится «за вечер»: установить GitHub App, положить `<key-var>` в secrets, скопировать `claude.yml` в `.github/workflows/`. Код остаётся на раннерах GitHub.
- **SWE-agent, OpenHands** — open-source харнесы. Замеренный success-rate на SWE-bench Verified: OpenHands+CodeAct v2.1 (Claude 3.5 Sonnet) — 53,0% (проверенный open-source-результат на момент публикации Darwin Gödel Machine, апрель 2025). Топ-модели кластеризуются выше 80%, публичный лидерборд — низкие-средние 90-е на июль 2026. **Важная поправка из первоисточников**: OpenAI прямо признала, что «SWE-bench Verified no longer measures frontier coding capabilities» (контаминация; аудит 138 провалов o3 показал, что 59,4% провалов вызваны дефектами тестов, а не модели) и рекомендует SWE-bench Pro. Академический SWE-Bench+ (Aleithan et al., OpenReview, ICLR 2026) при устранении задач со слабыми тестами роняет OpenHands+CodeAct v2.1 на Verified с 52,4% до 26,8% (в другом срезе того же исследования — до 18,2%). То есть реальный success на ваших репозиториях будет заметно ниже лидербордных цифр — стройте свой eval-набор из 6+ реальных задач из своего бэклога.
- **Codex cloud** — облачный вариант, оптимизирован скорее под build-from-scratch.

**C14 — Что НЕ копировать (где у Browserbase оверинжиниринг для вас).**
- **Эфемерные облачные VM со снапшотами каждые 30 минут** — нужны, потому что bb исполняет недоверенный код и держит клиентские данные под комплаенсом. У вас личный флот — VM не нужны.
- **Network-level credential brokering с firewall-инъекцией ключей** — сложная инфраструктура ради того, чтобы sandbox «никогда не видел секрет». У вас агент обязан вводить пароли в формы — этот паттерн физически неприменим.
- **Serverless integration proxy** — имеет смысл при десятках интеграций и командной ротации токенов. Для одного человека это лишний слой; SOPS+age + PreToolUse-хуки закрывают тот же риск дешевле.
- **RBAC/ABAC двойной слой** — их ABAC оправдан multi-tenant SaaS. Вам хватит event-scoped прав через хуки.

### bb #2 — стоит ли ставить себе

**Что даёт поверх голого Claude Code:** единый таймлайн и управление многими агентами (Claude Code + Codex + Grok + Gemini через ACP) в одном месте; фоновые треды, которые можно вести/рулить/передавать; **встроенные scheduled jobs, dispatch-очередь со scheduled sends (плагин Send later) и concurrency-лимитом (по числу ядер, 0.41.0)**; плагинный слой, которым сам bb себя расширяет; удалённый доступ через bb connect/Tailscale; мобильный клиент. Это ближе к вашей боли «поднять фоновую сессию сам, работать ночью» — bb делает это через плагины Automations + Concurrency limit + Send later, а не через самописный спавн.

**Windows 11 через WSL2 рядом со 100+ рутинами:** bb на Windows живёт только в WSL2. Значит он будет работать в Linux-ФС WSL2, а ваши 100+ рутин и scheduled-tasks, если они завязаны на Windows-пути/планировщик Windows, — в другом мире. Мост `/mnt/c` медленный (9P). Практически: bb не «подхватит» ваши Windows scheduled-tasks; у него свой планировщик (плагинные scheduled jobs внутри его сервера). Это **параллельная система**, а не замена вашей.

**Конфликт плагин-слоя с вашими Claude Code хуками/скиллами:** прямого конфликта на уровне файлов нет — bb с 0.37.0 «looks for skills in the places each agent already reads», то есть подхватывает существующие скиллы Claude Code/Cursor без копии. Но два риска: (1) bb запускает Claude Code как под-провайдера через свой bridge — ваши `settings.json`-хуки Claude Code продолжат работать, но bb добавляет **свой** слой разрешений (permission mode на машину/тред, «child thread cannot exceed parent's permission mode»), который может конфликтовать по смыслу с вашими PreToolUse-гейтами (двойной гейт, труднее отлаживать); (2) плагинные scheduled jobs bb и ваши scheduled-tasks — два независимых планировщика, риск двойного запуска одной рутины.

**Ломает ли межмашинный синк (Syncthing):** ❌ **Да, если синкать каталог данных bb.** bb хранит состояние в **живых SQLite-базах** (better-sqlite3; согласно source-read у flaviocopes.com — «one SQLite database as the source of truth», dev-каталог `~/.bb-dev/<checkout-instance>/`, прод-каталог — вероятно `~/.bb`, но точный путь в первоисточниках не подтверждён) плюс WAL-сайдкары (`-wal`, `-shm`). Синк активной SQLite-базы файловым синкером — классический путь к коррупции. **bb сам это решает НЕ синком**, а клиент-серверной моделью: одна SQLite-база на сервере, остальные машины — enrolled hosts по WebSocket через bb connect/Tailscale (README: «Using bb on multiple devices», docs/multiple-devices.md; changelog 0.36.0: «Move to bb connect, or put bb behind Tailscale Serve, then remove and re-add each machine in Settings → Machines»). Вывод: **не кладите каталог данных bb в Syncthing-папку.** Используйте его родной bb connect. Ваш Syncthing для кода/секретов это не затрагивает, пока каталог данных bb исключён.

## Recommendations

**Сценарий A — «молотком и отвёрткой», один вечер.**
- Ставим: (1) SOPS+age на всех 6 машинах, мигрируем `credentials.md`/`.env`/`.json` в зашифрованный `secrets.yaml`, приватный ключ age — в OS-keychain каждой машины руками; (2) один общий `PreToolUse`-хук Claude Code, который блокирует классы «деньги/необратимое удаление/секреты третьим лицам» по exit 2.
- Закрывает боль: plaintext-секреты на 6 машинах; часть «тихо-зелёных» апрувов (опасное блокируется детерминированно).
- Меряем успех: 0 файлов с plaintext-секретами в синке; хук ловит фиксируемое число опасных вызовов в логах.
- Ломается если не взлетит: агент не может дешифровать — откат тривиален (age-ключ на месте, вернуть `credentials.md` из бэкапа).

**Сценарий B — неделя.**
- Ставим: (3) Claude Code GitHub Action на 2–3 ключевых репозитория для «issue → PR»; (4) event-scoped права — обёртка, которая по типу рутины отдаёт хуку узкий allow-set (аналог `PermissionConfig` bb #1); (5) реестр выходов с машинным доказательством приземления (чинит «выход заявлен у 17%»).
- Закрывает: ручной путь тикет→PR; зависания рутинных сессий на апрувах (C12); «тихо-зелёные» приборы.
- Меряем: доля рутин с проверяемым артефактом на выходе; число зависших безлюдных сессий → 0.
- Ломается: GitHub Action открывает мусорные PR — отключается удалением workflow-файла; event-scoping слишком узкий — рутина падает на deny (видно в логах, расширяете allow-set).

**Сценарий C — месяц.**
- Ставим bb #2 на **один** узел (Linux-анкер VPS или Windows-хаб через WSL2) как сервер, остальные машины — enrolled hosts через bb connect/Tailscale. Каталог данных bb **исключён из Syncthing**. Мигрируем рутины на плагинные Automations/scheduled jobs bb постепенно, начиная с 5–10 самых проблемных из 187 зависших.
- Закрывает: межмашинный дрейф задач (единый сервер-источник истины вместо 187 несинхронизированных); единый таймлайн вместо слепых фоновых сессий.
- Меряем: 187 зависших межмашинных задач → тренд вниз; 142 непринятых deploy-посылки → приняты через единый диспетч.
- Ломается: bb connect/Tailscale лёг — hosts офлайн, работа встаёт (bb это ретраит, но single-server = single point of failure); плагинные scheduled jobs дублируют старые Windows-tasks — выключаете старые перед миграцией каждой рутины.

**Пороговые триггеры смены курса:** если после A секретов в plaintext всё ещё >0 через неделю — проблема в дисциплине, не в инструменте. Если после B success-rate «issue→PR» на вашем реальном eval-наборе <30% — не масштабируйте, оставьте PR ручными. Если bb #2 в C требует >2 дней на стабильный enroll 6 машин — откатывайтесь на A+B (они самодостаточны).

**C17 — писать ли Derek Meegan / Browserbase.** Да, имеет смысл — но как обмен опытом, не как запрос кода (кода bb #1 нет и не будет). У вас есть что предложить: **журнал отказов агентных браузеров** и эксплуатационные данные флота из 6 узлов с сотнями автономных рутин — ровно тот тип продакшн-сигнала, который Browserbase собирает (их пост прямо зовёт «Build your own bb»). Контакт Derek Meegan публичен (почтовый ящик, derekmeegan.com; в Browserbase — Customer/Software Engineer, лид bb). Реалистичный размен: вы даёте им антипаттерны безлюдных сессий и метрики «тихо-зелёных» приборов, они — консультацию по event-scoped правам. Вклад в их OSS bb #1 невозможен (не OSS); но можно вложиться в **OpenCode** (их рантайм, MIT) или в **get-bb/bb** (bb #2, MIT, сотни форков, активно принимает внешние PR — в changelog десятки внешних контрибьюторов и co-authors).

## Итоговая таблица: наша боль → что лечит → чем заменить дешевле → вердикт

| Наша боль | Что из bb лечит | Чем заменить дешевле | Вердикт |
|---|---|---|---|
| Секреты в plaintext на 6 машинах | bb #1: credential broker (агент не видит ключ) | SOPS+age + age-ключ в keychain | **Брать паттерн, не инфраструктуру.** SOPS+age. |
| Безлюдные сессии виснут на апрувах (93/101) | bb #1: event-scoped права, действие структурно разрешено/запрещено | PreToolUse-хук + event→allow-set | **Брать паттерн.** Хуки Claude Code. |
| 187 зависших межмашинных задач | bb #2: единый сервер-источник истины + enrolled hosts | — (это и есть замена вашего синка задач) | **Брать bb #2** на 1 узел, сценарий C. |
| 142 непринятых deploy-посылки, 284 без доказательства | bb #2: единый диспетч; bb #1: типизированные service-пакеты | реестр выходов с машинным пруфом (свой, вечер) | Сначала свой реестр (B), потом bb #2 (C). |
| Тихо-зелёные приборы (выход у 17%) | bb #1: результат exec пишется на диск, а не в контекст | свой реестр выходов + проверка артефакта | **Не bb.** Свой реестр. |
| Тесты не краснеют (16 поломок, поймано 5) | ни один bb не лечит напрямую | усилить регресс-сетку + eval-набор из реальных задач | **Не bb.** Своя работа. |
| Ручной путь тикет→PR | bb #1: skill `create-pr`; bb #2: GitHub-плагин | Claude Code GitHub Action | **Брать Action**, сценарий B. |
| Code Mode Sandbox изоляция | bb #1: эфемерная VM | не нужен на личном железе; хватит least-privilege | **Не брать.** Оверинжиниринг для вас. |

## Caveats
- **Числа bb #1 непроверяемы.** «100% coverage», «99% <24hrs», «30–60 мин → одно сообщение» — из маркетингового блога и подкаста, без базы сравнения, выборки и периода. Единственные источники — browserbase.com/blog/internal-agents и insecureagents.com/episodes/derek-meegan. Не переносите их в свои ожидания.
- **Кода bb #1 нет.** Все архитектурные детали — из одного поста и одного подкаста; примеры кода в посте помечены как «Sample/Simplified». Реальная реализация может отличаться.
- **Механизм хранения секретов bb #2 не задокументирован** в первоисточниках (keychain / шифрование / plaintext) — не нашёл; проверяйте `plugins/secrets` в репозитории перед тем, как доверять ему реальные ключи. Не путать с iOS-клиентом bb (там server-connection-креденшл лежит в iOS Keychain — это про мобильный клиент, а не про серверный Secrets-плагин).
- **Точный прод-путь каталога данных bb #2** (`~/.bb`?) в первоисточниках явно не подтверждён; подтверждён только dev-путь `~/.bb-dev/<checkout-instance>/`. Перед настройкой исключений Syncthing проверьте фактический каталог на вашей машине.
- **SWE-bench success-rate завышены контаминацией.** OpenAI сама отозвала Verified как метрику frontier-способностей; лидербордные 80–90% не переносятся на приватные репозитории (при удалении слабых тестов падение до ~18–27%). Стройте собственный eval.
- **bb #2 в активной разработке** (0.42.0 на 05.09.2026), многие API помечены `experimental_`, ломающие изменения вероятны. Сервер bb с неаутентифицированным API, исполняющим команды — не выставляйте наружу, только loopback/Tailscale.
- **Расхождение в счётчиках bb #2** (звёзды/форки) между прямым съёмом и снапшотом энричера — репозиторий растёт; цифры порядка «3k+ звёзд» актуальны на сентябрь 2026.
- Даты релизов и версии bb #2 указаны по changelog getbb.app на дату съёма (сентябрь 2026).

---

## Источники (URL, реально открытые исследователем; 7 шт.)

- https://getbb.app/
- https://open.spotify.com/episode/7Ab7GaiPRXcYblR7XWh35T
- https://www.google.com/s2/favicons?sz=64&domain=getbb.app
- https://github.com/get-bb/bb
- https://www.google.com/s2/favicons?sz=64&domain=github.com
- https://getbb.app/changelog
- https://github.com/ymichael/bb/blob/main/docs/system-overview.md
