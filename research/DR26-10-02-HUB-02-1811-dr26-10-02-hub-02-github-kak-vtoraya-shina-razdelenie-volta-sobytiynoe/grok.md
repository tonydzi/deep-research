# DR26-10-02-HUB-02-1811 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

Идём в исследование как в решение: GitHub-шина либо доказывает себя на узком пилоте, либо её закрываем и оставляем брокер. Сначала подниму ваш прошлый вердикт по rail #5 и живые источники 2025–2026, потом сверю сценарии S1–S5.The research query splits into a few different evidence targets. I’ll check that those targets are actually distinct before writing the questions.The query separates into six evidence targets: git-as-bus design, GitHub limits, vault split, session wake, bus comparison, and misuse risks. Those are not paraphrases of one question.

```json
{"questions":["What envelope, branch, and folder conventions do documented git-as-bus, GitOps, git-queue, and CRDT-over-git systems (git-bug, Fossil, Dolt, JuiceFS, DVC, Radicle, Nostr, git-annex, etckeeper, chezmoi, plus Flux or Argo CD critiques) actually use, and how does each handle two peers pushing at once?","What do current GitHub docs state for git push and REST or GraphQL rate limits, repository and Git LFS size limits, and the privacy model of a private repository if it is used as a high-frequency machine-to-machine channel?","For an Obsidian vault of roughly 150-180k files, what do official git and Obsidian sources say about a small git core plus a store kept outside git, including selection criteria, one-way replication, monorepo versus split repos, Git LFS, partial clone, sparse checkout, and scalar?","How can a GitHub event wake an already-running local coding-agent session: webhook to a self-hosted receiver, polling with backoff, or GitHub Actions as an alarm; how is that event delivered through hooks, a file inbox, or MCP; and which documented anti-patterns cover wake storms and agent-wakes-agent loops?","On ordering, delivery acknowledgement, offline operation, cost, and human observability, how do official docs compare a git repository bus with Telegram, a Syncthing folder, MQTT, NATS or JetStream, GitHub Issues or Discussions, gists, and one repo per peer?","What primary sources document the risks of using GitHub as a message bus: secrets retained in git history, GitHub Terms of Service and automation-abuse rules, GitHub as a single point of failure, and commit noise or history bloat?"]}
```I'll check current GitHubI'll primary docs look for push up primary/API docs rate limits for, repo how and Git L thoseFS size limits git,-backed systems name branches and what, folders, and a envelopes private repository, and what actually hides they. do when two peers push at once.I'll look for primary docs on waking a running local coding agent from aI'll look for GitHub event, both primary sources on those in four Git thisHub-as- workspace and inmessage upstream-bus risks: sources. retained secrets, ToS and automation rules, single point of failure, and history bloat.I'll check official Git and Obsidian docs, plus any local vault notes, on a small git core with an external store.I'll look for primary docs that compare a git-repository bus with those other transports on ordering, acknowledgement, offline use, cost, and human observability.Primary GitHub and Git documents cover retained secrets, automation limits, a named internal single point of failure, and history or push-volume limits. None of those pages uses the phrase “message bus.”

