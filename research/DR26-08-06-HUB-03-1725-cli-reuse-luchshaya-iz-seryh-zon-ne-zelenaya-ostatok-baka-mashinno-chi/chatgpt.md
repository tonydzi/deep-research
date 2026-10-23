# DR26-08-06-HUB-03-1725 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr: DR26-08-06-HUB-03-1725
vendor: OpenAI (Codex CLI)
door: cli-headless
rail: codex
cmd: «внутренний путь лаборатории»

collected: 2026-08-06T17:31:45Z
took_sec: 1099
exit: 0
sources_unique_urls: 112
note: >-
  Добыто локальным CLI по подписке (не вендорский режим Deep Research из
  веб-морды). Уровень = умная модель + веб-поиск вендора.
---

# Deep Research: оркестраторы, которые используют подписочные лимиты LLM

**Состояние рынка на 6 августа 2026 года.** Под «подпиской» ниже понимается именно фиксированный пользовательский план, а не API credits, gateway credits или pay-as-you-go.

## Главный вывод

Готового продукта, который одновременно:

1. поддерживает Anthropic Max, ChatGPT Pro, SuperGrok и Google AI Pro;
2. пользуется официальными подписочными входами;
3. видит остатки всех четырёх квот;
4. заранее маршрутизирует задачу в наиболее свободный бак;
5. работает headless на шести машинах,

**сейчас нет**.

Ближе всех:

- **OpenClaw**: лучший общий control plane, настоящий subprocess для `claude -p`, Codex и Gemini CLI runtime, cron, узлы, реактивные fallback-цепочки. Но Google AI Pro через старый Gemini CLI уже не работает, а остаток квоты пока не участвует в выборе модели.
- **Goose**: наиболее чистый способ завернуть официальные Claude Code и Codex CLI через ACP/subprocess, но слабый как централизованный планировщик шести машин.
- **Hermes Agent**: хорошие реактивные fallback-цепочки и подписочные OAuth OpenAI/xAI; не использует базовый бак Claude Max и не использует Google AI Pro.
- **Pi + pi-grok-cli**: сильная программируемая рельса для ChatGPT и SuperGrok, включая чтение квоты Grok, но не готовый инфраструктурный control plane.
- **Google Antigravity CLI**: теперь именно он расходует Google AI Pro. Это полезный отдельный исполнитель, но не универсальный мультивендорный брокер.

Следовательно, для вашей установки реалистичная архитектура — не смена Claude Code, а **тонкий диспетчер под ним**:

```text
Claude Code: цель, декомпозиция, сложное суждение, финальная приёмка
       │
       ├── Codex app-server / codex exec
       ├── claude -p
       ├── Grok Build / SuperGrok OAuth
       ├── Antigravity CLI -p
       └── deterministic shell без LLM
```

OpenClaw уже даёт большую часть транспорта. Недостающая деталь рынка — небольшой quota-aware policy layer, который читает квоты и выбирает исполнителя до запуска.

---

# 1. Сводная таблица оркестраторов

Обозначения:

- **A** — Anthropic;
- **O** — OpenAI;
- **X** — xAI;
- **G** — Google;
- **Sub-OAuth** — пользовательская подписка;
- **Device** — официальный device-code flow;
- **CLI** — запуск официального локального CLI;
- **Key** — только API key/pay-as-you-go;
- **reactive** — переключение лишь после 401/403/429/5xx;
- **proactive** — выбор до запроса по измеренному остатку бака.

