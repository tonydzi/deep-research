# DR26-10-02-HUB-02-1811 — raw report from mistral

> Unedited output of one model. Our reconciled synthesis is the page one level up.

<!-- leg: mistral · rail: Mistral Vibe CLI (подписка Pro), vibe --trust --auto-approve --max-turns 80, в промпте требование URL · node: a lab machine (Claude-a lab machine.de95de29) · finished 2026-10-02T18:38:41Z (mtime ответа) · правка разметки: 2 строк «---» (горизонтальные линии) заменены на «* * *», иначе dr_leg_gate.py:173 режет тело как frontmatter; сырой ответ sha256 b457992185c66e43… у узла ~/lab/dr/DR26-10-02-HUB-02-1811-legs/mistral.raw.md -->

Фактура собрана по всем блокам — пишу отчёт.

* * *

# DR26-10-02-HUB-02-1811 — ответ

## 1. Вердикты по S1–S5

| Сценарий | Жизнеспособность | Главный риск | Цена владения (6 мес) |
|---|---|---|---|
| **S1** Статус-кво+ (dual-send + пробуждение всех пиров) | Высокая (краткосрочно) | Лаг до 12ч на пирах не исчезает; Telegram не даёт ни ordering, ни подтверждений — рельса уже перегружена | Низкая, но нулевая отдача по боли 150k-файлов |
| **S2** Реанимация git_bus как есть (triple-send) | **Низкая** | Параллельные push конфликтуют; ordering теряется; у git нет семантики доставки/ACK | Средняя инфра + высокая драгоценная вжа по операм |
| **S3** Ядро-репо (git) + messages/ + webhook | Средне-высокая | Секреты в истории навсегда; GitHub становится SPOF для слоя канона; шум коммитов | Средняя |
| **S4** GitHub Issues как шина | Средняя | Кладбище issues, неразличимое с реальным операционным шумом; полный lock-in | Низкая инфра, средняя курируемость |
| **S5** NATS/MQTT на VPS-якоре + ядро-репо | **Высшая** | Ещё один self-hosted сервис на обслуживании; наблюдаемость человеком хуже (нужен мост) | Низкая (один Go-бинарник на уже существующем VPS) |