```json
{
  "claims": [
    {
      "claim": "GitHub documents that sensitive data such as a password, token, or credential committed to a repository can remain reachable after a local history rewrite and force-push, including in clones, forks, cached views addressed by SHA-1, and pull requests that reference those commits.",
      "confidence": "high",
      "evidence": "GitHub Docs states that if the data to remove is a secret, the first step is to revoke or rotate it, and that rewriting history still leaves commits accessible \"in any clones or forks of your repository,\" \"directly via their SHA-1 hashes in cached views on GitHub,\" and \"through any pull requests that reference them.\" It also says GitHub cannot remove the data from other users' clones and that a commit present in any fork \"will continue to be accessible there.\"",
      "source_locator": "https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository",
      "source_title": "Removing sensitive data from a repository",
      "source_type": "primary"
    },
    {
      "claim": "GitHub's Acceptable Use Policies prohibit automated excessive bulk activity and using GitHub servers to place an undue burden through automation, and they reserve suspension, throttling, or repository deletion for excessive bandwidth or infrastructure strain.",
      "confidence": "high",
      "evidence": "Section 4 forbids \"automated excessive bulk activity\" and \"using our servers for any form of excessive automated bulk activity, to place undue burden on our servers through automated means.\" Section 9 says that if bandwidth use is \"significantly excessive,\" GitHub may \"suspend your Account, throttle your file hosting, or otherwise limit your activity,\" and, after notice, \"delete repositories\" that place \"undue strain on our infrastructure.\" The policy states that users must comply with these Acceptable Use Policies while using the Service.",
      "source_locator": "https://docs.github.com/en/site-policy/acceptable-use-policies/github-acceptable-use-policies",
      "source_title": "GitHub Acceptable Use Policies",
      "source_type": "primary"
    },
    {
      "claim": "GitHub's REST API rules cap automated content creation and say continued requests while rate-limited may get an integration banned.",
      "confidence": "high",
      "evidence": "The rate-limit documentation says primary limits \"help prevent abuse and denial-of-service attacks.\" Secondary limits include: \"Create too much content on GitHub in a short amount of time. In general, no more than 80 content-generating requests per minute and no more than 500 content-generating requests per hour are allowed,\" covering the web UI as well as REST and GraphQL. It also states: \"Continuing to make requests while you are rate limited may result in the banning of your integration.\"",
      "source_locator": "https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api",
      "source_title": "Rate limits for the REST API",
      "source_type": "primary"
    },
    {
      "claim": "GitHub's own 13 September 2026 incident report says a shared database cluster read on nearly every authenticated request was a single point of failure that degraded about 28 services.",
      "confidence": "high",
      "evidence": "The resolved report says that between 08:43 and 10:44 UTC on 13 September 2026, availability was degraded \"across approximately 28 services, including Issues, Pull Requests, Actions, Codespaces, Pages, Notifications, Code Scanning, Git LFS, and new account signup,\" because a cleanup job wrote to a cluster that \"stores permission data read on nearly every authenticated request.\" The remediation sentence is: \"In addition, we are breaking apart this database cluster to remove the single point of failure.\"",
      "source_locator": "https://www.githubstatus.com/incidents/0rn90wk115q9",
      "source_title": "Incident with several GitHub Services",
      "source_type": "primary"
    },
    {
      "claim": "GitHub treats repository sizeSix systems document a concrete ref and commit, folder frequency as health, or event factors, recommends envelope and keeping repositories ideally under 1 GB a rule and strongly for two under writers 5 GB., and says The Git is not designed as others a backup store in.",
Идём верно в разрезе «маленькое ядро отдельно      the list do "confidence от склада": " not,high",
     ». И "evidence in the pages checked": "\"дём, defineRepository health неверно, если ту is a function of various interacting же боль a git concurrent factors, including-push protocol.

 size, ле commit frequency,``` contentsчить третьей, and structure.\" \"json
{"claimsWe recommend тру":[{"claim": repositories remain small,"git-bug ideallyбой less than из 1 stores each entity at GB, and less than git- refs/< 5 GB isnamespace>/<entity- stronglyкоммитов. Лаг recommended.\"id> (remote If в a repository \"excess 12ively часов снима copies impacts at refs/rem our infrastructure,\"ется слуша Supportotes/<remote>/< may ask forтелем на correctivenamespace>/<entity- action. The уже существующей sameid>). Each commit page states: шине. tree \"Git is not designed Но to holds serve as version-< a backupвая tool. труN>, an ops JSON blob,ба этот and ла\" Lamport clocks",г не ле
      as empty-blob "чит.

source_ nameslocator. If":🎙 Май "https two://docs. tips diverge, theк: каgithub.comнон и до/ client makesen/repositories a merge commit whose/ска «ктоworking-with ops array что-files/ is empty andmanaging дер-large-files orders operationsжит» до/ by editabout-large-clock,-filesстой-onны then pack ID. It git. Ра only accepts fastция до-githubстойна",-forward pushes.
      "","confidence":"source брокера или уже жи_titlehigh","evidence":": "вогоAbout large files on Telegram. С"The DAG GitHub",
 entity spec lists those     ме " twosource_type":шать их в одном ref patterns, requires "primary"
 репозитории значит    },
 merge    через {
 commits to carry zero месяц пере operations, and says      "claim": "GitHub scenario'sстать repository limits warn that 5 ( frequent читать историю file.

Ниneither tip an changes ancestor) creates mergeже вер and commit M with an a high pushдикт по источ rate bloatникам, которые я maintenance cost empty operation array. and can It be thrott открыл led also says git202: the recommended6-10-02. В push-bug only accepts cap is fast-forward pushes ну6 pushes perтреннее, so minute a merged, state cannot and the ию recommended be overwritten."," on-льскоеdisksource_locator": .git size cap"https://raw is исследование из 10 GB брифа я.",.githubusercontent.com/
      "confidence": не переgit-bug/ "high",
     spec/trunk/проверял "evidencedag-entity.md": " по\"Directories","source_title containing numerous фай":"git-лу волbug DAG frequently modified files can significantly increase repository maintenance costs andта и entity format specification"," degrade the performance ofsource_type": basic счита Git operations."primary"},{"\"ю его Activity guidance saysclaim":"F вашей постановкойossil treats an accidental second, не внешней child of the same check-in as \" улиPushкой.

## 1. Верди rate a fork. With: The autosync onкт по recommended maximum, the limit is  S1–S5

| С later6 pushesценарий | Живу per commit is aborted minute perчесть with \" repository.\" |would fork\" so Size guidance Главный риск | Цена вла the second says \"Onдения |  user can fossil-disk size: 10 GB,\" defined1 месяц | 6 месяцев |
| update and merge. as the .git folder, and---|---|---|---|---|---|
| that Offline large repositories \"can slow down fetch operations, **S1. and increase clone times.\"",
 Статус     - " autosync off,source_locator": or commit "кво +https://docs.github.com/ --allow-forken/ будиrepositories still/creating-and-льникmanaging-repositories records/repository-limits",
 both на всех пирах** | Высо children.кая. Это After чиwardнит, the leaf наз      with the newer "sourceванную боль_ check-in timetitle":. | Если is chosen "Repository limits as the parent of файл", later
      до " worksource пи_, buttype":ра е " only if that leafprimary"
дет ча    has been synced into }
сами, бу  the local ],
 clone.","confidence  "unc":"high","ertainties": [
evidence":"Fдильник спи    "ossil'sNone of branching document the inspected says Alice's primary pages check namesт вместе с ним or. | Ноль новых серви evaluates-in 3 aсов. Один маленький makes Bob's commit 'Git слушаHub as abort aтель на пир with \"would fork message bus'\",. | Пи design; whileр онлайн applying these limits to an that ви off architectureдит входя-network or -- is anallow-fork commit inference.",щее за
    минуты, не leaves " two leavesThe. It за пол 13 then says the September дня. | Жи parent2026в report describes of a later oneёт, пока shared check-in is database слушатель не шлёт новое cluster as chosen an by leaf сообщение в internal ответ single check-in time point of на про failure., and only if Itбуждение. |
 does not say that that check-in GitHub| **S2. В.com as has been synced down a whole is. a","source_ singleключить ` point oflocator":"https failuregit_bus.py` треть for every external user://fossil-.",им рельсом** | Тех
    "scm.org/homeAcceptable Use section/doc/trunk нически4 does/www/branch not give a вклю numeric threshold for commiting.wiki","-чится. Как or issue-basedsource_title": ра messaging,"Fossil: so whetherция a Branching, Fork particular bus is ' не доexcessiveing, Merging' isтяги not defined, and Tagging on thatвает page","source_type.",
    "":"primary"},. | ГThe github/онкаgit{"claim":"-s вершиgit-annex keepsны ветки, се repositoryizerкрет в истории, ш and README object-ум коммитов. | В, whichремя людей Gitlocation state onHub's large-files page points to, separately says many slightly на rebase a separate git. Д-annex branch, unrelatedеньги GitHub при  to master2–, and journals5 пуша pendingх в день ну changes in .gitлевые/annex/journal changed versions of large text files are expensive to reconstruct and diff and advises against storing logs in Git, but that README was not used as one. | Пер/ before committing them throughвые heartbeat- .git/annexфай/index. Filesлы кажды on that branch areе N built to auto минут у-merge by concatenпиating lines, andраются each line carries a в ли timestamp so the newest informationмит пушей и хоро can of the six claims."
  ]
}
```нят be identified."," журналconfidence":"high. | К","evidence":"то-The internalsнибудь сдела page says .git/annex/ет force-push «journal/ journals changesчтобы по to the git-чинитьannex branch before a» и со commit, thatтрёт чу the branch is not connected to master,жо and that itsе доказа files \"тельство. Ваare allш же скрипт это прямо designed to be auto-merged by simply запрещает. |
| **S3. Ядро- concacenating them together\"репо и with в a нём папка `messages/`** timestamp on each line | Я.","source_дро жиlocator":"httpsвое://git-annex. Папка сообще.branchable.com/internals/ний я","source_titleдро по":"git-ртиannex internals","sourceт. | Д_type":"ва писателя одногоprimary"},{"claim файла:":"Radicle vault puts each peer's collaborative и git. Плю objectс ши under thatна внутри peer's git namespace ка, refs/namespaces/<remoteнона. | Н>/refs/cедели на списокobs/<typename>/< допускаobject_id>, and likewise. stores По branches asтом д refs/namespaces/<ёшево, если пишетremote>/refs/ только хаheads/<branch>б. | Ка and signed refs asнон наконец refs/namespaces имеет/<remote>/refs diff. Со/rad/sigобщения начинаютrefs. Draft COB refs under спо refs/draftsрить с ка/ are not replicatedноном за or signed. Two внимание peers therefore update. | Ли different refsбо rather than one shared ядро оста refётся чита.","confidence":емым"high","evidence, либо пре":"radвращается вicle 0.25.1 git ленту.rs documents cob() as refs/ коммитов,namespaces/<remote>/ иrefs/cobs люди пере/<typename>/<object_id>, branchста_of()ют его as refs/namespaces открывать. |
| **S4/<. Gitremote>/refs/heads/<branchHub Issues как шина** | Го>, sigдноrefs() as refs как/namespaces/<remote доска>/refs/rad/sigrefs, and says draft, которую чита references \"are not replicated orет человек signed.\"",". | Веsource_locator":б"https://docsхук не гаранти.rs/radicle/latest/srcрует порядок/radicle/ и не догоgit.rs.html","няет самsource_title":. О"radicle ф0.25.лайн нет1 git.rs",". | Беsource_type":спла"repository"},{"тно на маclaim":"NломIP-34 announces объёме a git. Л repoимиты as No создания конstr kind 30617 with a dтента: tag (usually a 80 запросов в минуту и kebab-case repo id) plus 500 в час. | On clone andAir и relays tags, and прось announces branchбы о помощи удоб and tag tipsно искать as kind 306. Ма18 with repeatedшин tags named refs/heads/<ный ACK неудобенname> or refs. | В/tags/<name> whoseторая value is a CRM из commit id. Patches are kind  закры1617 andтых issue their. В content is git ава format-patch outputрии и он. When severalбординге упи status events exist, the newest by created_at from the issue/patch author or a maintainer is the valid status.","confidence":"high","evidence":"NIP-34 defines kind 30617 with [\"d\", \"<repo-id>раетесь в лимит создания. |
| **S5. NATS или MQTT на VPS, GitHub только для ядра** | Самая чистая архитектура на полгода. | Новый\"], kind  процесс30618 with [, который кто\"refs/<-heads|tags>/<то должен поднимаbranch-or-ть. Яtag-name>\", \"<commitкорь умер-id>,\"], and kind и 1617 content as \"<patch ф>\" fromлот нем git, если Telegram вы format-patch.ключен. | Со It statesфт that the most recent status event by бесплатный created_at from. С the authorчёт за or a maintainer VPS у is considered вас уже есть. Опера valid.","source_locator":"цииhttps://github на.com/nostr вас. | Н-protocol/nедеips/blob/master/34.mdля","source_title на под":"NIPнять-34"," и не слоsource_type":"primary"},{"мать Tailclaim":"Dscale. ПольVC keepsза Git ви-дна толькоtracked . после заdvc pointerмера S files in1. | С the repo andку storesчный и правильный кон the data itselfтур, outside если Telegram оста Git. Forётся вто a trackedрым рель directory, a configuredсом, merge driver runs а не вы dvc git-ключаетсяhook merge-driver. |

## 2. Рекомен and combinesдация

 bothБ branches. If the sameрать file was added or ги changed onбрид. both sides, the На driver fails unless those changes are identicalзвать. его так A plain: **S1 сейчас file, узкое одно .dvc conflict is resolved by pickingсторон one hashнее ядро без or па by checkingпки сообще out both versions and runningний, S dvc add again5 только если за.","confidence":мер у"high","evidenceбь":"DVCёт S's merge-conflict1.** guide

У showsверен .gность: [itattributes \"*.dvc mergeemerging]. О=dvc\"пора and the driver command: d двеvc git-hookнадцать merge-driver. It says a часов directory merge \"will, simply combine data from которые both branches\" вы and fails when описа the same file was added or changed inли both branches unless the, сов changes are the sameпадают с робо.","source_том инlocator":"httpsбокса «://doc.dvc.org/userдва раза в сутки»,-guide/how а не с-to/resolve отсутствием GitHub. Это-merge-conflicts гипотеза.","source_title Е":"How toё Resolve Merge Conflicts in уби DVC","source_type":"вает или подтверprimary"}],"uncждает перваяertainties":["JuiceFS documentation не describes object-storageделя пилота.

По prefixesчему так chunks/ and meta.

/Про plus a databaseбу metadata engine, notждение и транспорт a git ref — разные работы or commit. Git envelope, so no twoHub в-peer git push середине rule was found. пути","etckeeper's «ве README treatsбхук бу /etc as anдит хаб, хаб будит ordinary git repo and пира warns against checking out» повто another machine's branchряет ныне.шний It does not define doorbell и a special concurrent-push envelope. добавляет точку отказа","Flux's repository-structure. Пир guide specifies folders за such NAT веб as clustersхук Git/,Hub сам apps/, не при and infrastructure/ on one branchмет. Слу, with short-шаlived branches, butтель на маши does not specify whatне пира, который happens if смо two peers push that branch at onceтрит уже при.","chezmoiеха documents a source directoryвший and a three файл `_-way destinationmachine-bus` или уже/source/target приеха merge duringвшее apply. That сообщение is not a git Telegram, бу ref race betweenдит се two pushingссию без ха peers.","Dба.

Яolt's cell-дро иlevel merge and рация тоже transaction-rollback разные работы. Это behavior appeared only in search snippets. не The docs URL мой redirected and вкус was not fully, это fetched рас, so itхождение двух is omitted.","No жи primary gitвых школ-queue or stacked,-patch specification и обе was fetched пра that statesвы на a сво simultaneous-push rule."ём объекте.

*]}
``` Flux: git хранит желаемое состояние. Контроллер тянет сам. Вебхук только сдвигает момент тяги. Доставка вебхука не гарантирована, поэтому медленный интервал оставляют. Нижнюю границу интервала авторы Flux советуют держать выше 30 секунд, а щель закрывать приёмником, не частым опросом. [Flux webhook receivers, страница от 2025-10-03](https://fluxcd.io/flux/guides/webhook-receivers/); [Flux end-to-end, страница от 2026-07-02](https://fluxcd.io/flux/flux-e2e/). [established]
* git-bug и эксперимент Anthropic: git годится как носитель совместных записей, если хранить операции или чужие файлы не трогать, а отказ push считать замком. Nicholas Carlini, 16 агентов, февраль 2026: замок — файл в `current_tasks/`, второй push не проходит, конфликты слияния были частыми, репозиторий был локальный bare, не GitHub.com, отдельного канала сообщений он не строил. Когда все упёрлись в один и тот же баг ядра Linux, агенты затирали правки друг друга, пока задачу не разрезали. [Anthropic Engineering, 2026-02-05](https://www.anthropic.com/engineering/building-c-compiler). [established] для этого эксперимента, [emerging] как общий рецепт флота.
* Ваш `git_bus.py` уже стоит на правильной половине этого рецепта: узел пишет только `state/<своё имя>/`, один `pull --rebase`, force-push запрещён, потому что сотрёт чужое доказательство. Это транспорт посылок выката, не чат сессий. Файл: `public-repos/fleet-deploy/transports/git_bus.py`.

Отсюда шаги.

1. Неделя замера, без новой трубы. Двадцать сообщений. Время от отправки до появления на пире. Отдельно: оба узла онлайн, и отдельно: пир был выключен. Счёт конфликтов Syncthing за те же дни.
2. Если при двух онлайн-узлах файл есть через минуты, а сессия виCurrent GitHub docs separate API quotas from git push guidanceдит его через ча, capсы, ordinary ставить слушатель на Git blobs существующий well инбокс below Git LFS, and treat a. Слу private repository as accessша controlтель имеет plus confidential handling право раз, not as a hiddenбудить сессию. Слу high-frequency pipe.

```json
{"шатель не имеет права писать вclaims":[{" шину.
3. Параclaim":"GitHub's currentллельно, только с REST docs set хаба, одно a primary limit of 60 requests perсторонний сни hour for unauthenticated callsмок я andдра в 5,000 requests per hour for новый authenticated users, raised приватный репозиторий to 15,000 per hour when a GitHub Enterprise Cloud organization. Список допуска-owned GitHub App,, or an O соAuth app owned or approved by suchтни фай an organization, calls on a member'sлов, не сто behalf. The пять builtдесят-in Actions тысяч. Пиры GITHUB_TOKEN is limited к to 1,000 requests per hourло per repository, or 15,000ни per hour perруют только repository for Git наHub Enterprise Cloud resources. Git LFS чтение. В upload and download API vault calls use a separate bucket of те 300 requests per minute unauthenticated and же файлы этим 3,000 per minute authenticated. репозитори","confidence":"highем не редаA GitHub event can reach a live local coding","evidence":"Theктируются. On session by REST rateAir кла a self-hosted webhook-limit page states: thatдётся как ` \"The primary rate limit for unauthenticated appends a fileonair/<машина requests is 60 requests per hour,\" queue, by>.json`, и and injecting a эту authenticated turn into an existing thread стро requests \"count towards your personal rate limitку пишет только сама, or by a GitHub Actions job of 5,000 requests per hour posting to машина. Это,\" with  желаемое a local inbox15,000 per hour for the состояние, не лен the agent drains Gitта сообще.Hub Enterprise Cloud appний.
4. Па No source I inspected names both cases named above. It also statesпку `messages/` не создавать the G “wake storms” and “agent-. `git_bus.py` неITHUB_TOKEN limitwakes-agent loops” as one включать как is \" catalog тре1,000 requests per hour per repository.

```json
{
  "тий рельс\" andclaims": [
    {
      "claim чата.
5 \".15 S,5000 откры requests": "IDE Agent Kit documents per hour per repository\" for Gitвать только если замер a self-hosted GitHub Enterprise Cloud resources, and that показал Git LFS requests useHub webhook другое receiver that \"a separate rate: оба limiting bucket with a limit of 300 узла verifies HMAC, requests per minute for unauthenticated requests and онлайн, а файл `_ normal 3,000 requests per minute formachine-bus` всё равно еizes the authenticated requests.\"","source_locator event, appendдет часами, либо сообщения":"https://docs.github.com теs it to a local JSONL queue/en/rest/using-the, and canря-rest-api/rate-limitsются. Тогда nudge-for-the-rest-api JetStream на яко a tmux session.",","source_title":"Rate limitsре
      "confidence": "high", for the REST API","source_type, Telegram
      "evidence": "The README":"primary"},{"claim":" остаётся.The GraphQL API uses's Git a separate point primary path is \" budgetHub поGitHub event → webhook server → normalized,-прежнему только JSONL queue → IDE agent reads queue я not the REST request count → acts → receipt.\" The: дро.

До GitHub Webhooks section says5,000 points per hour per user и после на ва, or src/webhook-server.mjs receives events, verifies HMAC signatures, normalizes 10,000 points per hour whenших числа a GitHub Enterprise Cloud organization them, appendх.-owned GitHub App or as to a local JSONL queue, С qualifying Oейчас общее and optionally nudges a tmux sessionAuth app acts дерево; serve for около that listens 150–180 тысяч файлов го user. Git on aняется Syn configuredHub App installations on Gitcthing целиHub Enterprise Cloud get port suchком, одна 10,000 points per hour; as  волна да other installations start8787 atла 959 конфликт at 5,000 and can scaleных /webhook.",
      "source_ tolocator": "https at копи most 12,500. Actionsй, пир://github.com/ThinkOffApp/ GITHUB_TOKEN is 1,ide-agent-kit/blob/ видит входящее с000 points per hour per repository, ormain/README.md",
      "  15,000 forsource_title": "IDE Agent Kitлагом до половины enterprise суток README",
      "source_type-account resources on GitHub.com": "primary",
      "source, а."," гото_type_confidence":"high","evidence":"note": "projectвый railThe GraphQL rate README"
    },
    {
     -limit page says the REST #5 после "claim": "On API \" одного сообщения молhas a separate primary rateчит. После этого плана that same kit limit\", a склад and then states остаётся скла notification:дом и не \"For users: file alone по 5,000 points per hour per does not wake the agent,падает в git. Не user,\" with \" and two simultaneousсколько соa wake paths will higher rate limit of 10,000тен answer the points per hour\" for the фай same mention twice unless Gitлов канона, one owner isHub Enterprise Cloud GitHub App реестров и OnAir имеют and OAuth cases. declared.",
 один It      "confidence": "high",
 also diff      "evidence": "The states, README non который пишет ха says nudge_mode \"-Enterprise Cloudб. Сеnone\" is installations are \"ссия на вклю queue-only with5,000 points per hour\" andчённом пире про no nudge, \"cannot increase and the beyond 12,500 points per hourсыпа Grok section says that mode only writes the notify file so the agent looks dead until a human types,\" Enterprise Cloud. installations are \"10,ется от000 points per hour per installation,\" and GITHUB_TOKEN is \"1, уже приеха000 points per hour per repository\" or \"15,000 points per hour perвшего конвер Footgun 2: a repository\" for enterprise-account resources.","source_locator":та. Треть"https://docs.github.com/ webhook receiver and the poller nudge,en/graphql/overview/rate-ей or the app's own scheduledlimits-and-query-limits- истории check, answer every mention twice; poll коммитов неfor-the-graphql-api","er.wake_path must besource_title":"Rate limits and появляется.

## 3 one query limits for the GraphQL API",". От of nudge, webhook, automation, orsource_type":"primary"},{"веты на noneclaim,": and"Beyond the poller refuses to nudge unless it primary A–E

### A. Git как шина

Рабо quotas, GitHub enforces secondary is nudge.",
      "source_чиеlocator": "https://github.com/ limits sharedThinkOffApp/ide-agent- by REST образ and GraphQL: no more than цы естьkit/blob/main/README.md100 concurrent requests, no.",
      "source_title": more than 900 points per minute on Они не похо "IDE Agent Kit README",
      aжи "source_type": "primary"
 REST endpoint and 2,000 points на «один    },
    {
      "claim per minute on the GraphQL endpoint, `": "WakeWire delivers and generally no more than 80 contentmain`, каж a GitHub webhook as a new turn-generating requests per minute and 500дое сообщение — коммит в in per hour across общий the web UI, REST, an already and GraphQL. These secondary limits can файл».

* **Ж chosen change without notice, localелаемое состояние.** and a Flux и Codex thread, and it secondary limit Argo тя ded mayupes, serializes,нут git по also интервалу backs be applied. off, and coalesces bursts instead for undisclosed reasons of letting Вебхук не.","confidence":"high","evidence пре the agent":"The REST rate poll.",
      "confidence": "вращает их-limit page says в push-: \"No more than 100 concurrenthigh",
      "evidence": "The README says a requests are allowed. This limit is sharedсистему webhook fires and. across the REST API and GraphQL API,\" seconds Он бу \"No more than 900 points per later a new turn appears in the threadдит тот же pull minute are allowed for REST API endpoints, you chose, explicitly. [established and no more than 2,000]
 as points per minute are allowed for the Graph* **ЖQL API endpoint,\" and \"no anурнал операций внутри alternative to agents polling on a timer. more than 80 content-generating requests git.** git-bug хранит сущность per minute and no more than 500 The default sink is codex-app как це content-generating requests per hour are allowed-server using,\"почку thread/resume and turn/start, including web право and,к, а не как REST, and GraphQL actions. It it backs текущий сни off when a turn is already in flight addsмок. Пара. Deliveries are deduplicated by: \"These secondary rate limits are subjectллельные правки с delivery to change without notice. You may alsoходятся id, serialized so encounter a secondary rate limit for undisclosed доба wake reasons.\"","source_locator":влением mergewire never has two turns in flight on"https://docs.github.com/ вen/rest/using-the- one thread, retried with capped backoffrest-api/rate-limits- свой when Codex is unreachable, and coalesced DAG, без конфликтаfor-the-rest-api"," into one digest if a содержиsource_title":"Rate limits for route exceeds its defaultмого на the REST API","source_type": "primary"},{"claim":"For уровне10- git traffic файлаper-minute limit.",
      ", Git. [диsource_locator": "https://githubHub's repositoryзайн--limits page recommends at.com/glenncalleja/wakewireдок/blob/HEAD most 6 pushes per minute and trunk 15 git/README.md",
      "source, даты read operations per second per repository,_title": "wakewire публикации нет says README",
      "source_type, смо large read": "primary"
    },
   трел 2026-10- volume can throttle {
      "claim": "Claw02](https://github.com/ agit-bug/git-bug/Code documents Git repository, and says automated CIblob/trunk/doc/design/Hub Actions as an alarm that,data-model.md). [single- POSTs to a local webhook, with machine users, or third-party appssource], the running can degrade performance. It states это специ agent draining that push size is enforced at 2фикация самого GB and that проекта, queue through an that a single Git не MCP inbox object is recommended независи tool on its to next idle turn.",
      "confidenceмый отчёт.
* **Замок stay at or": "high",
      "evidence через below": "docs/webhooks.md shows отказ 1 MB and push External.** Carl is enforced at 100 MB. The system →ini, ци recommended on-disk maximum POST /v1/webhook → queueтата по for смы the . (up to 1000) → agentgit directoryслу: два аген reads via MCP. The CI/ is 10 GB.","confidence":та пи"high","evidence":"Thatшут один page saysCD example is и a GitHub Actions step: \"Push rate тот же lock- that curls http: The recommended maximum limit is 6файл, git за pushes per minute per repository,\"ставляет \"Git read operations (e.g://your-host второго:18790/v1/webhook. The agent reads. fetches, clones): The recommended maximum взять events limit is 15 operations per second per другую with chat_inbox_read, and repository,\" and задачу. Кон \"Automated processes such as CI, the pageфликты при machine users, or third-party applications says a, can degrade a repository's performance in этом ча 202 meansстые some cases.\" It also the event is queued and processed. С says \"Push size: This limit is when the agent is idle. The queueреда enforced at 2GB,\" \"Single is in-memory and lost if — локальный bare и object size: The recommended maximum limit is Docker Claude Code restarts.",
      " 1MB. This is enforced at, не APIsource_locator": "https://github 100 MB,\" and \"On- GitHub. [202.com/crisandrews/Cldisk size: 10 GB,\"6-02-05]( meaningawCode/blob/main/docshttps://www.anthropic.com/ the .git folder.","source_/webhooks.md",
      "locator":"https://docs.githubengineering/building-c-compiler).source_title": "ClawCode
* **Раз.com/en/repositories/creating- docsведениеand-managing-repositories/repository-/webhooks.md",
      "limits","source_title":"Repository писаsource_type": "primary"
    limits","source_type":"primaryтелей по"},{"claim":"Git LFS },
    {
      "claim": папкам.** Ваш ` "claude maximum filegit_bus.py` и схема size depends-code-tools' `fleet-deploy`: on plan github-wake skill wakes the current Codex ха:  or Claude Code session on the first futureб пишет `parcels/2 GB on GitHub Free and comment of`, узел пишет только свой GitHub Pro, 4 GB on an existing issue `state/`. Со GitHub Team, and 5 GBдержи, and it on GitHub Enterprise Cloud. Gitмое фай forHub saysлов не сталкиbids that aвается session from polling GitHub itself.",
 file that. Ста      "confidence": "high",
 exceeds the лки      "evidence": "SK5 GB per-file limit is rejectedваетсяILL.md says to вер by Git LFS with run an error.","confidence":"highшина ветки. Это github-wake ISSUE_URL only","evidence":"The Git L леFS documentationчи when the issue exists and progress depends on a future comment.тся одним table Codex lists maximum rebase и ло must have file size as GitHub Free  been launched through codex-dynamic.2 GB, GitHub Pro 2мается force On-push.

Гра GB, GitHub Team 4 GB Claude Code 2.1.98, and GitHub Enterprise Cloud 5бли, которые уже GB, and states or later the записа Monitor: \"If you exceed the per-ны уfile limit of 5 GB, the GitHub.

 tool must own github-wake --claude file will be rejected by Git LFS* Пу-monitor so its singleш, with an error message.\"","source_locator":"https://docs. который не fastgithub.com/en/repositories/working output line wakes that-forward, отвер-with-files/managing-largeгается. Второй-files/about-git-large conversation. The skill says not to poll GitHub yourself or-file-storage","source_title из":"About Git Large File Storage"," launch a watcher; двухsource_type":"primary"},{" одновремен the command stores the watch andclaim":"Onных пу reшейuses one shared watcher, про GitHub.com, a private repository is and the an comment body is untrusted data.",
игры access-      "source_locator": "httpsвает. Этоcontrol setting://github свой, not a statementство.com/pchalasani/claude that GitHub cannot see the-code-tools/blob/main git, не на bytes/plugins/dynamic-workflow/skills: itстройка./github-wake/SKILL.md Ста is accessible only to the owner",
      "source_title":, people givenрое объяс "github-wake SKILL.md", explicit access, and,нение, механизм
      "source_type": " for organization repositories, certain organization members, тот while organization owners always have access toprimary"
    },
    {
      же: [Stack "claim": "Document every repository in the Overflow, 2011-01-ed loop break organization. GitHub's10](https://stackoverflow.com/ers for Terms sayquestions/4643998/how- GitHub- private-git-works-when-two-triggered agents are botpeersrepository contents- arepush confidential-changes-to- non, but personnelsame-remote-simultaneously). М may stillеханизм [-re access them without consent fortrigger and aestablished], security, automated scanning or manual review for cap on assigning страница known vulnerabilities, active malware, or other further старая Terms-violating content, support.
 agents, not a* text Рекомендуемый, service integrity, or legal obligations. named пото","confidence":"high","evidence": wake storm"About repositories states:лок пушей:  or agent-wakes-agent.",6 в минуту на репозито \"Private
      "confidence": "high", repositories are only accessible to you, peopleрий. Ч
      "evidence": "anthrop you explicitly share access with, and,теics/claude-code-action prepareний git: 15 в for organization repositories, certain organization members,\"AgentMode calls checkHumanActor with секунду and \"Organization owners always have access to every repository created in an organization.\". Раз the comment that Terms it prevents bot-triggered loops,мер одного пуша принуди and PR # section E says GitHub \"тельно ре826 saysconsiders the contents of private repositories toжется на automation mode rejects bot-triggered workflows be confidential to 2 ГБ. Один so объект: предупре you,\" while Claudeждение по section pushing code E.3 says personnel will not access смы that does not fireслу ли content without consent except \" synchronize and run Claudeмитовfor security purposes,\" \" на again unlessfor automated scanning or manual review for known  the50 vulnerabilities, active malware, or other content Ми known to violate our Terms of Service,\"Б, блок bot is in allowed_bots. GitHub Agentic Workflows' support rate-limiting page says the на 100 МиБ., \" github-actions[bot] account does Шиto maintain the integrity of the Service,\" not trigger workflow events, so a safeрина одного or legal-output issue or comment каталога: 3000 obligations.","source_locator":" им willhttps://docs.github.com/en not retrigger workflows, and assign-/siteён. Ветto-agent defaults to max 1-policy/github-terms/githubки: because-terms-of-service","source 5000. On without a_title":"-disk `.git`: ориентиGitHub Terms of Service, limit one workflow could spawn three agents thatр 10 ГБ. [Repository section E, each spawn three more.",
      " limits, жи readsource_locator": "https://githubвая страница, сня with About repositories","source_type":та 2026-10-02.github.com/gh-aw/"primary"}],"uncertainties":reference/rate-limiting-controls/",](https://docs.github.com["The user-access
      "source_title": "/en/repositories/creating-and sentence in claim-managing-repositories/repository-limitsGitHub Agentic Workflows rate- 6 is from https). [limiting controls; anthrop://docs.github.com/en/establishedics/claude-code-action agentrepositories/creating-and-managing-]
* REST mode andrepositories/about-repositories; the confidentiality для PR #826",
      "source во and staff_type": "primary"
    }
