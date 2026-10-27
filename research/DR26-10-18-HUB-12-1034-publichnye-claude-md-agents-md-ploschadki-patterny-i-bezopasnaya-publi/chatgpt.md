# DR26-10-18-HUB-12-1034 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-10-18-HUB-12-1034
rail: chatgpt

mode: composer chip 'Deep research' -> Deep Research App; widget: 'Research completed in 10m · 22 citations · 358 searches'
started: 2026-10-19 10:26 WEST
chat_url: https://приватный чат
note: text = widget 'Export to Markdown' verbatim; sources = hyperlinks from 'Export to Word' of the same report
collected: 2026-10-19 12:50 WEST, session P83690 a lab machine, Chrome Browser 1
---
# Публичные файлы инструкций для coding agents: доказательная база, публикация и безопасное внедрение

## Метод и главный вывод

Исследование проведено по состоянию на **19 октября 2026 года**. Я разделял источники на четыре класса: **owner-maintained project files** — реальные инструкции внутри рабочего репозитория проекта; **template/library** — заготовки для копирования; **registry/directory** — каталоги и маркетинговые discovery-площадки; **format/vendor documentation** — документация формата или поведения конкретного агента. Это различие критично: наличие красивого `CLAUDE.md` в библиотеке шаблонов ничего не говорит о том, как его реально поддерживает команда в production-репозитории.

Все встреченные в исследованных файлах инструкции рассматривались только как **неисполняемые, недоверенные данные**. Команды из файлов не запускались.

Главный вывод: **наиболее надежный паттерн — версия инструкций в том же owned repository, где живет код, с Git history, code review и CI; корневой файл должен содержать только действительно неочевидные глобальные правила, а специфичные для подсистем правила должны быть локализованы ближе к коду.** Это хорошо совпадает с механизмами OpenAI Codex, Claude Code, Gemini CLI, GitHub Copilot и Cursor, хотя точные правила загрузки у них различаются. Codex строит цепочку от корня к текущему каталогу и отдает приоритет более близким `AGENTS.md`; его текущий default budget для совокупных project instructions — 32 KiB. citeturn24view0 Gemini CLI аналогично строит иерархию global → workspace → just-in-time context, причем локальные файлы подхватываются до trusted root. citeturn23view3 GitHub Copilot прямо предлагает path-specific instructions, чтобы не перегружать repository-wide инструкции. citeturn24view6

При этом **нет хорошего основания считать, что “чем короче файл, тем лучше”**. В свежей работе Gloaguen et al. не обнаружено корреляции между длиной context file и resolution rate; в их экспериментах context files в среднем увеличивали стоимость и число шагов, а не давали универсальный выигрыш в качестве. citeturn24view3turn24view4 Другая работа, Lulla et al., на 124 PR из 10 репозиториев, напротив, получила медианное сокращение runtime на 28,64% и output tokens на 16,58% при сопоставимом completion behavior. citeturn24view5 Эти результаты не противоречат друг другу настолько, насколько кажется: исследования используют разные наборы задач, агентов и метрики; они скорее говорят, что **ценность файла надо проверять экспериментально в своей среде, а не выводить из длины или самого факта наличия `AGENTS.md`.**

Репрезентативные запросы, использованные при поиске:

```text
site:github.com "AGENTS.md"
site:github.com "CLAUDE.md"
site:github.com "GEMINI.md"
site:github.com ".cursorrules"
site:gitlab.com "AGENTS.md" "blob"
site:docs.github.com create gist GitHub Gist official
cursor.directory submit rules .cursorrules
PatrickJS awesome-cursorrules GitHub contributing submit
claude-code-templates GitHub submit template
site:cursor.com/docs .cursorrules legacy project rules Cursor
licensing a repository GitHub Docs
classifying your repository with topics GitHub Docs
site:geminicli.com/docs GEMINI.md hierarchy trust context
```

Дополнительно для GitHub-репозиториев проверялись сами project files, repository metadata, последние commits по конкретному path и `LICENSE`, а не только README.

## Карта источников и площадок

Ниже **13 проверенных полезных мест/репозиториев с понятным механизмом изменения или подачи**, плюс несколько каталогов, которые были исследованы, но которые я сознательно не считаю надежными publication targets без повторной проверки их текущих contribution rules.