**S1.** Через месяц работает, и это единственный сценарий без новых точек отказа. Пробуждение всех пиров поверх doorbell физически возможно только через long polling Telegram Bot API ([getUpdates](https://core.telegram.org/bots/api)) — вебхуки к машинам за NAT не дотягиваются, а smee.io — это dev-инструмент, не продакшен ([probot/smee.io](https://github.com/probot/smee.io)). Через 6 месяцев ничего не изменилось: шина по-прежнему «fire-and-forget», Syncthing по-прежнему сыплет `.sync-conflict` при одновременных правках ([док Syncthing](https://docs.syncthing.net/users/syncing.html)), скан 150k файлов по-прежнему тяжёл. Сломается не техникой, а тем, что идея не закрывает ни одну из трёх исходных болей. Цена: ~дни работ.

**S2.** Убить. Третья рельса поверх dual-send без единственного писателя даёт ту же болезнь, что и Syncthing-шина — параллельные записи без ordering. Git решает её только дисциплиной «один писатель» или rebase-петлями; GitHub сам рекомендует ≤6 pushes/мин на репо и жёстко режет push >2 ГБ ([Repository limits](https://docs.github.com/en/repositories/creating-and-managing-repositories/repository-limits)). Пулл по cron каждые N минут × 6 машин упирается в rate limits API ([5000 req/h authenticated](https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api)) и secondary limits. У шины нет доставки-гарантий: GitHub не перепосылает неудачные вебхуки и не даёт листинга пропущенного ([док](https://docs.github.com/en/webhooks/using-webhooks/handling-failed-webhook-deliveries), [discussion #24721](https://github.com/orgs/community/discussions/24721)) — а git-слой тем более. Июльское исследование уже назвало файловую шину плохой идеей; git-файл — та же файловая шина, только с историей. Rail #5 умер после одного сообщения не случайно.

**S3.** Ядро-репо — лучшая половина этой идеи, messages/ — худшая. Ядро (канон, регламенты, реестры, OnAir, машинные JSON) в git — это правильный ход: у `.claude` это уже работает. Но папка messages/ наследует все грабли S2 плюс новое: история git append-only, значит «шум коммитов» не чистится без переписывания истории, а переписывание требует координации со всеми клонами и не трогает форки ([док](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)). Через месяц всё работает (трафик маленький). Через 6 месяцев: сотни машинных коммитов в день разъедают историю, конфликты push при редком онбординге, и операционная правда — «git не рация, git — это лedger». Цена: средняя.

**S4.** IssueOps — реальный, одобренный самим GitHub паттерн ([github.blog: IssueOps](https://github.blog/engineering/issueops-automate-ci-cd-and-more-with-github-issues-and-actions/), [github/command](https://github.com/github/command)): комменты = команды, labels = адресация, триггер issue_comment, UI наблюдаем человеком бесплатно. Через месяц это красиво: адресация labels (`to:хаб`, `urgent`), полнотекстовый поиск, никакой возни с git-историей. Через 6 месяцев: машино-issues не отличимы от живого операционного трекера; API создания issues под ударами упирается в secondary rate limits; закрытие/архивация сотен тикетов — отдельная рутина. Подходит как слой «обращения к оператору», не как транспорт. Цена: низкая инфра, средняя гигиена.

**S5.** Технически самый честный сценарий. NATS — один Go-бинарник ([nats.io](https://nats.io/)), JetStream даёт персистентность с at-least-once и exactly-once-дедупликацией ([docs.nats.io JetStream](https://docs.nats.io/concepts/jetstream), [FAQ](https://docs.nats.io/reference/faq)), ordering на subject, нативные ACK, offline-пир дочитывает поток по возвращении. Tailscale делает приватную сеть тривиальной ([tailscale](https://github.com/tailscale/tailscale)). VPS-якорь уже есть. Через месяц: работает. Через 6 месяцев: работает и не тянет за собой ни GitHub-зависимость, ни конфликтов. Главный риск — самодельный сервис надо бэкапить и обновлять; и шину не видно человеческим глазом (лечится мостом в Telegram, который уже есть). Цена: низкая.

## 2. Рекомендуемый сценарий: гибрид S5 + ядро из S3 (без messages/)

Архитектура из трёх слоёв, у каждого файла ровно один мастер:

1. **Рация (события, сообщения, пробуждение)** — NATS JetStream на VPS-якоре внутри Tailscale. Субъекты `fleet.<machine>.*`, `fleet.session.*`, `fleet.all`, `fleet.wake.*`. Конверт — текущий формат machine_bus (адресат + HMAC) без изменений; git_bus.py списывается, транспорт меняется на nats_pub/nats_sub — движок конвертов переиспользуется целиком. OnAir-доска = KV-bucket JetStream (ключ `onair/<session>`), аналог retained-сообщений MQTT ([HiveMQ retained](https://www.hivemq.com/blog/mqtt-essentials-part-8-retained-messages/)); мёртвый пир детектится отсутствием heartbeat.
2. **Ядро (канон)** — приватный GitHub-репо ~тысячи файлов, **генерируемый** из волта однонаправленным экспортом (волт = мастер, git = зеркало + точка восстановления). Машинные JSON и OnAir-стейт, наоборот, мастерятся в git. Правило: у одного файла один мастер, никогда два. Secret scanning + push protection обязательны ([push protection](https://docs.github.com/en/code-security/secret-scanning/introduction/about-push-protection)).
3. **Мега-склад** — остаётся Syncthing, но разбитый на шейрываемые папки поменьше, чтобы конфликтные волны (959 файлов) и тяжёлый скан локализовались; конфликт-файлы у Syncthing — не баг, а признание, что репликация без ordering ([док](https://docs.syncthing.net/users/syncing.html)).

**Пробуждение:** NATS-демон на каждой машине подписан на `fleet.wake.<machine>`; класс `urgent` будит живую сессию (hooks/файловый inbox), класс `task` порождает новую. Ответ GitHub-событий (webhook на публичный VPS, где и так стоит NATS) превращается в публикацию в субъекта — не в отдельную инфраструктуру. Обычный ритм — те же 2 синка в сутки; частый — на онбординге и в аварии, как и задумано.

**План внедрения:**
1. День 1: NATS + JetStream на VPS, systemd, Tailscale-адрес, ключи.
2. День 2-3: `nats_pub.py`/`nats_sub.py` = обёртки над существующим движком machine_bus.py (конверт, HMAC, ACK — без изменений).
3. День 3-4: шим-режим: dual-send продолжает работать, NATS пишет параллельно (tee). Меряем.
4. Неделя 2: wake-демон на хабе + одном мае; только класс urgent; анти-шторм: дедуп по origin-id с TTL, будильник не будит в ответ на wake-класс (правило «агент не будит агента дважды»).
5. Неделя 3: перевод пиров на NATS как primary для сообщений; Telegram остаётся human-observability-мостом и дублирующей рельсой.
6. Неделя 4: экспорт ядра из волта → core-репо (список: канон, 02-Decisions, регламенты, реестры, .claude-канон уже git-ится — собрать в ту же схему).
7. Неделя 5+: разбор Syncthing-папки на модули; messages из «внутренний архив лаборатории» в архив.

## 3. Ответы на вопросы A–E

**A. Git как message bus в реальном мире.**
- GitOps — самый массовый паттерн «git как источник истины», но с ключевой оговоркой: там git — **desired state, а не транспорт**; доставку делает reconciliation-цикл агента, не сам git ([ArgoCD vs Flux](https://www.portainer.io/blog/argocd-vs-flux)). Это и есть правильный способ думать о ядре-репо.
- Прямые аналоги «сообщения в git»: [git-bug](https://github.com/git-bug/git-bug) (распределённый трекер, issues как git-объекты, push/pull между пирами), [dspinellis/git-issue](https://github.com/dspinellis/git-issue) (тот же принцип). Radicle унёс это в продакшен-паттерн Collaborative Objects — «issues, discussions и code review как git-объекты» ([radicle.dev](https://radicle.dev/), [LWN](https://lwn.net/Articles/966869/)). Общий вывод этих проектов: структура «один объект = один файл/одна ветка» работает, «мутация общего файла несколькими писателями» — нет.
- Nostr как архитектурная параллель: события с подписями, relay-транспорт, отсутствие глобального ordering ([NIP-01](https://github.com/nostr-protocol/nips/blob/master/01.md)) — полезная рамка для конвертов, но сам по себе тянет публичную сеть, вам не нужна.
- Грабли, подтверждённые документацией: push >2 ГБ режется, ≤6 pushes/мин рекомендуется ([repo limits](https://docs.github.com/en/repositories/creating-and-managing-repositories/repository-limits)); файлы >100 МБ запрещены ([discussion #146740](https://github.com/orgs/community/discussions/146740)); API 5000 req/h ([rate limits](https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api)); неудачные webhook-доставки не перепосылаются ([handling failed deliveries](https://docs.github.com/en/webhooks/using-webhooks/handling-failed-webhook-deliveries)). Jujutsu показывает, как выглядит VCS, спроектированный под вечный rebase и живые конфликты ([docs.jj-vcs.dev](https://docs.jj-vcs.dev/latest/conflicts/)) — но это не решает «шины».
- CRDT-over-git как готового продакшен-проекта нет; git merge — не CRDT, при конфликте он рождает текстовый маркер, который машина не разрулит ([Wikipedia: CRDT](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type)).
- JuiceFS/Dolt/DVC — правильный способ держать «версии над большим»: метаданные отдельно от блоков ([JuiceFS](https://github.com/juicedata/juicefs)), данные в object storage + указатели в git ([DVC remote storage](https://doc.dvc.org/user-guide/data-management/remote-storage)), SQL-версии с git-CLI ([Dolt](https://github.com/dolthub/dolt)).

**B. Ядро в git + склад вне git.**
- Критерии отбора ядра: то, что загружается в контекст агента каждый раз (канон, регламенты, реестры, решения, OnAir, машинные JSON), т.е. файлы с высокой read-frequency и малой read-склонностью к байтам; итог — тысячи, не сотни тысяч.
- Консистентность: одно-направленная генерация (экспорт из волта), не двунаправленная синхронизация. Двух-мастерная схема = то, что уже производит 959 конфликтов в Syncthing. Правило «у файла один мастер» — главное в этой интеграции.
- Monorepo-vs-split: Google держит монорепо на миллиарды строк, но ценой кастомного стека — Piper, виртуальная ФС, кастомный индекс; прямой перенос на git не предполагался ([Potvin & Levenberg, CACM 2016](https://research.google/pubs/why-google-stores-billions-of-lines-of-code-in-a-single-repository/)). Вывод для вас: монорепо на 150k файлов волта в git не воспроизводить; ядро — да.
- Если бы волт всё же целиком пошёл в git: partial clone (`--filter=blob:none`, `--filter=tree:0`) и sparse checkout снижают стоимость ([github.blog](https://github.blog/open-source/git/get-up-to-speed-with-partial-clone-and-shallow-clone/)), Git LFS для бинарников — но с квотами ~1 ГБ storage/bandwidth в месяц бесплатно, при превышении LFS молча отключается ([LFS billing](https://docs.github.com/en/billing/managing-billing-for-git-large-file-storage/about-billing-for-git-large-file-storage)). Для «мега-склада» это запрет, не решение. Seafile тоже не панацея: у него свои пределы на размер библиотек ([форум Seafile](https://forum.seafile.com/t/library-is-too-large-to-sync/14897)).
- Для конфигов агентов по машинам эталон — chezmoi ([chezmoi.io](https://www.chezmoi.io/)); ваш `.claude`-git-автоснимок каждые 15 минут — та же идея, её и держать.
- Практика Obsidian+git: obsidian-git с auto commit-and-sync интервалом — рабочий паттерн для маленьких волтов, не для ваших объёмов ([obsidian-git plugin](https://community.obsidian.md/plugins/obsidian-git)).

**C. Событийное пробуждение сессий.**
- Вебхуки → self-hosted приёмник: приёмник должен быть на публичном HTTPS-эндпоинте; для приватного флота это ваш VPS (Tailscale не поможет — GitHub должен уметь дозвониться). Требования к секрету и валидации подписи — [best practices for webhooks](https://docs.github.com/en/webhooks/using-webhooks/best-practices-for-using-webhooks), [валидация доставок](https://docs.github.com/en/webhooks/using-webhooks/validating-webhook-deliveries). Гарантий доставки нет: ретраев нет, очередь надо вести самому ([handling failed deliveries](https://docs.github.com/en/webhooks/using-webhooks/handling-failed-webhook-deliveries)).
- Polling с бэкоффом: conditional requests через ETag/If-None-Match — GitHub отвечает 304 без траты rate limit, если ничего не изменилось ([best practices REST API](https://docs.github.com/en/rest/using-the-rest-api/best-practices-for-using-the-rest-api)). Это честный запасной вариант, если VPS-приёмник не хочется.
- GitHub Actions как будильник: `repository_dispatch` по HTTP POST → workflow → а дальше Actions-раннер всё равно должен как-то дозвониться до машин (обычно тем же Telegram-мостом) ([Marc Nuri: triggering across repositories](https://blog.marcnuri.com/triggering-github-actions-across-different-repositories)). Это лишнее звено для приватной шины.
- Доставка события в идущую локальную сессию: реальный механизм — файловый inbox, который демоны уже умеют ([inotify](https://man7.org/linux/man-pages/man7/inotify.7.html)), плюс hooks-события кодового агента на lifecycle-фазах ([Claude Code hooks](https://code.claude.com/docs/en/hooks)). MCP не даёт push-нотификаций в живую сессию — поэтому файловый inbox остаётся универсальной стыковкой.
- Анти-паттерны: шторм пробуждений — классический thundering herd ([Wikipedia](https://en.wikipedia.org/wiki/Thundering_herd_problem)); петли «агент будит агента» — лечатся только жёсткими правилами транспорта: дедуп по origin-id+TTL, wake-класс не порождает wake-класс, лимит авто-пробуждений на сессию в час, и ключевое — будильник живёт вне LLM, в детерминированном демоне, чтобы сама модель не могла «договориться» о встрече в 3 часа ночи.

**D. Матрица альтернатив.**

| Транспорт | Ordering | Подтверждение доставки | Offline-устойчивость | Цена | Наблюдаемость человеком |
|---|---|---|---|---|---|
| Telegram-группа | нет | нет (read ≠ ack) | да (история на сервере) | 0 | высокая |
| Syncthing-папка | нет | нет (конфликты как побочный продукт, [док](https://docs.syncthing.net/users/syncing.html)) | да, с лагом | 0 | низкая |
| git-репо (S2/S3) | только при одном писателе | нет (пуш ≠ прочитано) | да | средняя | низкая |
| GitHub Issues (S4) | слабое (timeline) | частично (API-статусы) | нет (офлайн = слеп) | низкая | **высокая** |
| MQTT (Mosquitto) | per-topic | QoS 1/2 ([mosquitto](https://mosquitto.org/man/mqtt-7.html)) | persistent session, retained ([HiveMQ](https://www.hivemq.com/blog/mqtt-essentials-part-8-retained-messages/)) | низкая | низкая |
| **NATS JetStream** | **per-subject гарантированный** | **native ACK, at-least-once/exactly-once ([JetStream](https://docs.nats.io/concepts/jetstream))** | **да, pull-consumer** | **низкая** | низкая (мостом в TG) |

Telegram оставить как human-bridge и запасную рельсу (он уникально хорош наблюдаемостью и пробиванием NAT через [long polling](https://core.telegram.org/bots/api)); Issues не использовать как транспорт, но можно как витрину обращений; git — только для состояния, не для событий.

**E. Риски.**
- Секреты в истории: git не забывает; удаление = переписывание истории с координацией всех клонов, форки не чистятся ([removing sensitive data](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)). Обязательны push protection и secret scanning ([docs](https://docs.github.com/en/code-security/secret-scanning/introduction/about-push-protection)) и правило «ядро генерируется фильтром, никогда не коммитится вручную».
- ToS: прямого запрета «репо как очередь» в Acceptable Use Policies нет, но есть «disrupting the experience» и общие abuse-крючки ([AUP](https://docs.github.com/en/site-policy/acceptable-use-policies/github-acceptable-use-policies)); реальный ограничитель — операционные лимиты (6 pushes/мин) и billing Actions ([docs](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions)). Формально терпимо, практически — сам себя ограничит.
- SPOF: у GitHub были сутки деградации при 43-секундном сетевом сбое — Oct 21–22, 2018 ([разбор](https://medium.com/@tnale/how-githubs-database-self-destructed-in-43-seconds-d4b4f7b9a617)). Ядро-репо обязано быть **зеркалом**, а не мастером, иначе GitHub-бан/аутаж ослепляет флот.
- Шум коммитов: append-only история —Messages-класс данных нельзя положить в git надолго без регулярной хирургии истории.

## 4. Минимальный пилот на 2 недели

Цель: дёшево доказать/убить «NATS как рация» и «ядро-репо как зеркало», не трогая продакшен-шину.

- **Дни 1–2.** NATS+JetStream на VPS-якоре (systemd, Tailscale, HMAC-ключи). Один бинарник ([nats.io](https://nats.io/)).
- **Дни 3–4.** `nats_pub.py`/`nats_sub.py` поверх существующего конверта machine_bus (адресат+HMAC+ACK — без изменений). Шим: dual-send продолжает работать, NATS включается параллельно (tee).
- **Дни 5–8.** Wake-демон на хабе + одном мае, только класс `urgent`. Файловый inbox → hooks живой сессии; `task` → новая сессия. Дедуп origin-id+TTL, будильник игнорирует wake-класс.
- **Дни 9–10.** Односторонний экспорт ядра (канон+реестры+решения) в приватный core-репо, push protection ON, скрипт `vault→git` по расписанию 2×/сутки.
- **Дни 11–14.** Замер и решение. Метрики: латентность доставки p50/p95 (NATS vs Telegram vs Syncthing), доля ACK за 5 мин, число ложных пробуждений, человекочасы на сопровождение за неделю, инциденты по секрет-сканеру.
- **Kill-критерии:** латентность NATS не лучше Telegram; >1% сообщений без ACK при живом пире; >2 ложных пробуждений/неделю; сопровождение >1 ч/неделю. При любом — возвращаемся к S1, а ядро-репо (оно независимо полезно как бэкап канона) оставляем.
- Бюджет: ~$0 инфраструктуры, 2–4 дня работы агентов.

## Источники

1. GitHub repository limits — https://docs.github.com/en/repositories/creating-and-managing-repositories/repository-limits
2. REST API rate limits — https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api
3. REST API best practices (conditional requests, ETag) — https://docs.github.com/en/rest/using-the-rest-api/best-practices-for-using-the-rest-api
4. Webhooks: best practices — https://docs.github.com/en/webhooks/using-webhooks/best-practices-for-using-webhooks
5. Webhooks: handling failed deliveries (нет автоперепосылок) — https://docs.github.com/en/webhooks/using-webhooks/handling-failed-webhook-deliveries
6. Community: «no retries of failed webhook deliveries» — https://github.com/orgs/community/discussions/24721
7. Community: repo/file size limits — https://github.com/orgs/community/discussions/146740
8. Git LFS billing — https://docs.github.com/en/billing/managing-billing-for-git-large-file-storage/about-billing-for-git-large-file-storage
9. Partial clone / shallow clone — https://github.blog/open-source/git/get-up-to-speed-with-partial-clone-and-shallow-clone/
10. Repository_dispatch через REST — https://blog.marcnuri.com/triggering-github-actions-across-different-repositories
11. smee.io (dev-only) — https://github.com/probot/smee.io
12. git-bug — https://github.com/git-bug/git-bug
13. git-issue (dspinellis) — https://github.com/dspinellis/git-issue
14. Radicle (COBs как git-объекты) — https://radicle.dev/ и https://lwn.net/Articles/966869/
15. Nostr NIP-01 — https://github.com/nostr-protocol/nips/blob/master/01.md
16. Jujutsu (конфликты, автorebase) — https://docs.jj-vcs.dev/latest/conflicts/ и https://github.com/jj-vcs/jj
17. JuiceFS — https://github.com/juicedata/juicefs
18. DVC remote storage — https://doc.dvc.org/user-guide/data-management/remote-storage
19. Dolt — https://github.com/dolthub/dolt
20. Potvin & Levenberg, «Why Google Stores Billions of Lines of Code in a Single Repository», CACM 2016 — https://research.google/pubs/why-google-stores-billions-of-lines-of-code-in-a-single-repository/
21. chezmoi — https://www.chezmoi.io/
22. Syncthing: конфликт-файлы — https://docs.syncthing.net/users/syncing.html
23. Seafile: лимиты на размер библиотек — https://forum.seafile.com/t/library-is-too-large-to-sync/14897
24. obsidian-git — https://community.obsidian.md/plugins/obsidian-git
25. NATS JetStream — https://docs.nats.io/concepts/jetstream и https://docs.nats.io/reference/faq
26. NATS — https://nats.io/
27. MQTT retained messages — https://www.hivemq.com/blog/mqtt-essentials-part-8-retained-messages/
28. Mosquitto (persistent session, QoS) — https://mosquitto.org/man/mqtt-7.html
29. Telegram Bot API — https://core.telegram.org/bots/api
30. IssueOps (GitHub Blog) — https://github.blog/engineering/issueops-automate-ci-cd-and-more-with-github-issues-and-actions/
31. github/command — https://github.com/github/command
32. Removing sensitive data from a repository — https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository
33. Push protection — https://docs.github.com/en/code-security/secret-scanning/introduction/about-push-protection
34. GitHub Acceptable Use Policies — https://docs.github.com/en/site-policy/acceptable-use-policies/github-acceptable-use-policies
35. GitHub Actions billing — https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions
36. Разбор аутажа GitHub Oct 2018 (43 секунды → 24 часа) — https://medium.com/@tnale/how-githubs-database-self-destructed-in-43-seconds-d4b4f7b9a617
37. Claude Code hooks — https://code.claude.com/docs/en/hooks
38. inotify(7) — https://man7.org/linux/man-pages/man7/inotify.7.html
39. Thundering herd problem — https://en.wikipedia.org/wiki/Thundering_herd_problem
40. Tailscale — https://github.com/tailscale/tailscale
41. git-annex sync — https://git-annex.branchable.com/sync/
42. ArgoCD vs Flux (GitOps reconciliation) — https://www.portainer.io/blog/argocd-vs-flux
43. CRDT — https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type

* * *

Итог: рекомендую гибрид — шина на NATS JetStream (VPS-якорь + Tailscale, конверт machine_bus переиспользуется), GitHub только как зеркало ядра волта с push protection, Syncthing остаётся для мега-склада, Telegram — человеческий мост. Rail #5 (git_bus как шина) и Issues-как-транспорт — не реанимировать. Пилот на 2 недели стоит ~0 долларов и либо даёт метрики доставки, либо убивает идею чисто.