-access exceptions are from Termsшед  ],
  "uncertainties sectionшего E. One": [
    "No page пользователя: 500 inspected claim therefore0 запросов в час. В uses the exact anti- reliespattern titlesтори on two inspected primary \"чный pages. преwake storms\" and \"agent-wдел: неakes-agent loops\" as a больше","Acceptable Use Policies sections paired 4 and 9, 100 одновремен catalog for local codingных запросов, и по at-agent wakes.",
    "No созданию кон primary sourceтента вообще https://docs.github.com/en/site-policy/acceptable-use found не больше 80 в минуту и-policies/github-acceptable-use 500 в час. [Rate limits that-policies, prohibit excessive for the REST API] polls the GitHub API with backoff in( automated bulkhttps activity:// anddocs.github.com/ using order to wake an already-running local serversrest/overview/rate-limits session. github-wake forbids the so-for-the-rest-api session from polling and does as). [established]
 not document a to place an* П backoff schedule.", undue burden, and reserve suspensionлоский
    "ClawCode's webhook, file каталог `messages/` с де queue-hosting throttсятком is drained тысяч файлов уling, or deletion after notice on the next idle turn and for significantly excessive bandwidth. Theyпирается в лимит is not ширины каталога раньше do not publish shown a separate numeric rule, чем в ли to interrupt a busy turn.",мит ги that a
    "The local workspace and Eгабайт private repository may:\\Obsidian\\. Ре notAnton-Knowledge had no matching designж be used as a machine-to-machine channel.","https note for thisьте wake path."
  ]
 по://docs.github.com/en/repositories/working-with-files/ маши}