| Класс | Источник, точный URL | Владелец и evidence date | Как файл туда попадает |
|---|---|---|---|
| **Owner-maintained** | Shopware `AGENTS.md`: `https://github.com/shopware/shopware/blob/trunk/AGENTS.md` | `shopware` organization; файл реально содержит Definition of Done, subtree guidance и lint commands; последнее подтвержденное изменение path — 2026-10-14. fileciteturn29file0L2-L6 fileciteturn0file0L1-L2 | Commit в owned repo; внешние изменения — обычный PR: `https://github.com/shopware/shopware/pulls`. Repository metadata подтверждает public repo и PR support. fileciteturn5file0L2-L6 |
| **Owner-maintained** | Google DeepMind `GEMINI.md`: `https://github.com/google-deepmind/simply/blob/main/GEMINI.md` | `google-deepmind`; последнее изменение этого path входит в commit `b5aec4…`, committed 2026-09-30. fileciteturn6file0L2-L6 | Commit/PR в `google-deepmind/simply`: `https://github.com/google-deepmind/simply/pulls`. Repo public, Apache-2.0, PRs включены. fileciteturn7file0L2-L6 |
| **Owner-maintained** | Palo Alto Networks `GEMINI.md`: `https://github.com/PaloAltoNetworks/pan-mcp-relay/blob/main/GEMINI.md` | `PaloAltoNetworks`; последний path commit `8cb0f215…`, 2025-08-14. fileciteturn8file0L2-L6 | Commit/PR: `https://github.com/PaloAltoNetworks/pan-mcp-relay/pulls`. fileciteturn9file0L2-L6 |
| **Owner-maintained** | Mandiant `GEMINI.md`: `https://github.com/mandiant/harbinger/blob/main/GEMINI.md` | `mandiant`; latest path commit `0ff6bd4…`, committed 2025-10-08. fileciteturn12file0L2-L6 | Commit/PR: `https://github.com/mandiant/harbinger/pulls`. Repo — Apache-2.0. fileciteturn13file0L2-L6 |
| **Owner-maintained** | Sentry Spotlight `.cursorrules`: `https://github.com/getsentry/spotlight/blob/main/.cursorrules` | `getsentry`; path updated in commit `e50d252…` on 2025-11-22. fileciteturn14file0L2-L6 fileciteturn15file0L2-L6 | Commit/PR: `https://github.com/getsentry/spotlight/pulls`. Repository remains active in 2026. fileciteturn16file0L2-L6 |
| **Owner-maintained** | Panfactum `CLAUDE.md`: `https://github.com/Panfactum/stack/blob/main/CLAUDE.md` | `Panfactum`; latest path commit `f3fb3d0…`, 2026-04-03. fileciteturn2file0L2-L6 fileciteturn18file0L2-L6 | Commit/PR: `https://github.com/Panfactum/stack/pulls`. fileciteturn19file0L2-L6 |
| **Owner-maintained** | Ultracopier `CLAUDE.md`: `https://github.com/alphaonex86/Ultracopier/blob/master/CLAUDE.md` | `alphaonex86`; latest path commit `d110578…`, 2026-09-10. fileciteturn3file0L2-L6 fileciteturn20file0L2-L6 | Commit/PR: `https://github.com/alphaonex86/Ultracopier/pulls`. fileciteturn21file0L2-L6 |
| **Owner-maintained** | Leemons `.cursorrules`: `https://github.com/leemonade/leemons/blob/main/.cursorrules` | `leemonade`; latest path commit `81550dc…`, 2024-11-18; repository last pushed 2025-05-14 according to GitHub metadata. fileciteturn22file0L2-L6 fileciteturn23file0L2-L6 fileciteturn24file0L2-L6 | Commit/PR: `https://github.com/leemonade/leemons/pulls`; это реальный project file, но формат требует проверки на актуальность относительно нынешнего Cursor. |
| **Owner-maintained** | Gramps `AGENTS.md`: `https://github.com/gramps-project/gramps/blob/maintenance/gramps61/AGENTS.md` | `gramps-project`; добавлен maintainers в commit `3eda7b1…`, committed 2026-04-06; repo pushed 2026-10-18. fileciteturn26file0L2-L6 fileciteturn28file0L2-L6 fileciteturn27file0L2-L6 | Commit/PR: `https://github.com/gramps-project/gramps/pulls`; repository metadata показывает `has_pull_requests=true`. fileciteturn27file0L2-L6 |
| **Owner-maintained, non-GitHub** | OpenBlob `AGENTS.md`: `https://gitlab.com/lx-industries/openblob/-/blob/main/AGENTS.md?ref_type=heads` | LX Industries; проверено 2026-10-19. Страница показывала owner project file и commit `aeb54aea`; инструкция включает schema-generation и CI blocker semantics. citeturn14search22 | Изменение в самом GitLab project через merge request: `https://gitlab.com/lx-industries/openblob/-/merge_requests`. Это не registry submission. |
| **Publication venue** | GitHub Gists: `https://gist.github.com/` | GitHub; документация проверена 2026-10-19. Public Gists индексируются и searchable, каждый Gist сам является Git repository. citeturn23view2 | Sign in → Gist home → `+` → filename (`AGENTS.md`, `CLAUDE.md` и т. п.) → **Create public gist**. GitHub также документирует `gh gist create`. citeturn23view2 |
| **Template/library** | `https://github.com/PatrickJS/awesome-cursorrules` | PatrickJS/community; проверено 2026-10-19. Это **collection of reusable rules**, не доказательство owner adoption конкретным продуктом. citeturn13search2turn16view5 | Contribution guide требует вносить новые/обновленные rules через modern rule form и обычный repository PR; маршрут: `https://github.com/PatrickJS/awesome-cursorrules/pulls`. citeturn16view5 |
| **Template/library** | `https://github.com/davila7/claude-code-templates` | `davila7`/contributors; проверено 2026-10-19. Это библиотека templates/assets, а не production instruction corpus. citeturn13search3 | CONTRIBUTING описывает fork/clone → feature branch → changes/tests → pull request; маршрут: `https://github.com/davila7/claude-code-templates/pulls`. citeturn16view6 |

**Дополнительно исследованы, но не включены в эти 13 как подтвержденные каналы публикации owner file.** `https://cursor.directory/` — community discovery/catalog для Cursor ecosystem, который сами Cursor docs упоминают для community plugins/MCP, но в проверенной версии сайта я не получил достаточно надежного, однозначного rule-submission workflow именно для произвольного owner-maintained instruction file; поэтому отправлять туда файл “по памяти” не рекомендую. citeturn13search1turn21search9 `https://cursorrules.org/` — browser/validator/migration-oriented ресурс вокруг rules, но не source of truth owner project. citeturn13search13 `https://agents.md/` — полезная документация идеи/формата, но **не свидетельство того, что конкретный файл поддерживает владелец проекта**, и не должна смешиваться с выборкой production files. citeturn14search2