| Инструмент, владелец, лицензия, релиз | Аутентификация A / O / X / G | Чужой CLI как subprocess | Квота и fallback | Headless / cron | Где ломается на практике |
|---|---|---|---|---|---|
| **[OpenClaw](https://github.com/openclaw/openclaw)**, OpenClaw Foundation / Peter Steinberger, MIT. Последний stable `2026.7.1-2`, 4.08.2026 ([releases](https://github.com/openclaw/openclaw/releases)). | **A:** API key или настоящий локальный `claude -p`, повторно использующий вход Claude Code ([документация](https://docs.openclaw.ai/providers/anthropic)). **O:** ChatGPT/Codex browser или device OAuth и API key ([документация](https://docs.openclaw.ai/providers/openai)). **X:** SuperGrok/X OAuth/device и API key ([документация](https://docs.openclaw.ai/providers/xai)). **G:** сейчас API key/Vertex; новый consumer OAuth не предлагается ([документация](https://docs.openclaw.ai/providers/google)). | **Да.** Claude запускается как реальный `claude -p`; каждый node использует собственный бинарник, логин и файлы Claude. Есть Codex app-server и Gemini CLI runtime, но Gemini runtime теперь должен работать с API key, а не с Google AI Pro OAuth. | Ротация auth profiles и fallback chain при rate limit, auth error, timeout, overload. Это **reactive**, не proactive по проценту остатка ([failover](https://docs.openclaw.ai/model-failover)). Anthropic usage windows показывает, но сам по ним модель заранее не выбирает. | Отлично: daemon gateway, nodes, automations/cron, каналы, JSON/CLI. | Требуется Claude Code ≥2.1.206 для нужной lifecycle-capability; Claude CLI должен жить на том же host. Сам проект прямо определяет один trusted-operator boundary, а не безопасный multi-tenant SaaS ([security](https://github.com/openclaw/openclaw/blob/main/docs/gateway/security/index.md)). В последних релизах отдельно чинились преждевременно завершавшиеся Codex app-server turns ([release notes](https://github.com/openclaw/openclaw/releases)). |
| **[OpenCode](https://github.com/anomalyco/opencode)**, Anomaly, MIT. Последний видимый stable `1.17.7`, 14.06.2026 ([releases](https://github.com/anomalyco/opencode/releases)). | **A:** API key. Подписочный Claude OAuth удалён: документация сообщает, что Anthropic его запрещает. **O:** ChatGPT Plus/Pro OAuth. **X:** SuperGrok OAuth/device. **G:** API key/Vertex, не consumer subscription ([providers](https://dev.opencode.ai/docs/providers)). | Нет штатного паттерна «запусти установленный Claude/Codex как агента». В основном собственный harness обращается к backend. | Переключение моделей возможно конфигом, но встроенного proactive quota router нет. | `opencode serve`, CLI, server API пригодны для automation ([CLI](https://dev.opencode.ai/docs/cli), [server](https://dev.opencode.ai/docs/server)). | OAuth-провайдеры и provider packages периодически расходятся по версиям; документация рекомендует чистить provider cache при несовместимости ([troubleshooting](https://dev.opencode.ai/docs/troubleshooting)). Claude subscription route фактически исчез после серверной блокировки. |
| **[Claude Code Router](https://github.com/musistudio/claude-code-router)**, musistudio, спонсировался Z.ai, MIT. Rolling npm/main; надёжного tagged release в публичном release feed на 6.08.2026 нет. | Claude Code остаётся интерфейсом, но реальные backends обычно требуют API keys. Есть transformers для `gemini-cli`, Qwen CLI и других CLI, однако это не общий подписочный OAuth для A/O/X/G ([README](https://github.com/musistudio/claude-code-router)). | Частично: experimental CLI transformers, особенно Gemini/Qwen. Это преобразователь запросов, а не полноценное управление чужими агентными сессиями. | Маршрутизация по task type, background, thinking и long-context. Остаток подписки не учитывается; общей cross-provider retry policy нет. | Есть non-interactive режим и примеры GitHub Actions. | Частые жалобы — model/version drift между Claude Code и transformer ([issue #1237](https://github.com/musistudio/claude-code-router/issues/1237)), конфигурационные несовместимости ([issue #774](https://github.com/musistudio/claude-code-router/issues/774)). После 18.06.2026 старый Gemini AI Pro путь через Gemini CLI больше не является рабочим подписочным баком. |
| **[Hermes Agent](https://github.com/NousResearch/hermes-agent)**, Nous Research, MIT. `v0.20.0` / `v2026.8.3`, 3.08.2026 ([releases](https://github.com/NousResearch/hermes-agent/releases)). | **A:** Anthropic OAuth существует, но документация предупреждает: это не базовый бак Pro/Max; на Max требуется extra-usage/credits. **O:** ChatGPT OAuth/device, возможен импорт Codex credentials. **X:** SuperGrok device flow. **G:** API key ([quickstart](https://hermes-agent.nousresearch.com/docs/getting-started/quickstart/), [environment variables](https://hermes-agent.nousresearch.com/docs/reference/environment-variables)). | Нет Claude CLI backend; соответствующая возможность всё ещё запрашивается пользователями ([issue #33462](https://github.com/NousResearch/hermes-agent/issues/33462)). Локальные ACP-процессы поддерживаются для отдельных интеграций. | Один из лучших reactive fallback: credential pools и переход между провайдерами на 429/5xx/auth/timeout ([fallback providers](https://hermes-agent.nousresearch.com/docs/user-guide/features/fallback-providers)). Процент остатка заранее не оптимизирует. | Да: CLI, cron, messaging gateway, background operation. | SuperGrok OAuth на некоторых тарифах возвращал 403 ([issue #26847](https://github.com/NousResearch/hermes-agent/issues/26847)); сами авторы публикуют отдельные предупреждения о rollout/account-tier differences ([xAI guide](https://hermes-agent.nousresearch.com/docs/guides/xai-grok-oauth)). |
| **[Goose](https://github.com/aaif-goose/goose)**, изначально Block, теперь Agentic AI Foundation, Apache-2.0. `v1.35.0`, 22.05.2026 ([releases](https://github.com/aaif-goose/goose/releases)). | **A:** установленный Claude Code через CLI/ACP, значит бак подписки обслуживает официальный бинарник. **O:** Codex CLI/ACP и его ChatGPT subscription. **X:** только API-compatible provider; поддержанного SuperGrok consumer flow не найдено. **G:** Gemini CLI/ACP исторически, но с 18.06.2026 Google AI Pro перенесён в Antigravity, поэтому прежний бак больше не расходуется ([CLI providers](https://goose-docs.ai/docs/guides/cli-providers/), [ACP providers](https://goose-docs.ai/docs/guides/acp-providers/)). | **Да.** Это один из наиболее чистых примеров запуска официального Claude/Codex/Gemini agent как дочернего процесса через ACP. | Нет proactive quota routing и общей цепочки между подписочными провайдерами. | Да: `goose run`, recipes, schedules; пригоден для scripts/CI. | ACP зависит от внешнего бинарника и версии протокола. Документированные ограничения — отсутствие resume/fork в ряде ACP backends; в релизах исправлялись headless approvals, child-process races и provider schemas ([releases](https://github.com/aaif-goose/goose/releases)). |
| **[Aider](https://github.com/Aider-AI/aider)**, Paul Gauthier/Aider AI, Apache-2.0. Последний stable `v0.86.0`, 9.08.2025 ([releases](https://github.com/Aider-AI/aider/releases)). | A/O/X/G — API keys или OpenAI-compatible gateways. Авторы отдельно поясняют, что ChatGPT subscription не является API access ([API keys](https://aider.chat/docs/config/api-keys.html), [installation notes](https://aider.chat/docs/install/optional.html)). | Нет. | Нет подписочного quota router; API rate-limit error возвращается пользователю. | Да, `--message`/`--yes-always` позволяют один headless turn ([options](https://aider.chat/docs/config/options.html)). | Это хороший coding client, но не multi-agent subscription control plane. Главная поломка для вашей задачи структурная: все четыре ваши уже оплаченные потребительские бака он не использует. |
| **[Continue](https://github.com/continuedev/continue)**, Continue Dev, Apache-2.0. Последний stable VS Code tag `v2.0.0`, 19.06.2026; `v2.1.0` pre-release ([releases](https://github.com/continuedev/continue/releases)). | В основном Continue account, API keys и локальные модели; поддержанных consumer subscriptions A/O/X/G как общего backend нет ([documentation](https://docs.continue.dev/)). | Нет общего subprocess-паттерна. | Нет quota-aware routing. | Есть CLI/agent режимы, но экосистема ориентирована прежде всего на IDE. | Репозиторий и продуктовая линия переживали заметную смену архитектуры; как подписочный диспетчер не подходит даже при исправной установке. |
| **[Cline](https://github.com/cline/cline)**, Cline Bot Inc., Apache-2.0. Последний публичный tagged CLI release `v3.0.24`, 11.06.2026 ([releases](https://github.com/cline/cline/releases)). | **O:** `openai-codex` позволяет вход ChatGPT subscription. **A/G/X:** direct API keys/gateways ([CLI README](https://github.com/cline/cline/blob/main/apps/cli/README.md)). | Нет: Codex subscription интегрирована в собственный harness, а не запускается как независимый Codex agent. | Нет измеренного quota scheduler; выбор provider/model ручной. | Да: CLI, `--json`, automation/CI. | Текущий changelog содержит исправления timeout, authentication catalog и неверного usage accounting ([changelog](https://github.com/cline/cline/blob/main/apps/cli/CHANGELOG.md)). |
| **[Roo Code](https://github.com/RooCodeInc/Roo-Code)**, Roo Code/community, Apache-2.0. Архивирован 15.05.2026; последний `v3.54.0` ([releases](https://github.com/RooCodeInc/Roo-Code/releases)). | Исторический direct Claude OAuth перестал работать 9.01.2026: Anthropic отвечал, что credential разрешён только Claude Code ([issue #10566](https://github.com/RooCodeInc/Roo-Code/issues/10566)). Оставались API providers. | Нет поддержанного current subprocess backend. | Нет. | IDE extension; надёжного cron/headless control plane нет. | Проект read-only. Даже до архива встречались provider hangs ([issue #5097](https://github.com/RooCodeInc/Roo-Code/issues/5097)). |
| **[OpenHands](https://github.com/OpenHands/OpenHands)**, All Hands AI, MIT core. `1.8.0`, 10.06.2026 ([releases](https://github.com/OpenHands/OpenHands/releases)). | Vendor API keys или OpenHands Cloud credentials. Его device flow авторизует OpenHands Cloud, а не подписки A/O/X/G ([Cloud CLI](https://docs.openhands.dev/openhands/usage/cli/cloud)). | Нет штатного consumer-CLI reuse. | Есть model/provider configuration, но нет subscription quota broker. | Хорошо: `--headless`, контейнерные runtimes, CLI ([quick start](https://docs.openhands.dev/openhands/usage/cli/quick-start)). | Тяжёлый runtime/container layer; consumer subscriptions остаются неиспользованными. |
| **[Zed](https://github.com/zed-industries/zed)**, Zed Industries, GPL-3.0/Apache-2.0 mix. `v1.6.3`, 10.06.2026 ([releases](https://github.com/zed-industries/zed/releases)). | **A:** Claude Pro/Max через внешний Claude Agent/ACP. **O:** официальный ChatGPT Plus/Pro sign-in. **G:** внешний Gemini CLI agent исторически, но Google AI Pro больше не обслуживается старым CLI. **X:** consumer subscription не поддерживается ([existing subscriptions](https://zed.dev/docs/ai/use-an-existing-subscription.html), [configuration](https://zed.dev/docs/ai/configuration)). | Да для external ACP agents. | Нет quota broker; пользователь выбирает agent/thread. | Нет: это GUI/editor, не cron control plane. | ACP/backend schema drift. В релизах регулярно исправляются Gemini/OpenAI compatibility errors ([releases](https://github.com/zed-industries/zed/releases)). |
| **[Crush](https://github.com/charmbracelet/crush)**, Charmbracelet, FSL-1.1-MIT. `v0.74.1`, 29.05.2026 ([releases](https://github.com/charmbracelet/crush/releases)). | A/O/X/G в основном API keys. Подписочные планы поддерживаются у отдельных других providers, но не у вашей четвёрки. Claude OAuth остаётся запросом пользователей, не поддержанным каналом ([issue #457](https://github.com/charmbracelet/crush/issues/457)). | Нет. | Нет quota router/fallback по consumer buckets. | Есть `crush run`; client-server остаётся экспериментальным. | В релизах исправлялись hangs shell, non-interactive reliability и provider schema incompatibility ([releases](https://github.com/charmbracelet/crush/releases)). |
| **[Amp](https://ampcode.com/)**, Amp Inc., proprietary; отделился от Sourcegraph. Rolling SaaS/CLI, последнее крупное подписочное обновление 18.07.2026 ([announcement](https://ampcode.com/news/subscriptions)). | Поддерживает привязку **ChatGPT subscription** и **X Premium+/SuperGrok**. Anthropic и Google обслуживаются через собственные Amp plans/credits, не ваши consumer buckets ([manual](https://ampcode.com/manual), [subscriptions](https://ampcode.com/news/subscriptions)). | Нет: inference авторизуется инфраструктурой Amp. | Внутренний model routing есть, но пользователь не получает quota-aware контроль над четырьмя своими подписками. | Да: `amp --execute --stream-json` ([manual appendix](https://ampcode.com/manual/appendix)). | Proprietary cloud boundary и зависимость от Amp control plane. Consumer/provider terms всё равно применяются, что Amp отдельно отмечает ([security](https://ampcode.com/security)). |
| **[Kilo Code](https://github.com/Kilo-Org/kilocode)**, Kilo Code, MIT ([LICENSE](https://raw.githubusercontent.com/Kilo-Org/kilocode/main/LICENSE)). Последний release feed — `v7.4.x`, июль 2026 ([releases](https://github.com/Kilo-Org/kilocode/releases)). | **O:** ChatGPT Plus/Pro OAuth. **X:** SuperGrok/X OAuth/device. **A/G:** API key/gateway ([authentication](https://kilo.ai/docs/getting-started/setup-authentication), [xAI](https://kilo.ai/docs/ai-providers/xai)). | Нет. | Нет proactive quota routing. | Да: `kilo run --auto`, `serve`, CI ([CLI](https://kilo.ai/docs/code-with-ai/platforms/cli), [repository](https://github.com/Kilo-Org/kilocode)). | xAI refresh-token rotation может инвалидировать вход другого процесса, что особенно неприятно на шести машинах; это отмечено в xAI provider docs. |
| **[Warp / Oz](https://www.warp.dev/)**, Warp Inc. Warp terminal client AGPL, Oz/cloud proprietary. Rolling SaaS; multi-harness orchestration объявлена 19.05.2026 ([announcement](https://www.warp.dev/blog/multi-harness-cloud-agent-orchestration)). | Может размещать Claude Code, Codex и другие harnesses, а также локальные CLI-сессии. В Oz cloud собственные credits/BYO inference — не гарантированное использование ваших consumer subscriptions ([Oz](https://www.warp.dev/blog/oz-orchestration-platform-cloud-agents), [universal agents](https://www.warp.dev/blog/universal-agent-support-level-up-coding-agent-warp)). Google AI Pro через старый Gemini CLI после 18.06.2026 не работает. | Да для локальных/native agent sessions; в cloud запускаются harnesses. | Нет публичного proactive router по остаткам ваших четырёх баков. | Сильный headless/API/schedules/cloud orchestration. | Смешение AI credits, compute и harness costs усложняет доказательство, какой именно оплаченный бак был использован. Cloud и control plane остаются proprietary, несмотря на открытие terminal client ([open-source announcement](https://www.warp.dev/blog/warp-is-now-open-source)). |
| **[Pi](https://github.com/earendil-works/pi)**, Earendil Works, MIT. `v0.84.0`, 6.08.2026 ([releases](https://github.com/earendil-works/pi/releases)). | Базовый Pi поддерживает ChatGPT subscription и несколько OAuth providers. Community extension **pi-grok-cli** добавляет SuperGrok/X OAuth/device, quota reading и account rotation ([providers](https://pi.dev/docs/latest/providers), [pi-grok-cli](https://github.com/kenryu42/pi-grok-cli)). Direct Anthropic OAuth юридически хрупок; Google consumer subscription сейчас относится к Antigravity, не к обычному Pi provider. | Обычно нет: собственный harness ходит к provider backend. | Сам Pi extensible, но готового cross-vendor quota policy нет. `pi-grok-cli` умеет читать Grok allowance и переключать собственные авторизованные аккаунты после исчерпания. | Отлично: print/JSON/RPC/remote-session APIs. | Extensions зависят от provider protocol. Релиз `0.84.0` содержит breaking changes в stream events, auth refresh interfaces и session APIs ([release notes](https://github.com/earendil-works/pi/releases)). |
| **[Vibe Kanban](https://github.com/BloopAI/vibe-kanban)**, BloopAI, Apache-2.0. `v0.1.44`, 24.04.2026; проект объявлен sunset ([releases](https://github.com/BloopAI/vibe-kanban/releases)). | Запускал установленные Claude Code, Codex, Gemini CLI, Amp и другие agents после локального входа. | Да. | Нет quota-aware router или automatic fallback. | В основном UI/worktree manager; слаб для cron. | Проект закрывается. Gemini subscription route дополнительно устарел после переноса Google AI Pro в Antigravity. |
| **[Google Antigravity CLI](https://github.com/google-antigravity/antigravity-cli)**, Google, proprietary client/control plane. Текущая линия `1.1.x`; changelog на 6.08.2026 показывает `1.1.5` ([changelog](https://github.com/google-antigravity/antigravity-cli/blob/main/CHANGELOG.md)). | **G:** официальный Google AI Pro/Ultra OAuth. A/O-модели могут появляться как модели внутри Google Ultra, но это расход Google quota, а не ваши Anthropic/OpenAI subscriptions. X subscription нет ([plans](https://antigravity.google/docs/plans?app=cli)). | Нет чужих consumer CLI. | Есть Google model-specific quotas, встроенные subagents и client retries; не маршрутизирует между вашими четырьмя счетами. | Да: `agy -p/--print`, scheduled tasks, background agents. | Ранние headless версии теряли stdout в non-TTY ([issue #76](https://github.com/google-antigravity/antigravity-cli/issues/76)); встречались MCP discovery/invocation расхождения ([issue #71](https://github.com/google-antigravity/antigravity-cli/issues/71)). |

## Что действительно подходит вашей задаче

### Первая группа: реально может сжигать уже оплаченные баки

1. **OpenClaw**
   - Claude Max через `claude -p`;
   - ChatGPT Pro через Codex OAuth/app-server;
   - SuperGrok через device OAuth;
   - Google AI Pro — **нет**, после перехода на Antigravity.
   - Лучший кандидат на transport/control plane.

2. **Goose**
   - Claude Max через официальный Claude agent;
   - ChatGPT Pro через Codex agent;
   - Google AI Pro route устарел;
   - SuperGrok subscription отсутствует.

3. **Hermes**
   - ChatGPT и SuperGrok;
   - не базовый Claude Max;
   - не Google AI Pro;
   - лучшие готовые reactive fallbacks.

4. **Pi + pi-grok-cli**
   - ChatGPT и SuperGrok;
   - Grok quota observability;
   - хорош как отдельная программируемая worker rail.

5. **Antigravity CLI**
   - единственная актуальная first-party рельса для Google AI Pro после 18 июня 2026 года. Google официально объявила, что AI Pro/Ultra/free перестают обслуживаться старым Gemini CLI и переходят в Antigravity ([объявление Google](https://github.com/google-gemini/gemini-cli/discussions/27274)).

### Вторая группа: выглядит мультимодельной, но ваши подписки не расходует

Aider, Continue, OpenHands, Crush и в значительной степени Claude Code Router работают через API keys, gateways или собственные credits. Для задачи «утилизировать уже оплаченные $720–740» это другой класс продукта.

### Отрицательный результат

Я не нашёл ни одного стабильного продукта, который использует **остаток всех подписочных квот как routing feature**. OpenClaw и Hermes реагируют на отказ. Antigravity, Claude Code, Codex и Grok показывают квоты. Но центрального оптимизатора вида:

```text
если Claude 7d > 70%, Codex < 40%, Grok < 30%:
    bounded coding → Codex
    web research → Grok
    ambiguous architecture → Claude
```

в готовом виде нет.

---

# 2. Юридическая и ToS-карта

Это техническая оценка договорного риска, не юридическое заключение.

## 2.1 Anthropic: что прямо написано сейчас

Для Free/Pro/Max действует Consumer Terms; для API/Team/Enterprise — Commercial Terms. Это прямо разведено в [Claude Code Legal and Compliance](https://code.claude.com/docs/en/legal-and-compliance).

На странице Claude Code Anthropic одновременно говорит:

- лимиты Pro/Max рассчитаны на **“ordinary, individual usage”**;
- Anthropic **“does not permit third-party developers … to route requests through Free, Pro, or Max plan credentials on behalf of their users”**;
- меры enforcement могут применяться без предварительного уведомления.  
  Источник: [Claude Code authentication and credential use, актуально 6.08.2026](https://code.claude.com/docs/en/legal-and-compliance).

В [Consumer Terms](https://www.anthropic.com/legal/consumer-terms) дополнительно запрещено:

- передавать кому-либо логин, API key или account credentials;
- автоматизированно обращаться к сервису, кроме случаев API key или явного разрешения Anthropic;
- обходить защитные механизмы.

Но `claude -p` является именно таким явно документированным исключением: Anthropic официально поддерживает programmatic/headless Claude Code и даже предлагает долгоживущий `CLAUDE_CODE_OAUTH_TOKEN`, создаваемый `claude setup-token`, для CI и scripts ([authentication documentation](https://code.claude.com/docs/en/authentication)).

Ещё важнее: 16 июня 2026 года Anthropic обновила help article. Запланированное отделение Agent SDK от subscription bucket было поставлено на паузу; **“For now, nothing has changed”** — `claude -p`, Agent SDK и third-party app usage пока продолжают расходовать подписочные лимиты ([официальное обновление Anthropic](https://support.claude.com/en/articles/15036540-use-the-claude-agent-sdk-with-your-claude-plan)).

### Следствие

`claude -p` в автоматизации сегодня:

- технически поддерживается;
- официально расходует подписку;
- не является сам по себе «взломом OAuth»;
- но не даёт carte blanche на multi-user gateway или продукт, обслуживающий чужих пользователей.

## 2.2 Чем subprocess отличается от встроенного OAuth

### Вариант A: оркестратор запускает официальный CLI

```text
orchestrator → claude -p → Anthropic
```

В этом случае:

- OAuth client, refresh, request format, beta headers и telemetry контролируются Anthropic;
- Anthropic может изменить протокол, и официальный CLI обновится вместе с ним;
- оркестратор не получает необходимости притворяться Claude Code;
- вызов идёт через документированный non-interactive surface;
- Anthropic видит session type `sdk-cli`/programmatic, а не принимает его за ручной TUI. OpenClaw сам описывает такие session records в [Anthropic provider documentation](https://docs.openclaw.ai/providers/anthropic).

Это **существенно ниже по риску**, чем прямое встраивание consumer OAuth.

### Вариант B: сторонний harness получает OAuth и сам вызывает consumer backend

```text
orchestrator → refresh token → внутренний Claude backend
```

Тогда сторонний продукт:

- использует Anthropic OAuth client/credential вне родного приложения;
- формирует запросы самостоятельно;
- может направлять запросы за других пользователей;
- попадает прямо под запрет third-party routing subscription credentials.

Именно этот путь серверно сломался у Roo Code 9 января 2026 года: Anthropic вернул сообщение, что credential разрешён только для Claude Code ([Roo issue #10566](https://github.com/RooCodeInc/Roo-Code/issues/10566)). OpenCode после этого удалил соответствующие плагины и теперь прямо предупреждает о запрете в [provider docs](https://dev.opencode.ai/docs/providers).

## 2.3 Карта риска Anthropic

| Паттерн | Риск | Почему |
|---|---:|---|
| Владелец лично использует интерактивный Claude Code | Низкий | Нативный сценарий подписки. |
| Владелец запускает официальный `claude -p` из собственного script/cron | Низкий–средний | Programmatic mode документирован и сейчас расходует subscription; остаётся предел «ordinary, individual usage» и право изменить правила. |
| Внутренний OpenClaw одного владельца запускает локальный `claude -p` | Средний | Технически это официальный CLI, но юридическая формулировка про third-party app не идеально определяет границу между personal tooling и third-party service. |
| Один Claude Max account обслуживает владельца и двух ассистентов | Высокий | Consumer Terms запрещают делиться account credentials или делать account доступным другим. |
| Сторонний harness импортирует OAuth token и сам обращается к backend | Высокий | Прямо запрещённая зона third-party routing; уже есть серверная блокировка credential class. |
| Multi-tenant gateway или продукт для внешних пользователей на одном Max | Очень высокий | Явное “on behalf of their users”, credential sharing и production automation. |
| Ротация нескольких Max accounts ради обхода limits | Очень высокий | Это уже похоже на обход technical limitations, а не нормальное использование. |

## 2.4 OpenAI: текущие условия

Для UK/EEA действуют [Europe Terms of Use, обновлены 16.01.2026](https://openai.com/policies/eu-terms-of-use/). Там прямо сказано:

- **“You may not share your account credentials or make your account available to anyone else”**;
- запрещено **“circumventing any rate limits or restrictions”**;
- при нарушениях доступ может быть ограничен или аккаунт приостановлен.

Для API и business/developer products действует [OpenAI Services Agreement, effective 1.01.2026](https://openai.com/policies/services-agreement/). Он разрешает интеграцию API в Customer Applications, но отдельно запрещает:

- делиться individual login credentials;
- настраивать сервис для avoidance/circumvention Usage Limits;
- извлекать данные иначе, чем разрешено интерфейсом сервиса.

При этом OpenAI сама:

- включает Codex в подписки ChatGPT Plus/Pro и другие планы ([официальный Help Center](https://help.openai.com/en/articles/11369540-using-codex-with-your-chatgpt-plan));
- открыла официальный `codex app-server`, предназначенный для внешних клиентов;
- поддерживает browser/device login, refresh-token lifecycle и методы чтения rate limits ([официальный app-server protocol](https://github.com/openai/codex/blob/main/codex-rs/app-server/README.md));
- публично поддержала использование ChatGPT subscription в стороннем harness Zed ([Zed announcement](https://zed.dev/blog/chatgpt-subscription-in-zed)).

Поэтому вызов официального `codex exec` или `codex app-server` самим владельцем — значительно более защищённый сценарий, чем копирование refresh token и самостоятельная эмуляция закрытого ChatGPT backend.

## 2.5 Карта риска OpenAI

| Паттерн | Риск | Почему |
|---|---:|---|
| Владелец использует Codex CLI/app-server с ChatGPT Pro | Низкий | Это официальный публично документированный interface. |
| Внутренний оркестратор запускает `codex exec` или app-server | Низкий | Именно app-server предназначен для внешних клиентов. |
| Сторонний инструмент реализует официальный ChatGPT/Codex OAuth flow, одобренный OpenAI | Низкий–средний | Уже есть несколько публично поддерживаемых интеграций, но consumer terms продолжают действовать. |
| Копирование refresh token и прямой вызов внутреннего consumer backend | Средний–высокий | Выход за документированный interface, больший риск enforcement. |
| Один Pro account используется владельцем и ассистентами | Высокий | Прямой запрет account sharing. |
| Несколько accounts вращаются для обхода лимитов | Очень высокий | Прямо попадает в rate-limit circumvention. |

## 2.6 Известные случаи блокировок

### Anthropic

- Пользователь OpenCode утверждал, что аккаунт был заблокирован после использования стороннего Anthropic OAuth ([OpenCode issue #6930](https://github.com/anomalyco/opencode/issues/6930)). Это пользовательское свидетельство, не официальный отчёт Anthropic.
- Roo Code получил воспроизводимый серверный запрет на Claude Code credential вне Claude Code ([issue #10566](https://github.com/RooCodeInc/Roo-Code/issues/10566)). Это наиболее сильное техническое доказательство enforcement.
- Создатель OpenClaw Peter Steinberger временно терял доступ к Claude; случай описан [TechCrunch](https://techcrunch.com/2026/04/10/anthropic-temporarily-banned-openclaws-creator-from-accessing-claude/). Публичных данных недостаточно, чтобы доказать, что причиной был именно subprocess-паттерн, поэтому связывать эти события как установленный факт нельзя.

Я не нашёл публичного подтверждённого случая, где Anthropic забанила владельца исключительно за нормальный внутренний запуск официального `claude -p`.

### OpenAI

Я не нашёл подтверждённого публичного случая блокировки за запуск официального `codex exec` или `codex app-server` как subprocess. Это отрицательный результат поиска, а не гарантия отсутствия непубличных enforcement cases.

### Google — важное дополнительное предупреждение

Хотя вопрос юридически фокусировался на Anthropic/OpenAI, у Google риск уже материализовался. В феврале 2026 года Google признала волну банов за third-party tools/proxies, обращавшиеся к Antigravity quota; блокировка затрагивала также Gemini CLI и Code Assist, потому что enforcement находился в общем backend layer ([официальное объявление Google](https://github.com/google-gemini/gemini-cli/discussions/20632)).

Google Gemini CLI FAQ отдельно считает harvesting/piggybacking OAuth сторонним инструментом нарушением и рекомендует API key ([официальный FAQ](https://github.com/google-gemini/gemini-cli/blob/main/docs/resources/faq.md)). Поэтому проксирование Antigravity OAuth в OpenClaw или Claude Code делать не следует. Безопасный вариант — запускать сам официальный `agy -p`.

## 2.7 Может ли вендор отличить subprocess от человека

Да.

Даже если запросы уходят через официальный бинарник, вендор может видеть:

- OAuth client id и тип grant;
- версию бинарника;
- interactive против `-p`/SDK session type;
- telemetry и session metadata;
- system prompt и набор tools;
- частоту, круглосуточность и параллелизм;
- количество hosts/IP/OS на один account;
- характерные retry bursts;
- несколько одновременных refresh-token sessions;
- отсутствие человеческих пауз;
- паттерн исчерпания quota и немедленного перехода к другому account.

Разница в том, что при subprocess эти признаки **не маскируются**: официальный CLI честно сообщает programmatic execution. Это лучше с точки зрения ToS, но не делает automation невидимой.

---

# 3. Как распределять нагрузку между вендорами

## 3.1 Что удалось найти у крупных пользователей

Я не нашёл опубликованного production-разбора, где крупная команда маршрутизирует именно **четыре consumer subscriptions** Anthropic/OpenAI/xAI/Google по остатку фиксированных баков. Публичные model-routing системы почти всегда работают с API endpoints и оптимизируют цену вызова, а не уже оплаченную месячную ёмкость.

Ближайшие релевантные источники:

1. **IBM Research, “Model routing is simple until it isn’t”, 15.07.2026.**  
   На 417 AppWorld tasks один и тот же CodeAct agent сравнивался на разных моделях. Авторы показывают, что sticker price недостаточен: caching и различия в количестве шагов переворачивают экономику. Маршрутизатор должен одновременно учитывать качество, фактическую стоимость траектории, latency и reliability ([IBM Research](https://huggingface.co/blog/ibm-research/model-routing-is-simple-until-it-isnt)).

2. **TwinRouterBench, май 2026.**  
   Исследование маршрутизирует не целые задачи, а отдельные agent steps. На 520 coding instances обученный router получил результат, сопоставимый с сильнейшей моделью, при снижении реализованной стоимости примерно на 53%. Ключевой вывод: один ошибочно перенесённый критический шаг может провалить всю траекторию ([paper](https://arxiv.org/abs/2605.18859)).

3. **Agent-as-a-Router, июнь 2026.**  
   Маршрутизация улучшается, если копить performance statistics по измерениям задачи, а не только анализировать prompt. Авторы сообщают относительное улучшение routing quality на 15,3% от task-dimension feedback ([paper](https://arxiv.org/abs/2606.22902)).

4. **Anthropic multi-agent research system.**  
   Внутренний подход использовал Opus как lead agent и Sonnet workers; на research eval система была на 90,2% лучше single-agent baseline. Но multi-agent режим расходовал примерно в 15 раз больше токенов, чем обычный chat, и хуже подходил для тесно связанных coding tasks ([Anthropic Engineering](https://www.anthropic.com/engineering/multi-agent-research-system)).

5. **Anthropic eval methodology.**  
   Рекомендуется измерять task, trial, grader, trajectory и реальное outcome state, а не оценивать качество по красивому тексту ответа. Для стохастических агентов нужны несколько trials ([Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)).

6. **Logicbric production routing case.**  
   Публичная таксономия делит работу на classification, extraction, generation, reasoning и orchestration; контролируются quality, cost и latency по классам. Это vendor case study, а не независимое академическое доказательство, но сама структура практична ([Logicbric](https://www.logicbric.com/insights/multi-model-routing/)).

## 3.2 Рабочая таксономия для вашей установки

Это не универсальная истина, а стартовая гипотеза, которую нужно проверить на ваших задачах.

| Класс | Примеры из ваших 36,8 млн output tokens | Первая рельса | Когда возвращать Claude |
|---|---|---|---|
| **L0: детерминированная механика** | `git status`, поиск файлов, SQL, подсчёт, grep, дедуп, parsing JSON, получение headers | Shell/SQL/локальный код, без LLM | Никогда, пока операция формально определена |
| **L1: извлечение и преобразование** | прочитать N файлов, извлечь поля, классифицировать, преобразовать формат, кратко свести tool output | Codex/Grok/Antigravity, иногда локальная модель | Если нет формальной схемы или результат неоднозначен |
| **L2: ограниченная кодовая задача** | написать тест, исправить локальную функцию, мигрировать API, bounded refactor | Codex первым; Antigravity/Grok как canary/second rail | Если тесты не проходят после ограниченного числа попыток |
| **L3: широкий агентный анализ** | расследование cross-repo бага, чтение большого codebase, план миграции | Codex или Claude, сравнивать на собственном benchmark | При неполной картине системы, противоречивых требованиях |
| **L4: frontier judgment** | архитектурная развилка, безопасность, юридические риски, итоговый synthesis | Claude остаётся основной рельсой | Не переносить без сильного eval evidence |
| **Специализированные задачи** | live web/recent discourse; длинный мультимодальный контекст; execution-heavy coding | Grok для web; Antigravity для Google-native multimodal; Codex для code execution | Если специализированная рельса теряет evidence или нарушает формат |

Главная единица маршрутизации — не пользовательский prompt и не отдельная shell-команда, а **пакет работы с проверяемым контрактом**:

```json
{
  "class": "bounded_code_change",
  "input": ["repo", "issue", "constraints"],
  "deliverable": "patch",
  "acceptance": ["tests pass", "no new lint errors"],
  "budget": {"max_turns": 8, "max_minutes": 20},
  "fallback": ["codex", "claude"]
}
```

Без `acceptance` перенос работы на более дешёвую рельсу нельзя измерить.

## 3.3 Как измерять «хуже стало»

Для каждого класса нужен golden corpus из реальных задач лаборатории:

- замороженный checkout/input;
- ожидаемое внешнее состояние;
- минимум три trials на модель;
- слепое сравнение;
- одинаковые tools и permissions;
- одинаковый time/turn budget.

Основные метрики:

| Метрика | Что ловит |
|---|---|
| Task success / pass@1 | Выполнена ли задача вообще |
| Test pass rate | Работает ли код |
| Regression count | Не сломано ли соседнее |
| Human repair minutes | Сколько времени ушло на доведение |
| Retry count | Скрытую нестабильность |
| Tool-call count | Агентное блуждание |
| Wall time | Практическую скорость |
| Quota used % | Реальную стоимость в фиксированном баке |
| Fallback rate | Насколько выбранная рельса самостоятельна |
| Unsupported-claim rate | Качество исследования/фактов |
| Rollback rate | Насколько часто перенос пришлось отменять |

Для вашей установки я бы оценивал перенос класса по формуле:

```text
usable_yield =
    completed_tasks
    / (quota_percent_used + repair_hours_weight + failed_retries_weight)
```

Токены сами по себе недостаточны. IBM показывает, что модель с более дорогим токеном может оказаться дешевле за завершённую задачу, если делает меньше шагов или лучше использует cache ([IBM Research](https://huggingface.co/blog/ibm-research/model-routing-is-simple-until-it-isnt)).

## 3.4 Какие классы обычно переносятся без заметной потери

Лучшие кандидаты:

- точный поиск и фильтрация;
- structured extraction;
- классификация по фиксированной схеме;
- преобразование форматов;
- boilerplate;
- генерация unit tests при наличии уже работающего интерфейса;
- документация существующего кода;
- запуск команд и краткое резюме результата;
- локальные исправления с сильными автоматическими тестами;
- независимые research branches, которые потом синтезирует сильная модель.

Слабые кандидаты:

- неоднозначная архитектура;
- security-critical изменения;
- debugging скрытого состояния;
- большие cross-repo migrations;
- задачи, где отсутствует executable grader;
- длинные траектории, где ошибка раннего шага портит всё последующее;
- финальный synthesis противоречивых источников;
- изменение общей инфраструктуры.

Нельзя честно утверждать, что какой-либо LLM-класс **всегда** переносится без потери. Единственный практически безусловный перенос — из LLM в детерминированный код, когда операция формализуема.

## 3.5 Применение к четырём подпискам

Рекомендуемый initial policy:

```text
1. Shell/SQL first:
   счёт, поиск, parsing, file enumeration, health checks.

2. Codex first:
   bounded coding, тесты, рефакторинг, repo exploration,
   выполнение хорошо определённой инженерной задачи.

3. Grok first:
   web/recent reconnaissance, независимая гипотеза,
   массовая первичная классификация с проверяемой схемой.

4. Antigravity first:
   Google-native multimodal/long-context,
   bounded parallel subagents, когда задача хорошо проверяется.

5. Claude remains:
   цель, декомпозиция, спорные решения, security,
   cross-system debugging, финальная приёмка.
```

Не следует делать fallback только линейным:

```text
Claude limit → Codex → Grok → Gemini
```

Это теряет специфику задач. Нужна двумерная политика:

```text
выбор = task_class × quota_state × measured_quality × health
```

---

# 4. Измеримость остатка квоты

## Сводка

| Вендор | Поддержанный машинный источник | Как снять | Вердикт |
|---|---|---|---|
| OpenAI ChatGPT Pro / Codex | **Да** | Codex app-server `account/rateLimits/read` и `account/rateLimits/updated` | Готово для production |
| Anthropic Claude Max | **Да** | Claude Code statusline JSON `rate_limits.five_hour` и `seven_day` | Готово для production |
| xAI SuperGrok | **Нет публичного стабильного API; есть воспроизводимый внутренний источник** | `grok.com/rest/rate-limits` с session cookie или community Grok OAuth extension | Работает, но schema/ToS risk |
| Google AI Pro / Antigravity | **Только интерактивный официальный `/usage`; стабильного headless JSON quota API не найдено** | TUI `/usage`/`/quota`; community local-service reader | Для production routing пока ненадёжно |

## 4.1 OpenAI

Официальный Codex app-server предоставляет:

```json
{"method":"account/rateLimits/read","id":7}
```

и отвечает структурой вида:

```json
{
  "rateLimits": {
    "primary": {
      "usedPercent": 25,
      "windowDurationMins": 15,
      "resetsAt": 1730947200
    },
    "secondary": null
  }
}
```

Есть push notification `account/rateLimits/updated`; документация предупреждает, что update sparse и его нужно merge с последним snapshot. Также доступны `rateLimitReachedType`, spend-control status и reset credits ([официальный app-server README](https://github.com/openai/codex/blob/main/codex-rs/app-server/README.md)).

Ваш transcript field `rate_limits.primary.used_percent` можно сохранить, но app-server RPC — более формальный interface.

Парсер обязан допускать отсутствие secondary/weekly window: такие расхождения уже наблюдались у пользователей ([Codex issue #32707](https://github.com/openai/codex/issues/32707)).

**Вердикт: да, полноценный машинный источник есть.**

## 4.2 Anthropic Max

Claude Code передаёт statusline script JSON через stdin. Для Pro/Max после первого ответа появляются:

```json
{
  "rate_limits": {
    "five_hour": {
      "used_percentage": 23.5,
      "resets_at": 1738425600
    },
    "seven_day": {
      "used_percentage": 41.2,
      "resets_at": 1738857600
    }
  }
}
```

Оба окна могут отсутствовать независимо, поэтому нужны nullable fields. Источник и готовые Python/Node/Bash examples опубликованы Anthropic в [официальной statusline documentation](https://code.claude.com/docs/en/statusline).

Снять можно двумя способами:

1. statusline command пишет последний payload в локальный JSON/JSONL;
2. сам оркестратор запускает лёгкий Claude turn, после чего читает обновлённое состояние.

Остаток:

```text
remaining_5h = 100 - five_hour.used_percentage
remaining_7d = 100 - seven_day.used_percentage
```

`/usage` остаётся человеческим fallback, но для automation он больше не нужен.

**Вердикт: да, полноценный машинный источник есть.**

## 4.3 xAI SuperGrok

xAI официально перешла на единый weekly usage pool. В `Settings → Usage` показываются:

- процент использованного weekly pool;
- breakdown по Build/Chat/Imagine/Voice/API;
- reset date/time;
- Extra Usage Credits.

Это описано в [официальном xAI FAQ, обновлённом 6.07.2026](https://docs.x.ai/grok/faq).

Но документированного public CLI/API для consumer quota я не нашёл.

Воспроизводимый неофициальный источник существует:

```text
grok.com/rest/rate-limits
```

Его использует Grok Desktop с authenticated session cookie и автообновлением ([репозиторий и описание implementation](https://github.com/AnRkey/Grok-Desktop)).

Ещё практичнее extension [pi-grok-cli](https://github.com/kenryu42/pi-grok-cli):

- browser/device OAuth;
- `/grok-cli-usage`;
- allowance, remaining credits и reset time;
- cache в `~/.pi/grok-cli/quota-cache.json`;
- переход на другой авторизованный account после final balance-exhausted error.

Но это community integration; она сама предупреждает, что availability зависит от account, plan, region и rollout. Ротацию между чужими accounts использовать нельзя; допустим только один account владельца либо отдельные законно принадлежащие пользователям accounts.

**Вердикт: официального стабильного машинного API нет. Технически получить квоту можно, но endpoint следует считать недокументированным и обязательно страховать реактивным 429/balance fallback.**

## 4.4 Google AI Pro

### Сначала важное изменение

С 18 июня 2026 года Google AI Pro/Ultra/free больше не обслуживаются старым Gemini CLI. Подписочный terminal surface перенесён в Antigravity CLI ([официальное объявление Google](https://github.com/google-gemini/gemini-cli/discussions/27274)).

Поэтому старые инструкции о 1 500 requests/day и `/stats model` в Gemini CLI исторически верны, но уже не описывают ваш текущий consumer subscription bucket. Старый документ всё ещё доступен в [Gemini CLI quota docs](https://github.com/google-gemini/gemini-cli/blob/main/docs/resources/quota-and-pricing.md), что создаёт опасную документационную коллизию.

### Что есть в Antigravity

Официальный Antigravity CLI предоставляет:

```text
/usage
/quota
```

Команда обновляет состояние с backend и открывает TUI с remaining requests/tokens по моделям ([официальная документация](https://antigravity.google/docs/cli/commands/usage?app=antigravity-ide)).

Есть также:

- statusline display quota;
- отдельный credit balance;
- пятичасовое и weekly ограничение для Pro;
- overage credits после исчерпания baseline ([plans](https://antigravity.google/docs/plans?app=cli), [credits](https://antigravity.google/docs/cli/credits?via=aigregator)).

Но официальный statusline JSON документирует context-window percentage, plan tier и session state, а не полный machine-readable model quota object ([statusline documentation](https://www.antigravity.google/docs/cli-statusline)).

Неофициальный инструмент [antigravity-usage](https://github.com/skainguyen1412/antigravity-usage) умеет получать model remaining/reset через локальный Antigravity/Google service. Это полезный диагностический источник, но не стабильный контракт Google.

**Вердикт: человек видит точный остаток через `/usage`, но поддержанного headless JSON quota API на 6.08.2026 не найдено. Для production router использовать community reader только как advisory signal; истинным hard fallback оставить официальный non-zero exit/quota error.**

---

# 5. Три вещи, которые вы делаете неправильно

## 1. У вас даже финансовый знаменатель сейчас не определён

Перечисленные суммы:

```text
Anthropic  $200
xAI        $300
OpenAI     $200
Google      $20
----------------
Итого      $720
```

Это не `$740`. Либо есть ещё $20 налогов/другой подписки, либо исходный total ошибочен.

Из известных сумм `$520/мес` — xAI + OpenAI + Google — сейчас используются слабо или вообще не измеряются. Это:

- **72,2% от $720**;
- **70,3% от $740**, если `$740` всё-таки правильный invoice total.

Пока Grok и Google не имеют ежедневной quota telemetry, вы не можете отличить «бак свободен» от «бак сломан/не подключён».

## 2. Вы измеряете деятельность модели, а не полезную работу

Из 36,8 млн output tokens:

- shell — `19,872 млн`;
- код — `5,888 млн`;
- чтение/поиск — `4,416 млн`;
- MCP — `4,048 млн`;
- нерасклассифицировано — `2,576 млн`.

Shell + чтение дают **24,288 млн, или 66%** всего выхода. С MCP получается **28,336 млн, или 77%**.

Но token share не отвечает:

- сколько задач завершено;
- сколько исправлений принято без переделки;
- сколько минут человека потрачено на repair;
- сколько было retries;
- сколько процентов каждого фиксированного бака израсходовано;
- сколько shell output было просто шумом.

Вы оптимизируете распределение «кто сказал больше токенов», хотя нужно оптимизировать «кто завершил больше проверенных задач на 1% подписочного бака».

## 3. Вы используете самую дефицитную рельсу как транспорт для всего

Claude четырежды упёрся в лимит за неделю, при этом Codex выбран только на 4%, а Grok/Google не измерены. Это не проблема размера Claude Max. Это отсутствие admission control.

Claude сейчас платит quota за:

- запуск shell;
- чтение сырого tool output;
- файловую навигацию;
- механические MCP envelopes;
- bounded coding, который может принять Codex;
- первичное исследование, которое может принять Grok;
- Google-native работу, которую может принять Antigravity.

Claude Code можно и следует оставить единственной дверью для человека. Но после декомпозиции он должен передавать **закрытый work order**, а не продолжать сам крутить каждую отвёртку.

## Дополнительный красный флаг: один владелец и два ассистента

Если два ассистента фактически используют персональные Claude Max/ChatGPT Pro credentials владельца, это сильнее, чем техническая проблема routing:

- Anthropic Consumer Terms запрещают делиться account login/credentials ([Consumer Terms](https://www.anthropic.com/legal/consumer-terms));
- OpenAI Europe Terms запрещают делиться credentials или делать account доступным другому человеку ([Europe Terms](https://openai.com/policies/eu-terms-of-use/)).

Шесть машин одного владельца сами по себе не обязательно проблема. Три разных человека на одном personal login — уже проблема.

---

# Итоговый практический приговор

1. **OpenClaw — наиболее близкая готовая основа**, если использовать его как внутренний control plane одного доверенного владельца.
2. Для Claude использовать только реальный `claude -p`, не импортировать OAuth в собственный HTTP client.
3. Для OpenAI использовать Codex app-server, потому что он одновременно даёт официальный headless interface и machine-readable quota.
4. Для SuperGrok использовать официальный device OAuth, а quota брать advisory-способом через Grok session endpoint или `pi-grok-cli`.
5. Для Google AI Pro использовать `agy -p`; старый Gemini CLI subscription route считать закрытым с 18.06.2026.
6. Не строить account rotation для обхода лимитов. Переключать **вендора и класс работы**, а не личности/accounts.
7. Первый полезный routing rule у вас уже следует из цифр: **66% shell+file traffic не должен по умолчанию попадать в Claude**.
8. Главное отсутствующее звено — не ещё один агент, а quota collector и policy table над четырьмя официальными CLI. Именно такого полного готового продукта рынок пока не предлагает.