```managing-large-files/about-не и по месяlarge-files-on-github warnsцу, above 50 MiB, blocks если когда files larger than 100 MiB,- limitsнибудь всё же browser uploads to 25 MiB, recommends repositories ideally under 1 GB and по strongly under 5 GB, and saysй Git is not designed asдё a backup tool. That softте в size guidance is stricter than the repository- файлlimits page's recommended 10 GB on--diskна maximum-сооб, and thatщение.

Juice page says theFS я object cap в is  этот за100 MB rather than ход пер100 MiB.","The 6вич-pushes-per-minute andным доку 15-readsментом не подтверждал-per-second figures are labeled recommended maximums. Не ста.Official Git documents aвь small tracked Theте core beside same page says push size and его в data kept out of the normal single-object size are enforced. ряд «git- No inspected objectшин set page states a hard git». D.-push requests-olt Obs тожеidian’s ownper-minute quota не comparable ци help does not describe that Git to the REST hourly layout, and neitherтирую source gives cap: стра a rule.","Terms section B.3 permitsницу не открывал for a vault a human of about 150.

### B. Я-owned–180k filesдро в git, machine account used.

```json
 склад с only{
  "claimsнаружи

GitHub пишет for automated tasks, with at": [
    {
 прямо      "claim": most one free machine account besides: ре a free personal account. That "Scalar’s enlistпозиторий лучшеment is a permission дер small was Git inspectedжать меньше worktree,, 1 ГБ, меньше usually but no inspected page 5 ГБ настоя src authorтельно,/, with untrackedizes an git files such unbounded high-frequency private не сдела as build artifacts kept-repository channel."]}
 outside thatн резер``` directory; scalarвной clone downloads копи only commitsей, с and trees and checksгенерированное out only the top и-level files unless большое --full-clone класть в объектное is used.",
      "confidence": хранилище, "high",
 а      "evidence": "The не в git. [About scalar manual says large files on GitHub, живая an страница, снята 2026- enlistment “10-02](https://docsusually contains the subdirectory src/ which is.github.com/en/repositories/ a Git worktreeworking-with-files/managing-large-files/about-large-” and “encourages the separation betweenfiles-on-github). [ tracked files (insideestablished src/) and un]

Вашtracked files, such склад ( as build artifacts (outside src/).несколько” ГБ заме Cloneток “, 7.4 ГБ черновиков, by3 ГБ оригиналов, default” 150–180 тысяч fetches “only commit and файлов) в этот tree objects,” enables конверт sparse-checkout except with --full- не входит. Яclone, and leaves “onlyдро из со files presentтен … in the top- илиlevel directory.”", низкой
      "source тысячи_locator": " текстоhttps://git-вых файлов входит,scm.com/docs если ту/scalar",
да не по      "source_title": "Gitпадают бинарники и если scalar один и Documentation",
      тот же markdown "source_type не пере": "primary"
писывается двумя система    },
    {
      "claimми.

Критери": "Partial cloneй от is Gitбора, который’s mechanism вы for veryдерживает large repositories: провер it can skipку.

* В blobs and trees outside a ядро по narrow coneпадает то, что должно or old versions of large binaries быть одинаковым, then на всех машинах demand-fetch missing objects from a promisor remote, and и что человек способен it does not replace про shallow or single-читать diff-branch clone.",ом: канон, регламенты,
      "confidence": "high", реестр
      "evidence машин": "The partial, OnAir,-clone manual says коро theткие JSON реестров.
* В склад feature lets остаётся то, что только Git “ дописывается иfunction почти никогда without having a complete не сли copy” of “вается: оригинаextremely large repositories,” citingлы, черновики, транскрипты, в “ложения.
* Се500K directories and креты не3.5M по files” whenпадают ни only a “ туnarrow cone” isда, needed, and “ ни туlarge binary assets” whoseда. Конфиг `.claude` у previous versions need вас уже едет своим not be downloaded. git. Missing objects are “demand fetched Второй” from a prom разisor remote, через and the user я must be online forдро волта его that. Non не ве-goals sayзи it onlyте limits blobs and trees. Для “ конфиwithin aгов машин как given range of commits раз существует” and is independent of shallow clone chezmoi: один source, single branch repo, or, шаблоны по a fetch refspec.", ОС
      "source и имени_locator": " хоста, секреты из менеhttps://git-scm.com/docsджера, не из git/partial-clone. [chezmoi, страница",
      "source_title": «what does chezmoi do», 202 "Git partial-clone Documentation",
6-04-17](https      "source_://chezmoi.io/what-doestype": "primary-chezmoi-do/). Это"
    },
 про    {
      "claim": "Sparse dot checkout keeps onlyfiles, не про вол a chosenт знаний. [established]

Конси subset of trackedстентность ду files in the workingбля. Только tree; одна cone mode selects сторона пишет. Хаб публи directories,кует снимок из списка допуска git commit -a does. Пир not record делает paths outside that `git pull` и не ком subset as deletedмитит обратно, and the command. Дву is still marked experimentalсторонний.",
      " синкconfidence": "high тех",
      " же файлов черезevidence": "git Syncthing и через-sparse-checkout “ git — это способ повтоchange[рить волs] the workingну из tree from having all 959 конфли tracked files present toктов уже only having a subset в двух,” местах. Syncthing при chosen “ одновременby providing a list of directories in coneной mode (the default правке пере).”име Itно statesвывает про that “играgit commit -a will not record pathsвший файл в `*. outside the sparse-sync-conflict-*` и разcheckout directories/patternsносит as deleted,” эту ко and that “THIS COMMAND IS EXPERIMENTALпию как.”",
      обычный файл. Он "source_locator не резервная": "https:// копия:git-scm.com удаление тоже/docs/git раз-sparse-checkout",
      "носитсяsource_title":. [Syncthing, conflicting "Git git- changes]sparse-checkout Documentation(https://docs.syncthing",
      ".net/users/syncing.html);source_type": "primary"
    [Syncthing FAQ },
    {
, не      "claim": backup "Git LFS](https://docs.synct keepshing.net/users a small pointer in/faq.html). Да the Git repository and stores the fileты на bytes страница outsideх документации normal пла Git blobsвающие, selected by path patterns in, .gitattributes механизм много.",
      "летconfidence": "highний. [established]

Partial clone и",
      " sparse checkout имеютevidence": "The Git LFS specification смысл для says that “ монорепоinstead of writing large, blobs to a Git где к repository, only aлон pointer file is written тол,” and thatстый, а pointer files “ рабоmust be less thanчая копия нужна 1024 bytes уз.” Clean and smudge filtersкая. Для “ensure ядра that large files aren’t в written into the repository несколько proper, instead being stored locally at . соgit/lfs/тен файлов они не нужны.objects/{OID- Sparse-PATH} … synchronizedcheckout with в the Git LFS server.” cone- The .gitatрежиtributes file decidesме Git which paths useHub разбира the filters; the sampleл в tracks 2020 году *.mp3 and на примере *.zip with filter=lfs.", дерева в
      "source_locator": " миллионыhttps://github.com путей/git-lfs. Это/git-lfs старый,/blob/main/docs/spec.md",
      но перви "source_titleчный текст": "Git L. [GitHub BlogFS Specification",
, 2020-01-17      "source_](https://github.blog/type": "primary"
    },
open-source/git/bring-    {
      "your-monorepo-down-claim": "Gitto-size-with-sparse-’s submodule documentationcheckout/). [ treats splitting one logicalestablished] как project into several возможность repositories as a supported git, не как ваш alternative to one large следующий repository, including putting шаг.

Git LFS ре large binariesшает in shallow-cloned submodules большие and би notнарники це fetching repositories you doной not need к.",
      "вотыconfidence": "medium",
      " храevidence": "gнилища и траitsubmodules lists “фика. На Free иSplitting a ( Pro этоlogically single) project into multiple repositories and tying them back together” to address 10 ГБ хра “Size of the Git repository,”нилища и 10 ГБ трафика в месяц, дальше деньги. [Product usage included, живая табли becauseца](https://docs.github Git “.com/en/billing/reference/scales upproduct-usage-included). Для я poorly for large repositoriesдра из containing content that is markdown not compressed by delta computation.” The LFS не ну example is toжен. Для “use submodules оригина to hold large binaryлов вол assets”та and “ LFS —shallowly clone” them “ плоsuchхой склад that you do not. [established]

### C. С have a large historyобытийное пробуждение

Три способа, и locally,” and, for transfer size, to ни один не “avoid fetching the за working trees of the repositories you are notменяет тя interested in.”",гу
      "source.

* **Вебхук на_locator": " свойhttps://git приёмник.** GitHub ж-scm.com/дёт ответ docs/gitsub2xxmodules",
      за "source_title": "Git g 10 секунд, иначе считаетitsubmodules Documentation",
      "source доставку_type": " проprimary"
    },ва
    {
     лом. Са "claim": "м проObsidian Sync’s publishedвален selection rulesное are file type не досыла and folder exclusionет. Ру, not Gitчная: images повтор, audio, videoная доставка есть, and PDFs только за are the последние default 3 дня. Порядок selective types, событий не обе otherщ types need “ан. ПоSync all other typesлезная,” Standard нагруз andка ре Plus capжется на 25 МБ. При aёмник file должен проверить at 5 MB and 200 MB под, alreadyпись и-synced exclusions тип stay события и on отве the remote, dot-prefixedтить paths сразу are excluded, а работу except . сделатьobsidian, and после Sync. should not run on [best the same files practices] as i(https://docs.github.com/Cloud, Dropboxen/web, OneDrive,hooks/using-webhooks/best or Google Drive.",
      "confidence-practices-for-using-": "medium",webhooks); [handling failed deliveries,
      "evidence Enterprise Server 3.18 docs": "The indexed](https://docs.github.com official Sync/en/enterprise-server@3 settings page says selective.18/webhooks/using- sync is on bywebhooks/handling-failed-webhook default for images-deliveries); [red, audio, videoselivering,, and PDFs о; otherкно 3 дня](https:// types require “Syncdocs.github.com/enterprise-cloud all other types”;@latest/webhooks/testing- Standard allows files upand-troubleshooting-webhooks/red to 5 MBelivering-webhooks); порядок and Plus up to 200 MB; — adding a file to официа Exльная французcluded files “ская страница тойdoes not remove it from the remote vault же до if it has alreadyки, [troubleshooting been synced”; folders web can be excluded;hooks](https://docs.github and names beginning with “.com/fr/webhooks/testing.”-and-troubleshooting-webhooks/ are excluded except “troubleshooting-webhooks). [established]
.obsidian.”* **Опрос The Switch с ред to Obsidian Sync pageким интервалом.** Это, и fetched есть in full, says Obsidian does not модель recommend Sync Flux: “ редalongside cloud storage services (eкий pull.g. i как страховCloud, Dropbox, OneDrive,ка, вебхук как ранний Google Drive)” because з thatвонок. Для can cause conflicts, вашего рит while thoseма «два раза в сутки, services “ часто только онmay play a roleбординг и авария in your backup strategy» о.”",
      "source_locatorпрос раз": "https:// в несколькоobsidian.md/help/sync/ часовsettings", че
      "sourceст_title": "нее,Ob чем cronsidian Help раз: Sync settings and в минуту.
* ** selective syncing",GitHub Actions как будильник.**
      "source_type": " Расprimary"
    }
писание не  ],
  чаще "uncertainties раза": [
    "No inspected в 5 минут, в ча official Git or Obsidianсы пи page states a designк or limit с for anдвигается Obsidian vault of roughly, 150– при180k files.", высокой нагруз
    "Obке заданиеsidian Help могут does not document a small Git core с plusнять an external store, с and it does not очереди. [Events mention Git LFS, partial clone, that trigger workflows] sparse checkout, or(https://docs.github.com/ scalar.",
   en/actions/using "The submodule manual-workflows/events-that-trigger-workflows); [scheduling issue still says Git “does not allow partial creation, trees to be transferred про in fetch or clone задерж,” which conflictsку и drop with the current partial-clone manual;](https://docs.github.com/actions neither/managing-issues-and-pull page reconciles the two.",-requests/scheduling-issue-creation
    "The). От large-object-деpromisors document describes a separate remoteльный for large blobs as запрет: на хо an alternative to Gitст- LFS,ран but it is aнерах design note and says Actions нельзя it is not possible to push only specific занима objects to a promть деisor remote.",
лом    "git bundle, которое is official не является one-way replication сбор of Gitкой, тестом, вы objects and refs, with noкладкой git push into a или публикацией проекта этого bundle, but it is not a репозитория. Условия действуют non-Git content store с 27 августа 2026. beside a small core [Git.",
    "HubA direct Terms for Additional Products, Actions] fetch of https://obsidian.md/(https://docs.github.com/help/sync/en/site-policy/github-settings returned anterms/github-terms-for- empty body; theadditional-products-and-features). selective-sync Будить wording is from the search index of that дома officialшний флот из Actions — се URL, while httpsрая зона этого://obsidian.md пункта/help/sync. [established/switch was fetched] как текст in full."
 условий, [speculative]  ]
}
``` как «вас за это отключат» на двух тихих прогонах в сутки.