Для discovery на самом GitHub более надежен native mechanism **Topics**: GitHub прямо говорит, что topics помогают находить проекты и контрибьютить в них, а featured topics поддерживаются через `github/explore`. Точный путь для featured-topic contributions: `https://github.com/github/explore`. citeturn23view0

## Репрезентативные owner-maintained файлы

Восемь основных файлов ниже были прочитаны непосредственно как project files; README summary не использовался вместо содержимого файла.

| Проект | Канонический и commit URL | Какие секции реально использованы | Maintenance evidence | Лицензия |
|---|---|---|---|---|
| **Shopware** — `AGENTS.md` | Canonical: `https://github.com/shopware/shopware/blob/trunk/AGENTS.md`  Commit: `https://github.com/shopware/shopware/blob/53a6cceb130892ed385933a3eac502f6d0b9cbfc/AGENTS.md` | `AI Skills → Definition of Done`, `Subtree Guidance`, `Coding Guidelines → File Linting`. Файл требует тестов для behavior changes, запрещает объявлять CI success без доказательства запуска, отсылает CI/Admin work к nested `AGENTS.md` и дает конкретные lint commands. fileciteturn29file0L2-L6 | Path изменялся 2026-10-14; основной repo активно обновлялся 2026-10-19. fileciteturn0file0L1-L2 fileciteturn5file0L2-L6 | **MIT** на уровне repository. fileciteturn5file0L2-L6 |
| **Google DeepMind Simply** — `GEMINI.md` | Canonical: `https://github.com/google-deepmind/simply/blob/main/GEMINI.md`  Commit: `https://github.com/google-deepmind/simply/blob/b5aec4a7b6f3f1df9cbf37ca66ab02eca59439b7/GEMINI.md` | `Installation`, `Common Commands`, `Testing`: setup команд, CPU/multi-device testing и особенности generated gRPC artifacts. Связанный commit прямо описывает восстановление generation path и тестовую схему, включая отдельные multi-device processes. citeturn3view1 fileciteturn6file0L2-L6 | Instruction path вошел в крупное v0.4.0 update, committed 2026-09-30; repo pushed 2026-10-13. fileciteturn6file0L2-L6 fileciteturn7file0L2-L6 | **Apache-2.0**. fileciteturn7file0L2-L6 |
| **Palo Alto Networks pan-mcp-relay** — `GEMINI.md` | Canonical: `https://github.com/PaloAltoNetworks/pan-mcp-relay/blob/main/GEMINI.md`  Commit: `https://github.com/PaloAltoNetworks/pan-mcp-relay/blob/8cb0f21511712c12d2fb0844685d8fcb3a4ec2e5/GEMINI.md` | `Core Development Rules`, tooling/formatting sections. Есть точные `uv`/pytest expectations и commit-trailer conventions, но также внутреннее противоречие между разными line-length/type-checking указаниями. citeturn3view2 | Последний path commit 2025-08-14; сам repo pushed 2026-01-27. fileciteturn8file0L2-L6 fileciteturn9file0L2-L6 | **PolyForm Internal Use License 1.0.0**; текст прямо ограничивает distribution. Это особенно важный пример того, почему “publicly readable” ≠ “можно копировать в свой публичный шаблон”. fileciteturn10file0L2-L6 |
| **Mandiant Harbinger** — `GEMINI.md` | Canonical: `https://github.com/mandiant/harbinger/blob/main/GEMINI.md`  Commit: `https://github.com/mandiant/harbinger/blob/0ff6bd4a5c599173de6af842b0f1463eba78d87a/GEMINI.md` | `Getting Started`, `Tooling and Commands`, `Testing`, `Frontend`. Файл фиксирует точный venv, ruff/mypy invocations, Testcontainers/Docker limitation и source-of-truth decision по frontend API paths. fileciteturn11file0L2-L6 | Последний file-related commit — 2025-10-08. Repo pushed 2026-06-23. fileciteturn12file0L2-L6 fileciteturn13file0L2-L6 | **Apache-2.0**. fileciteturn13file0L2-L6 |
| **Sentry Spotlight** — `.cursorrules` | Canonical: `https://github.com/getsentry/spotlight/blob/main/.cursorrules`  Commit: `https://github.com/getsentry/spotlight/blob/e50d25229e0ee60a5e7d3d8f297259b190c574ea/.cursorrules` | `Node Version Management`, `Package Management`, `Common Commands`, `Important Notes`: Volta, pinned Node/pnpm, pnpm-only policy, build/test/e2e/lint commands. fileciteturn14file0L2-L6 | Path touched in E2E-infrastructure commit on 2025-11-22; repo pushed 2026-10-17. fileciteturn15file0L2-L6 fileciteturn16file0L2-L6 | Root `LICENSE` — **Apache-2.0**, несмотря на GitHub metadata `Other`; actual license text важнее поверхностной metadata classification. fileciteturn17file0L2-L6 |
| **Panfactum Stack** — `CLAUDE.md` | Canonical: `https://github.com/Panfactum/stack/blob/main/CLAUDE.md`  Commit: `https://github.com/Panfactum/stack/blob/f3fb3d0e870fa4364c31247c8f207dfedc8de181/CLAUDE.md` | Весь файл фактически состоит из `Code Guidelines → Guiding Principles`: readability, testability, correctness. Это полезный контрпример: **короткий root существует, но сам по себе не дает setup/test/verification contract**. fileciteturn2file0L2-L6 | Path touched 2026-04-03 вместе с развитием skills/changelog workflow; repo pushed 2026-07-02. fileciteturn18file0L2-L6 fileciteturn19file0L2-L6 | **AGPL-3.0**. fileciteturn19file0L2-L6 |
| **Ultracopier** — `CLAUDE.md` | Canonical: `https://github.com/alphaonex86/Ultracopier/blob/master/CLAUDE.md`  Commit: `https://github.com/alphaonex86/Ultracopier/blob/d110578beefda1f2f8d80e7a9fb5b29f22275715/CLAUDE.md` | `Building and Running`, `Git history (read-only reference)`, `The async/thread backend is STABLE — do NOT modify it`, `Build Portability`. Содержит хороший non-obvious safety boundary — stable backend как read-only reference — но также публично зашит локальный путь `/mnt/data/perso/progs/ultracopier/git/`. fileciteturn3file0L2-L6 | Path updated 2026-09-10; repo pushed 2026-10-11. fileciteturn20file0L2-L6 fileciteturn21file0L2-L6 | **GPL-3.0**. fileciteturn21file0L2-L6 |
| **Gramps** — `AGENTS.md` | Canonical: `https://github.com/gramps-project/gramps/blob/maintenance/gramps61/AGENTS.md`  Commit: `https://github.com/gramps-project/gramps/blob/3eda7b1e49b3fa13a573b9f7787cfd3222ea2b63/AGENTS.md` | `Code Style & Formatting → Black`, `Testing → Running Tests`, `Type Checking`, `Commit Messages`, `Branch merges`. Есть runnable Black diff command, exact unittest discovery, `mypy`, naming rules и explicit “rebase rather than merge”. fileciteturn26file0L2-L6 | Maintainer commit от 2026-04-06; commit message прямо говорит, что файл добавлен как guidelines “for agents and contributors”; repo pushed 2026-10-18. fileciteturn28file0L2-L6 fileciteturn27file0L2-L6 | Repository metadata: **GPL-2.0**; сам `AGENTS.md` требует для новых Python-файлов GPL-2.0-or-later headers, то есть конкретный license scope надо читать внимательно. fileciteturn27file0L2-L6 fileciteturn26file0L2-L6 |

Отдельно я проверил **Leemons `.cursorrules`**. Это хороший отрицательный пример: в `Key Principles` говорится “functional, declarative programming; avoid classes”, тогда как ниже, в `Data`, предлагается использовать classes with internal validation. Там же есть трудно проверяемый бюджет “functions less than 20 instructions” и extensive testing expectations без сопоставимо четкого canonical test command. Файл действительно owner-maintained, но его последнее найденное изменение относится к 18 ноября 2024 года; GitHub license metadata — `Other/NOASSERTION`, поэтому права на повторное использование нельзя уверенно выводить из публичной доступности. fileciteturn22file0L2-L6 fileciteturn23file0L2-L6 fileciteturn24file0L2-L6

## Что подтверждают реальные файлы и vendor documentation — и где практика ломается

**Scope и precedence.** Это один из наиболее надежно поддержанных паттернов. Codex официально идет от project root к CWD; `AGENTS.override.md` имеет приоритет внутри уровня, а инструкции ближе к рабочей директории появляются позже в объединенном prompt и переопределяют root guidance. citeturn24view0 Gemini CLI загружает global context, workspace/parent context и JIT context из непосредственно затронутых директорий до trusted root. citeturn23view3 GitHub Copilot отдельно поддерживает `.github/instructions/NAME.instructions.md` для path-specific правил именно как способ не засорять global instructions. citeturn24view6 Shopware демонстрирует это на практике: root `AGENTS.md` делегирует Administration и `.github` в nested `AGENTS.md`. fileciteturn29file0L2-L6

**Claude Code требует особенно аккуратно понимать precedence.** На 19 октября 2026 года Claude Code уже умеет напрямую читать `AGENTS.md` начиная с v2.1.277, однако по default, если в текущей или родительской директории присутствует `CLAUDE.md`/`CLAUDE.local.md`, Claude читает их вместо project `AGENTS.md`; поведение можно изменить через Project instructions setting. Nested `AGENTS.md` подхватываются по мере работы в соответствующих subdirectories. citeturn24view2 Следовательно, старое утверждение “Claude Code всегда требует отдельный `CLAUDE.md`” уже **устарело**.

**Short root + nested specifics выглядит разумнее giant universal prompt, но это не аргумент в пользу произвольного ограничения длины.** Anthropic пишет, что более specific и concise инструкции обычно соблюдаются стабильнее и рекомендует выносить scoped behavior в rules; OpenAI имеет фактический 32 KiB cumulative project-doc cap по умолчанию. citeturn24view1turn24view0 Но Gloaguen et al. не увидели корреляции success rate с длиной context file. citeturn24view4 Поэтому “500 строк плохо, 100 строк хорошо” — не evidence-based правило. Правильнее измерять **полезную плотность, redundancy, contradiction rate и влияние на task success/cost**.