Как положить событие в уже идущую сессию. Внешний вебхук до сессии Claude Code или Codex не доходит. Доходит локальный факт: файл в инбоксе, который сессия и так умеет читать, или хук процесса на этой машине. MCP-нотификация из облака GitHub в локальную сессию — лишний прыжок через хаб.

Анти-паттерны, уже оплаченные чужими системами.

* Шторм: частый опрос плюс запись heartbeat в тот же репозиторий. Вы сами производите события, на которые просыпаетесь.
* Петля: пробуждение пишет новое сообщение, оно будит снова. У Flux та же форма, когда reconcile возвращает поле, которым владеет другой контроллер, и они дерутся каждый интервал. [Flux, ignore rules, 2026-08-03](https://fluxcd.io/blog/2026/08/ignore-rules-drift-detection/).
* Правило для вашего демона: входящее будит. Исходящее из самого пробуждения в шину не пишется. Повтор того же id конверта игнорируется.

### D. Сравнение

| | Порядок | Подтверждение | Офлайн | Деньги | Человек видит |
|---|---|---|---|---|---|
| Telegram, уже есть | Лента чата хранится в облаке, пока клиент дойдёт | ACK у вас в конверте, не в протоколе Telegram | Да, история дожидается клиента | Уже платите вниманием, не счётом API | Лучше всего |
| Папка Syncthing, уже есть | Нет общего порядка. Конфликт — переименование | Нет квитанции доставки. Ваш `fileshare.py` это прямо пишет | Только когда оба узла снова в сети одновременно | Ноль | Плохо, конфликтные копии выглядят как данные |
| Git-коммиты, rail #5 | Порядок есть только на одной ветке после rebase | Push принят не значит, что пир прочитал | Пир забирает всё пачкой, когда появится сеть | Ноль на малом объёме | Плохо, если сообщений много |
| GitHub Issues | Комментарии в треде читаемы. Вебхуки могут прийти не по порядку | Закрытие issue — слабый ACK | Нет, без API не прочитать | Ноль, пока не упрётесь в лимит создания | Лучше git, хуже чата |
| NATS JetStream | Поток с номером сообщения | At-least-once: нет ack, будет повтор. Есть nak, term, «ещё работаю» | Да, пока жив диск якоря | VPS уже ваш | Плохо без отдельной страницы |
| MQTT | Очередь брокера | QoS есть, брокер всё равно нужен | Зависит от брокера и сессии | То же | Плохо |
| Gist, repo-на-пира | Не шина | Нет общего журнала | Плохо | Упрётесь в лимит 100 000 репозиториев только если размножите безумно. Вам не грозит | Хуже одного репо |

JetStream: [доставка и ack, дока NATS, страница от 2026-09-30](https://docs.nats.io/learn/jetstream/delivery-and-acknowledgment). [established]

Цифры «10 миллионов сообщений в секунду» из сравнений NATS и MQTT я не беру в расчёт. Это вендорские блоги, для пяти машин они ничего не решают. [Synadia, 2026-02-17](https://www.synadia.com/blog/nats-vs-mqtt-technical-comparison-iot-fleet-management), [low-authority].

Вывод по матрице. Наблюдаемость человеком у вас уже куплена Telegram. Подтверждение доставки у вас уже нарисовано в конверте HMAC и ACK. Дыра не в отсутствии третьего носителя. Дыра в том, что потребитель спит.

### E. Риски

* **Секрет остаётся в истории.** GitHub прямо говорит: вычистить файл мало. Он живёт в чужих клонах, в кэше по SHA-1 и в ссылках pull request. Нужен `git-filter-repo` и согласование со всеми, кто клонировал. Чужие клоны вы не чистите. [Removing sensitive data](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository). Push protection блокирует новый пуш известного секрета, старую историю сам не лечит. [Working with push protection](https://docs.github.com/code-security/secret-scanning/working-with-secret-scanning-and-push-protection/working-with-push-protection-from-the-command-line). [established]
* **Запрета «репозиторий нельзя использовать как очередь» в тексте нет.** Есть запрет чрезмерной автоматической нагрузки, спама и права удалить репозиторий, который давит на инфраструктуру. [Acceptable Use, разделы 4 и 9, снято 2026-10-02](https://docs.github.com/en/site-policy/acceptable-use-policies/github-acceptable-use-policies). Пять машин и десяток сообщений в день под это не похожи. Heartbeat каждые пять минут в общий репозиторий уже похож на ту нагрузку, от которой написаны лимиты. [established] текст, [speculative] порог, на котором вас ткнут.
* **GitHub как единая точка отказа.** Для ядра это терпимо, потому что Syncthing остаётся. Для рации это плохо, если выключить Telegram. Частные сбои GitHub бывают. Будить флот только из Actions значит уснуть вместе с ними.
* **Шум коммитов.** При двух снимках ядра в сутки история читается. При сообщении-на-коммит история канона перестаёт быть историей канона. Это и есть поломка S3, если положить туда `messages/`.

## 4. Пилот на 14 дней

Цель пилота: убить или оставить идею дёшево. Новых сервисов нет. `git_bus.py` не включается.

**Дни 1–7. Только замер.**

Двадцать конвертов в текущую шину, с часами отправки. На одном пире записать три времени: конверт создан, файл или сообщение Telegram появилось локально, сессия это увидела. Десять конвертов, пока оба узла онлайн. Десять, после того как пир побыл выключенным.

Рядом счёт: сколько новых `*.sync-conflict-*` появилось в волте за эти дни.

Развилка в конце недели.

* Онлайн-доставка меньше двух минут, а сессия видит через часы. Идея git-шины для пробуждения мертва. Жив слушатель. Идти в дни 8–14.
* Онлайн-доставка сама измеряется часами. Папочная шина больна при живой сети. Тогда, и только тогда, имеет смысл ставить JetStream. Ядро всё равно не смешивать с сообщениями.
* Конфликтов сотни в неделю на файлах, которые вы хотели в ядро. Список допуска резать ещё сильнее, двусторонний синк этих путей выключать до публикации в git.

**Дни 8–14. Два узких опыта сразу.**

Опыт слушателя. На одном пире демон смотрит существующий инбокс. Новое входящее с чужим адресатом пишет локальный файл-будильник или поднимает сессию тем способом, которым вы уже поднимаете сессии. Повтор того же id молчит. Демон в шину не пишет. Успех: девять из десяти онлайн-конвертов будят сессию быстрее чем за две минуты, и число сообщений в шине не выросло из-за самого демона. Провал: хотя бы одна петля «проснулся и написал».

Опыт ядра. Хаб один раз публикует список допуска. Жёсткий потолок: 500 файлов, только текст, ни одного секрета, ни одного файла из `_originals` и черновиков. Пир делает read-only clone. Правка канона в эти две недели идёт как раньше в vault, а в git её заново снимает только хаб. Успех: клон с нуля быстрее минуты, в истории нет секрета, пир не закоммитил ничего, конфликтов на этих путях ноль. Провал: хоть один секрет в первом пуше, или кто-то начал править ядро и vault как два источника.

Что пилот сознательно не проверяет: вебхуки GitHub, Actions, NATS, Issues, включение rail #5. Их цена станет понятна только после развилки дня 7. До этого они шумят.

## Решение

Три именованных варианта.

1. **Только S1.** Дёшево и попадает в лаг сессий. Не даёт версии канона и не уменьшает скан склада. Риск: если папка больна при живой сети, вы потеряете неделю и вернётесь к брокеру.
2. **S1 плюс одностороннее ядро без сообщений.** Рекомендация. Канон получает историю, рация остаётся той, что уже есть, пробуждение становится локальным. Риск и допущение: лаг сидит в расписании потребителя. Если дни 1–7 покажут обратное, ядро всё равно остаётся, а рацию уводите на JetStream, Telegram не выключая.
3. **Сразу S5 и ядро.** Чище на бумаге полугодия. Дороже на этой неделе и не доказано вашим замером. Брокер без слушателя на пире снова спит.

Уверенность рекомендации: [emerging]. Главное допущение: при двух включённых машинах нынешняя шина уже довозит конверт за минуты, а полдня сидит робот, который смотрит инбокс два раза в сутки.

Источник, который перевернёт рекомендацию: таблица двадцати конвертов из дней 1–7. Если онлайн-доставка сама длинная, рекомендация меняется на вариантOfficial docs 3, и do not put это a будет git-repository bus, уже Telegram, a [ Syncthing folderestablished], MQTT, NATS/JetStream на, GitHub Issues ваших ча or Discussions, gistsсах, а не на чу, and one repoжих до per peer inках.

## one comparison. Each Источники

1. GitHub Docs source. only Repository limits. Жи states its own rulesвая страница, снята 2026 for order-10-02. https://docs, acknowledgement, offline.github.com/en/repositories/ use, costcreating-and-managing-repositories/, or whatrepository-limits
2. GitHub a person can see Docs. About large files on GitHub.

```json
. Живая страница, снята {
  "claims2026-10-02. https": [
    {
      "claim":://docs.github.com/en/ "On onerepositories/working-with-files/ shared Git repository,managing-large-files/about- a second concurrent push is rejected aslarge-files-on-github
 a3. GitHub Docs. Rate limits non-fast- for the REST API. httpsforward until that://docs.github.com/rest/ developeroverview/rate-limits-for- fetches and merges;the-rest-api
4. that GitHub Docs. Accept is overwrite protectionable Use Policies. С, not a perнята-message delivery 2026-10-02. acknowledgement.",
      https://docs.github.com/en "confidence": "/site-policy/acceptable-usehigh",
     -policies/github-acceptable-use "evidence": "-policies
5. GitHub DocsPro Git says Jessica's. Terms for Additional Products and Features. push is rejected because Дата she is trying to редакции push non-fast -forward changes and202 cannot push until she fetches6-08-27. https:// and merges, anddocs.github.com/en/site that Git will not-policy/github-terms/github let users overwrite each-terms-for-additional-products other.",
      "source_locator-and-features
6. Git": "https://Hub Docs. Removing sensitive data from agit-scm.com repository. https/book/en://docs.github.com/en//v2/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-Distributed-Git-Distributed-Workflows",
      "source_title":repository
7. GitHub Docs. "Pro Git, Working with push protection from the command line 5.1. https://docs.github.com/ Distributed Workflowcode-security/secret-scannings",
     /working-with-secret-scanning "source_type": "primary"
-and-push-protection/working-with-push-protection-from    },
    {
      "claim-the-command-line
8": "Official. GitHub Docs. Webhook best Git docs practices. https describe one public repository://docs.github.com/en/ per developer: eachwebhooks/using-webhooks/ person canbest-practices-for-using write their-webhooks
9. GitHub own public repository and Docs. Handling failed webhook deliveries. https read the others,://docs.github.com/en/ push to their own [email hidden]/, and continuewebhooks/using-webhooks/ workinghandling-failed-webhook-deliveries while
10. GitHub Docs. Red aelivering webhooks. О maintainer pulls laterкно 3 дня. https://docs.",
      "confidence": "high.github.com/enterprise-cloud@",
      "latest/webhooks/testing-andevidence": "The-troubleshooting-webhooks/redel integration-manager workflowivering-webhooks
11. says GitHub Docs. Troubleshooting webhooks, each developer has write access to their own француз public repository and readская реда access to everyone else'sкция,, pushes про changes порядок to their own public до copy, and contributors do not have toставки. https://docs.github.com wait for the main/fr/webhooks/testing- project to incorporate thoseand-troubleshooting-webhooks/troubleshooting changes.",
     -webhooks
12. GitHub "source_locator Docs. Events that trigger workflows. Ми": "https://нимальныйgit-scm.com интервал schedule/book/en 5 минут. https://docs./v2/Distributed-Git-github.com/en/actions/using-workflows/events-that-triggerDistributed-Workflows",
      "-workflows
13. GitHub Docssource_title":. Scheduling issue creation. За "Pro Git,держка и с 5.1нятие заданий Distributed Workflows",
      "source. https_type": "://docs.github.com/actions/primary"
    },managing-issues-and-pull-
    {
     requests/scheduling-issue-creation
 "claim": "14. GitHub Docs. Product usageA Syncthing folder does included with each plan. https://docs.github.com/en/billing/ not replay an ordered history afterreference/product-usage-included
 offline15. GitHub Blog. time: on Bring your monorepo down to size with sparse-checkout.  reconnect it takes2020-01-17. Ста the neighbor's latest synchronizedрый источник. https://github.blog state, and overlapping/open-source/git/bring updates, deletes,-your-monorepo-down creates, and conflicts-to-size-with-sparse collapse to the latest change for that-checkout/
16. Anthropic Engineering file or directory.",. Nicholas Carlini. Building a C compiler with a team of parallel Claudes. 2026-02
      "confidence": "high",
      "evidence": "The-05. https://www.anthropic.com/engineering/building-c FAQ-compiler
17. Flux. Setup says that after time Web offline Syncthing gets thehook Receivers. Страница от 2025-10- result03. https://fluxcd.io/ of allflux/guides/webhook-receivers changes since/
18. Flux. Flux from End the last known state-to-End. Страница от, and for overlapping updates, deletes 2026-07-02., creates, and https://fluxcd.io/flux/flux-e2e/
19. conflicts it only sees the latest change for Flux blog a given file or. Selective drift correction with ignore directory.",
      rules. 2026-08- "source_locator03. https://fluxcd.io/": "https://blog/2026/08/ignoredocs.synct-rules-drift-detection/
20hing.net/users. git-bug. Data model,/faq.html", вет
      "source_title": "ка trunkSyncthing FAQ. Даты публикации нет,. https://github.com/git Does the audit log contain every change?",-bug/git-bug/blob
      "source/trunk/doc/design/data_type": "-model.md
21. NATSprimary"
    }, Docs. Delivery and acknowledgment. Стра
    {
     ница от 2026-09- "claim": "30. https://docs.nats.ioN/learn/jetstream/delivery-ATS JetStream, unlike Core NATSand-acknowledgment
22. Synct, giveshing Docs sequenced. Understanding Synchronization storage, explicit, раздел acknowledgement with Conf redelivery, andlicting Changes. https offline catch-up://docs.syncthing.net/: publisherusers/syncing.html
23. and subscriber need not be online at the Syncthing Docs. FAQ, Syn same timecthing is not a backup. https.",
      "://docs.syncthing.net/confidence": "highusers/faq.html
24. chez",
      "moi. What does chezmoi do?evidence": "Core Страница от 2026-04 NATS delivers only-17. https://chezmoi.io to subscribers connected at publication,/what-does-chezmoi- at most once anddo/
25. Stack Overflow. never replayed. How git works when two peers JetStream append push ats a sequence number, and once. 2011-01- an acknowledgement advances the10. Старый источник, consumer cursor механизм fast; if a-forward. https://stackoverflow.com/ message is not acknowledgedquestions/4643998/how- in time thegit-works-when-two- server redeliverspeers-push-changes-to- it, whichsame-remote-simultaneously
26. the docs call Synadia. at-least- Nonce deliveryATS vs MQTT. 2026-. Jet02-17. [low-authorityStream extends], в decoupling to расчёт циф time soр не бра the two sidesл. https://www.synadia need not be online.com/blog/nats-vs- togethermqtt-technical-comparison-iot-.",
      "fleet-management
27source_locator": "https://docs. Локальный код.nats.io/.concepts/jetstream `E",
      ":\!CLAUDE-Palosource_title":AltoPC-June26\public- "NATS Documentationrepos\fleet-deploy\transports, JetStream",\git_bus.py` и `
      "source_type": "transports/primary"
    },README.md`. Смо
    {
     трел 2026-10- "claim": "02.

НеMQTT's подтвер officialждено spec makes отде acknowledgementльной откры aтой страницей в QoS choice and этом bounds заходе: Dolt, JuiceFS offline catch-up, пер by session expiry: QoвичнаяS 0 can до lose messages, Qoка DS 1 canVC ( duplicate them, QoредиреS 2 aimsкт не до atчитал), текст exactly once, and июльского an ordered topic preserves внутреннего publish order from one исследования. У client at oneтвер QoS.",ждения про
      "confidence них в": "high", вердикт не входят.

В
      "evidenceторой про": "The OASход поIS standard тем defines at most же вопросам once, at least ещё идёт отде once withльным исследователь possible duplicates, andским кон exactly once. Anту Ordered Topic is oneром. Если он при where messagesнес from the same clientёт источник at the same QoS are received in, который противоре publish order. Ifчит разви Session Expiry Intervalлке is 0, дней the session ends when 1–7 или the connection closes; ли a client onмитам intermittent connectivity can set выше a short interval, по to resume reliableправлю вер delivery, and applicationдикт то messages are lost if itчечно, а не у does not reconnect beforeсред the session expires.",ню
      "source.

🧒 Про_locator": "https://docs.стыми словами: большаяoasis-open.org пол/ка сmqtt/mqtt/v5. бу0/os/магаmqtt-v5ми.0-os пу.html",
     сть "source_title стоит": "MQTT дома Version 5. и0, OASIS Standard",
      е "source_typeз": "primary"
дит как    },
    е {
      "claim": "Telegram'sз Bot FAQ pricesди ordinaryла. В bot сей messaging at no chargeф but кла rateдём только тон-limits it,кую while GitHub's docs treat папку самых важных ли Issues and Discussions as human conversationsстов and every, и клю gist as a viewч отable Git repository, сейфа without describing один consumer, у acknowled хаба.gements.", Рация
      "confidence у": "high",
      "evidence вас уже есть. Она": "Telegram says мол bots canчит не message users потому, at no cost, что ра but shouldция пло avoid more than one message per second inхая, а потому что дежу a chat, cannotрный send more than 20 messages per minute за in a group,гля and cannotды broadcast more than about 30 messages perвает в неё два раза в second unless paid broadcasts день are enabled. С. GitHub saysначала постав issuesь дежурного are for planning, discussing, and tracking work, subscribers get notifications of the latest comments, and some conversations belong in Discussions instead. GitHub also says every gist is a Git repository that can be forked and cloned, and у двери. Новую рацию покупать имеет смысл только если замер покажет, что старая правда its full commit history не доносит can, be viewed with diffs даже.",
      "source_locator": "https://core.telegram.org/bots/faq", когда обе
      "source_title": " машины включены.Telegram Bots FAQ; GitHub Docs, About issues; GitHub Docs, Creating gists",
      "source_type": "primary"
    }
  ],
  "uncertainties": [
    "No inspected official page compares a git-repository bus with Telegram, Syncthing, MQTT, NATS or JetStream, GitHub Issues or Discussions, gists, and one repo per peer on all five axes together.",
    "The current git-scm.com URL for Getting Started, What is Git? returned a missing-page notice, so the often-cited local-operation wording was not re-read.",
    "Broker operating cost for NATS or MQTT, and GitHub plan pricing, are not stated on the pages inspected.",
    "Telegram user-client offline history and message order were not on the Bot FAQ page that was opened."
  ]
}
```