**Reproducible commands — один из самых сильных элементов реальных файлов.** Shopware указывает команды отдельно для PHP, PHPStan, JS/TS, SCSS, Twig, translations и Actions. fileciteturn29file0L2-L6 Gramps дает exact diff-based Black invocation, unittest discovery и `mypy`. fileciteturn26file0L2-L6 Harbinger указывает конкретный Python из project venv для pytest/mypy/ruff, что предотвращает использование случайной системной версии tooling. fileciteturn11file0L2-L6 Sentry аналогично привязывает Node/pnpm к Volta и дает commands для build, tests, e2e, lint и format. fileciteturn14file0L2-L6

**Generated files должны иметь источник истины и проверяемый regeneration path.** OpenBlob делает direct edits generated schemas merge blocker и указывает `just generate-schema`; Google DeepMind Simply в связанном обновлении отдельно чинит generation of gRPC stubs. citeturn14search22 fileciteturn6file0L2-L6 Это намного сильнее неопределенного “don't edit generated files”: агенту нужны source path, generation command и check, подтверждающий clean diff.

**Evidence of completion сильнее словесного “make sure tests pass”.** У Shopware формулировка для CI особенно удачна по сути: workflow не должен возвращать success без доказательства, что нужная работа реально выполнилась; Definition of Done связывает behavioral changes с tests и lint каждого затронутого файла. fileciteturn29file0L2-L6 Gramps дает команды, которые можно показать reviewer'у. fileciteturn26file0L2-L6 Harbinger показывает обратную сторону: instruction “ask the user to run pytest or ignore database-related errors” честно описывает Testcontainers limitation, но без дополнительной дисциплины легко превратить инфраструктурный failure в ложное “проверено”. fileciteturn11file0L2-L6

**Natural-language instructions не являются security policy.** Anthropic прямо предупреждает, что `CLAUDE.md` — context, а не enforced configuration; для жесткого запрета действия рекомендует `PreToolUse` hook. citeturn24view1 Cursor также документирует hooks, способные наблюдать, блокировать или изменять agent loop; project hooks загружаются из `.cursor/hooks.json` в trusted workspace. citeturn21search6 Поэтому запреты типа “never upload secrets” надо дублировать техническими ограничениями: sandbox/network policy, hooks, secret scanning и permissions.

**Trust boundary должен быть явным.** Gemini JIT discovery останавливается на trusted root. citeturn23view3 Cursor отдельно предупреждает о workspace trust и для untrusted repositories советует не полагаться на обычный AI workspace mode. citeturn21search24 Отсюда практический вывод: найденный в repo текст не должен иметь полномочия сам расширять свои privileges, отключать security checks или инициировать external communication.

**Review ownership в выборке существенно слабее test ownership.** Shopware детально говорит о PR hygiene и follow-up commits, Gramps — о commit format/rebase, но сами instruction files редко содержат ясную формулу вроде “changes to this file require review by `@platform-devex`”. fileciteturn29file0L2-L6 fileciteturn26file0L2-L6 Для долговременного policy-like документа это пробел; ownership разумнее выразить технически через `CODEOWNERS`/protected branch, чем фразой внутри самого файла.

### Основные недостатки

**Instruction overload.** Работа Gloaguen et al. на SWE-bench и их CTXbench показала отсутствие общего улучшения task success, но более чем 20%-ный средний рост inference cost; trace analysis показал больше navigation/testing/tool activity. citeturn24view3turn24view4 Это не доказывает, что дополнительные инструкции вредны всегда: при искусственном удалении другой документации context files становились полезнее, что указывает на проблему **redundancy**, а не самого формата. citeturn24view4

**Empirical evidence остается смешанным.** Lulla et al. получили обратный operational результат — меньшие median runtime и output-token consumption с `AGENTS.md`. Их эксперимент охватывал 10 repositories и 124 PR; следовательно, нельзя переносить ни их экономию, ни cost increase из другого исследования на любой repo без canary. citeturn24view5

**Противоречивые правила.** Palo Alto Networks file содержит конфликтующие formatting/type-tooling signals в разных частях документа; Leemons одновременно продвигает “avoid classes” и classes for data validation. citeturn3view2 fileciteturn22file0L2-L6 LLM может “следовать инструкциям” и все равно выбрать одну сторону конфликта непредсказуемо. Это аргумент за automated contradiction review, а не просто за сокращение текста.

**Stale platform claims.** `.cursorrules` у Sentry и Leemons — реальные исторические owner files, но текущая официальная документация Cursor описывает version-controlled project rules прежде всего как `.cursor/rules/*.mdc`, scoped path patterns/manual/relevance. citeturn21search12 Поэтому старые `.cursorrules` полезны как evidence of adoption, но **не как текущий format recommendation без миграционной проверки**.

**Правила без executable checks.** Panfactum root-файл содержит только readability/testability/correctness. fileciteturn2file0L2-L6 Это может быть сознательным thin root с другими mechanisms elsewhere, но взятый изолированно он не объясняет, какую команду запустить и какое наблюдаемое состояние означает “testable” или “correct”.

**Private/public coupling.** Ultracopier публично содержит workstation-specific `/mnt/data/perso/...` path. fileciteturn3file0L2-L6 Это не доказательство утечки секрета, но это объективно private-environment identifier и зависимость, которую внешний contributor воспроизвести не может. Аналогично rules вида `Reported-by: <name>` безопасны только если источник имени уже публичен или публикация согласована; перенос private ticket reporter в публичный commit может раскрыть идентификатор. Последнее — **риск/вывод**, а не утверждение, что Palo Alto реально допустила такую утечку.

**Unsafe automatic external communication** в восьми основных файлах я как обязательный универсальный workflow не обнаружил. Именно поэтому предложение “при блокере автоматически открыть issue / отправить email / написать в Slack / залить log на внешний сервис” я бы считал ухудшением. Такое действие должно требовать explicit user request или отдельно настроенной, auditable automation policy.

## Практические улучшения с before/after

Ниже — не переписывание какого-либо частного документа пользователя: **в запросе нет private source document, и его содержание я не предполагаю**. Это шаблоны улучшений, выведенные из наблюдаемой практики.

| Улучшение | До | После |
|---|---|---|
| **Явный scope и precedence** | `Follow backend conventions.` | `Scope: src/backend/**. This root file applies repository-wide. If a nested AGENTS.md exists for the file being changed, its local rules take precedence for that subtree.` Это совпадает с моделью nested guidance Codex и практикой Shopware. citeturn24view0 fileciteturn29file0L2-L6 |
| **Команда вместо пожелания** | `Run the tests before finishing.` | `Before handoff run the repository's canonical test target. Record command, exit code, and skipped checks. Do not claim "tests pass" for checks that were not executed.` Реальные Gramps/Shopware/Sentry files показывают ценность exact commands. fileciteturn26file0L2-L6 fileciteturn29file0L2-L6 |
| **Generated-source contract** | `Do not edit generated files.` | `Generated outputs are not source of truth. Change the schema/source, run the documented generator, then verify that a second generation produces no diff. Never hand-edit generated output unless explicitly documented.` Подход отражает OpenBlob/Simply patterns. citeturn14search22 fileciteturn6file0L2-L6 |
| **Hard security rule вынести из prompt-only** | `Never send secrets outside the repo.` | `Do not initiate network uploads or external messages unless explicitly authorized. Enforce prohibited operations with tool permissions/hooks; this Markdown is guidance, not the security boundary.` Anthropic прямо различает context и enforced hook policy. citeturn24view1 |
| **Убрать machine-specific identifiers** | `Use /mnt/data/alice/company/project-history.` | `Use repository-relative paths. Public instructions MUST NOT contain personal home paths, internal hostnames, private ticket URLs, credentials, customer identifiers, or private communication handles.` Ultracopier показывает, как абсолютный локальный путь может просочиться в public context. fileciteturn3file0L2-L6 |
| **Observed completion criteria** | `Make sure the change is correct.` | `Completion evidence: required checks executed; exit status recorded; generated outputs clean; changed files listed; any unrun check explicitly marked NOT RUN with reason.` Это превращает instruction в audit-friendly contract. Shopware — хороший существующий пример. fileciteturn29file0L2-L6 |
| **Ownership для самого instruction file** | `Keep this file updated.` | `Changes to agent instructions require normal code review and the designated CODEOWNERS approval. Agents may propose changes but may not self-approve policy changes.` Это **рекомендация**, поскольку в выборке ownership самого файла выражен слабее, чем test/PR rules. |
| **Pointers вместо дублирования** | Корневой файл повторяет всю architecture документацию и API reference. | Root содержит только non-obvious invariants, canonical commands и ссылки на version-controlled detailed docs/nested rules. Это снижает redundancy, но **не вводит произвольный max-lines limit**. Empirical work связывает часть проблемы с redundancy и не обнаруживает length→success correlation. citeturn24view4 |
| **Expiry/evidence для нестабильных фактов** | `We support tool X/version Y.` | `Canonical source: package.json / CI matrix / toolchain file. Do not duplicate exact version here unless the value is automatically checked; when duplicated, name the source of truth.` Это уменьшает риск stale platform claims, особенно в долгоживущих `.cursorrules`. Текущая Cursor documentation уже предпочитает `.cursor/rules/*.mdc`. citeturn21search12 |

Полезно также добавить machine-checkable linter для файла инструкций: проверять broken relative paths, существование referenced scripts, duplicate/conflicting “MUST/NEVER” statements там, где это возможно, наличие license/public-export marker и отсутствие denylisted private path patterns. Такой linter не доказывает semantic correctness, но убирает класс дешевых ошибок до того, как файл попадает в prompt агента.

## Публикация и обратимое канареечное внедрение

**Рекомендация: source of truth — owned repository.** Для multi-agent проекта наиболее переносимым каноническим артефактом сегодня выглядит `AGENTS.md`, потому что Codex нативно определяет его discovery/precedence, Claude Code с v2.1.277 умеет читать его непосредственно, а GitHub Copilot относит `AGENTS.md`, `CLAUDE.md` и `GEMINI.md` к agent instruction files. citeturn24view0turn24view2turn24view6 Это не означает, что один файл автоматически одинаково ведет себя в каждом клиенте: Copilot прямо предупреждает, что support зависит от feature/surface. citeturn24view6

Практичная структура:

```text
repo/
├── AGENTS.md                    # canonical cross-agent project contract
├── CLAUDE.md                    # only if Claude-specific delta/import is necessary
├── GEMINI.md                    # only if Gemini-specific delta is necessary
├── .cursor/
│   └── rules/
│       ├── core.mdc             # Cursor-specific scope/routing
│       └── frontend.mdc
├── backend/
│   └── AGENTS.md                # only backend-specific exceptions
├── frontend/
│   └── AGENTS.md
├── docs/
│   └── agent-development.md     # long explanatory material
└── .github/
    └── CODEOWNERS               # ownership of instruction changes
```

Не надо автоматически поддерживать четыре полноразмерных, почти одинаковых копии. Дублирование создает independent drift и contradiction surface. Для Claude особенно важно проверить выбранный loading mode: при coexistence `CLAUDE.md` может по default вытеснить project `AGENTS.md`, если не используется import/configuration, поэтому простое создание обоих файлов не гарантирует “оба загрузятся”. citeturn24view2

**Public export должен строиться по allowlist, а не как verbatim копия private operations document.** Даже если внутри организации существует более богатый private file, public artifact разумно формировать только из явно разрешенных категорий:

```yaml
public_export_allowlist:
  - repository_scope
  - supported_toolchain
  - reproducible_setup_commands
  - test_and_lint_commands
  - generated_file_policy
  - coding_invariants
  - contribution_workflow
  - public_documentation_links
  - completion_evidence

deny:
  - credentials
  - internal_hostnames
  - employee_or_customer_identifiers
  - private_ticket_urls
  - local_absolute_paths
  - incident_procedures
  - production_access_details
  - private_chat_or_email_destinations
```

Это именно **publication design recommendation**, а не описание несуществующего private source: в исходном запросе такого документа нет.

Для discovery сначала используйте GitHub repository topics. GitHub документирует topics именно как способ помочь другим найти проект; repository admin добавляет их через **About → Edit repository metadata → Topics → Save changes**. citeturn23view0 Не стоит утверждать существование какого-либо конкретного curated topic, не проверив его; можно использовать релевантные lowercase project topics, а для предложения featured topic существует `https://github.com/github/explore`. citeturn23view0

**Optional Gist** имеет смысл только как mirror/example: `https://gist.github.com/`. В header такого Gist я бы написал:

```markdown
# Mirror / example only

Canonical source:
https://github.com/OWNER/REPO/blob/main/AGENTS.md

This gist may lag behind the repository.
Use the repository version for current instructions and license terms.
```

Public Gists searchable и имеют полноценную Git history, но они отделены от code review, branch protections и scope конкретного repository, поэтому делать Gist главным source of truth хуже, чем хранить файл рядом с кодом. citeturn23view2

**Directory submissions — только после повторной проверки contribution rules в день подачи.** Для generic reusable Cursor rules подтвержденный route есть у `PatrickJS/awesome-cursorrules` через repository PR. citeturn16view5 Для reusable Claude assets/template — у `davila7/claude-code-templates` через fork/branch/test/PR. citeturn16view6 `cursor.directory` я бы пока не включал в автоматический publication pipeline именно потому, что в ходе этой проверки не удалось достаточно надежно подтвердить submission contract для standalone owner rule.

**Лицензия должна быть осознанной.** GitHub прямо объясняет: без лицензии сохраняются default copyright rights, то есть публичность репозитория сама по себе не дает другим права воспроизводить, распространять или создавать derivative works; GitHub Terms дают возможность просматривать/fork'ать репозиторий на платформе, но это не эквивалент open-source reuse license. citeturn23view1 Palo Alto Networks дает особенно наглядный реальный пример: `GEMINI.md` публично читается, но repository `LICENSE` — PolyForm Internal Use, запрещающий distribution software. fileciteturn10file0L2-L6

Поэтому при публикации собственного reusable instruction set лучше дать явный `LICENSE` и при необходимости file-level SPDX/notice. При заимствовании из чужих files сначала проверять **license scope и attribution obligations**. Не копировать публичный текст только потому, что браузер позволяет его прочитать.

**Canary adoption должен быть парным и обратимым.** Оптимальная схема — не “поставили `AGENTS.md` и субъективно стало лучше”, а зафиксированный маленький eval:

| Элемент | Практический дизайн |
|---|---|
| Baseline | Набор реальных задач на текущем production configuration без candidate instruction change. |
| Candidate | Один commit, содержащий новый/измененный instruction set; agent/model/version/tools фиксированы. |
| Tasks | Несколько классов: простой bugfix, multi-file change, тестовая задача, generated artifact, subtree-specific change, docs/config task и один security-sensitive negative case. |
| Pairing | Одна и та же задача выполняется baseline и candidate; порядок по возможности рандомизируется. |
| Functional acceptance | Tests/build/lint и task-specific assertions; нельзя заменять их субъективной оценкой текста ответа. |
| Behavioral acceptance | Правильный scope; отсутствие edits запрещенных/generated файлов; отсутствие unrequested external communication; соблюдение approval boundary. |
| Efficiency | Wall-clock, model/input/output tokens, tool calls, files opened/edited. Не считать +cost автоматически провалом, если success объективно вырос. |
| Evidence | Сохранять transcript summary, executed commands, exit status, final diff и reviewer score. |
| Independent review | Reviewer не должен быть автором candidate file; по возможности оценивает diff/result без знания baseline/candidate label. |
| Rollback | Один `git revert` instruction commit или возврат предыдущего tag; никаких migration side effects в codebase. |

Порог acceptance следует задать **до запуска**, например: zero critical trust/privacy violations; 100% выполнение обязательных deterministic checks там, где они применимы; task-success не хуже baseline в пределах заранее выбранного tolerance; cost/runtime regression принимается только при измеримом улучшении качества. Конкретные проценты нужно выбирать из стоимости вашей workload, а не выдавать за научный универсальный стандарт.

Такой canary непосредственно учитывает несовпадающие эмпирические результаты: одно исследование обнаружило >20% средний cost increase без общего success uplift, другое — снижение runtime/token use на своей выборке. citeturn24view3turn24view5 Единственный надежный способ разрешить это для конкретного проекта — paired local evaluation.

## Decision memo, отвергнутые идеи и неизвестные

**Решение.** Публиковать канонические инструкции **в owned repository**, версионировать и review'ить их как код. Для multi-agent проекта начинать с `AGENTS.md`, а vendor-specific файлы делать тонкими адаптерами/дельтами только там, где реальное loading behavior требует различий. Codex нативно поддерживает hierarchy `AGENTS.md`; современный Claude Code уже умеет `AGENTS.md`, но имеет собственные coexistence rules; Copilot поддерживает agent instruction files не на всех surfaces одинаково; Gemini имеет собственную hierarchical context system. citeturn24view0turn24view2turn24view6turn23view3

**Содержание source of truth:** scope/precedence; non-obvious invariants; canonical reproducible setup/test/lint commands; generated-file policy; completion evidence; security/external-action boundary; ссылки на более длинные docs. Subsystem-only details — nested instructions или path rules. Это согласуется и с real-world Shopware/Gramps patterns, и с vendor support for scoped files. fileciteturn29file0L2-L6 fileciteturn26file0L2-L6 citeturn24view6

**Не вводить универсальный “максимум N строк”.** Codex имеет технический byte cap, Anthropic говорит о выгоде specific/concise формулировок, но empirical study не показывает простой зависимости length→success. citeturn24view0turn24view1turn24view4 Оптимизировать надо redundancy и conflicting directives, а не число строк само по себе.

**Публиковать public export только из reviewed allowlist.** Не копировать internal operations material verbatim; удалять absolute local paths, internal domains, private identities/tickets, production procedures и destinations for automatic messages. Пример Ultracopier показывает, что даже вполне невинный local filesystem path легко становится частью public policy artifact. fileciteturn3file0L2-L6

**Discovery:** repository Topics сначала; optional public Gist как зеркало; template directories только при реальной пригодности файла как reusable template и после проверки их current contribution rules. GitHub Topics и Gists имеют документированные native discovery mechanisms. citeturn23view0turn23view2

**License:** добавить явную reuse license к собственному публичному artifact; при адаптации чужих files соблюдать их license/attribution. Public visibility не отменяет copyright. citeturn23view1

**Adoption:** только через reversible canary с baseline, paired tasks, observable checks, cost measurements, rollback commit и независимым review. Существующая научная литература пока слишком неоднородна для вывода “`AGENTS.md` всегда помогает” или “context files всегда вредят”. citeturn24view3turn24view5

**Отвергнутые идеи:** один гигантский универсальный файл для всех subtrees; четыре полностью дублирующихся `AGENTS.md`/`CLAUDE.md`/`GEMINI.md`/Cursor rules; автоматическая публикация private operations doc; hard security policy только в Markdown без technical enforcement; автоматическое создание issues/email/Slack messages только потому, что это написано в repository instruction; копирование public files без license review; вывод “короткий всегда лучше”; использование `.cursorrules` как новой default recommendation только потому, что в известных repositories еще лежат исторические файлы. Current Cursor documentation описывает modern project rules в `.cursor/rules/*.mdc`. citeturn21search12

**Неизвестные и ограничения.** У `cursor.directory` не был достаточно надежно подтвержден нынешний submission mechanism именно для standalone repository instruction file, поэтому он не вошел в shortlist как проверенный submission target. License state Leemons в GitHub metadata — `Other/NOASSERTION`, поэтому я не использую его как источник для reuse. fileciteturn24file0L2-L6 Некоторые vendor behaviors быстро меняются: особенно показателен Claude Code, где direct `AGENTS.md` support сейчас существует начиная с v2.1.277, тогда как более старые рекомендации предполагали `CLAUDE.md`/imports. citeturn24view2 Перед автоматизацией публикации или генерации wrapper files эти details надо pin'ить к конкретным supported agent versions.

**Итоговый shortlist publication routes:** canonical owned GitHub/GitLab repo → GitHub Topics / native repository discovery → optional public Gist → reusable-template PR в `awesome-cursorrules` или `claude-code-templates`, если файл действительно соответствует их scope. Форматные сайты, маркетинговые каталоги и registries не должны становиться source of truth; их роль — discovery, а не владение политикой проекта. Git history, review, license и executable evidence должны оставаться в repository владельца.

## Источники (гиперссылки из Word-экспорта того же отчёта, 22)
- https://agents.md/
- https://arxiv.org/html/2601.20404v2
- https://arxiv.org/html/2602.11988v3
- https://code.claude.com/docs/en/memory
- https://cursor.com/docs/agent/security
- https://cursor.com/docs/hooks
- https://cursor.com/docs/rules
- https://cursor.directory/
- https://cursorrules.org/
- https://developers.openai.com/codex/agent-configuration/agents-md
- https://docs.github.com/articles/licensing-a-repository
- https://docs.github.com/en/copilot/concepts/prompting-and-context/custom-instructions
- https://docs.github.com/en/get-started/writing-on-github/editing-and-sharing-content-with-gists/creating-gists
- https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics
- https://geminicli.com/docs/cli/gemini-md/
- https://github.com/PaloAltoNetworks/pan-mcp-relay/blob/main/GEMINI.md
- https://github.com/PatrickJS/awesome-cursorrules
- https://github.com/PatrickJS/awesome-cursorrules/blob/main/contributing.md
- https://github.com/davila7/claude-code-templates
- https://github.com/davila7/claude-code-templates/blob/main/CONTRIBUTING.md
- https://github.com/google-deepmind/simply/blob/main/GEMINI.md
- https://gitlab.com/lx-industries/openblob/-/blob/main/AGENTS.md?ref_type=heads
