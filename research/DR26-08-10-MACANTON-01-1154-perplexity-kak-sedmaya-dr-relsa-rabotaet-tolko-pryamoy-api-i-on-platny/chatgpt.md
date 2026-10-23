# DR26-08-10-MACANTON-01-1154 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr: DR26-08-10-MACANTON-01-1154
vendor: OpenAI (Codex CLI)
door: cli-headless
rail: codex
cmd: «внутренний путь лаборатории»

collected: 2026-08-10T19:44:42Z
took_sec: 1866
exit: -9
sources_unique_urls: 57
note: >-
  Добыто локальным CLI по подписке (не вендорский режим Deep Research из
  веб-морды). Уровень = умная модель + веб-поиск вендора.
---

# A. Executive Summary

**Рекомендация: строить только API-rail на прямом асинхронном вызове `sonar-deep-research`; MCP оставить для интерактивной работы, официальный CLI не считать deep-research-рельсой, web/Comet-автоматизацию не внедрять.** Это единственный путь, который одновременно поддерживается Perplexity, работает без браузера и человека, возвращает машиночитаемую телеметрию и выдерживает cron-сценарий. Но есть важная цена: подписки Pro, Max и Enterprise **не оплачивают API**; при пяти отчётах в день новый rail потребует примерно **$105–338 в месяц**, в зависимости от глубины поиска. Если архитектурное правило «только уже оплаченный бак, без нового per-call meter» жёсткое, правильный итог — **не добавлять Perplexity вообще**. [established] ([API pricing, наблюдение 10.08.2026](https://docs.perplexity.ai/docs/getting-started/pricing); [Perplexity Max FAQ, обновлено 16.07.2026](https://www.perplexity.ai/help-center/en/articles/11680686-perplexity-max); [Enterprise billing FAQ, обновлено 22.07.2026](https://www.perplexity.ai/help-center/en/articles/10352986-enterprise-pricing-and-billing-frequently-asked-questions))

Главное допущение рекомендации: вам нужна дополнительная **независимая поисково-цитатная выборка**, и ради неё допустим отдельный API-бюджет. Если нужна независимость именно reasoning-модели, использовать надо Sonar API: нынешний официальный MCP research preset работает через `openai/gpt-5.6-sol`, а Advanced Deep Research в веб-продукте — через Claude Opus 4.6/4.5. Эти поверхности усиливают поиск Perplexity, но не дают полноценного седьмого независимого model rail. [established] ([Agent API presets, наблюдение 10.08.2026](https://docs.perplexity.ai/docs/agent-api/presets); [Advanced Deep Research, обновлено 16.07.2026](https://www.perplexity.ai/help-center/en/articles/13600190-what-s-new-in-advanced-deep-research))

---

# B. Key Findings

1. **[established] Прямой API — единственная production-viable поверхность для безлюдных запусков.** Есть синхронный и асинхронный Sonar API, отдельный `sonar-deep-research`, bearer-key, polling статуса, usage breakdown и сохранение результата асинхронного задания на семь дней. Вебхук для завершения задания в актуальной публичной документации не найден. ([Async Sonar POST, наблюдение 10.08.2026](https://docs.perplexity.ai/api-reference/async-sonar-post); [Async Sonar GET, наблюдение 10.08.2026](https://docs.perplexity.ai/api-reference/async-sonar-api-request-get); вебхук: **[no source found]**)

2. **[established] Подписка не кормит API.** Pro, Max и Enterprise дают функции внутри потребительского продукта, но API Platform тарифицируется отдельно. «Computer credits» — отдельная валюта для Perplexity Computer, не Sonar API. ([Perplexity Max FAQ, 16.07.2026](https://www.perplexity.ai/help-center/en/articles/11680686-perplexity-max); [Enterprise billing FAQ, 22.07.2026](https://www.perplexity.ai/help-center/en/articles/10352986-enterprise-pricing-and-billing-frequently-asked-questions); [How credits work, 06.08.2026](https://www.perplexity.ai/help-center/en/articles/13838041-how-credits-work-on-perplexity))

3. **[established] Официальный MCP существует, но больше не является тонкой обёрткой над `sonar-deep-research`.** В июле 2026 года Perplexity перевела MCP-инструменты `ask`, `research` и `reason` с Sonar-моделей на Agent API presets. `perplexity_research` вызывает preset `high`, который сейчас использует `openai/gpt-5.6-sol`. Поэтому MCP хуже подходит как независимый седьмой vendor rail. ([Official MCP integration](https://docs.perplexity.ai/docs/getting-started/integrations/mcp-server); [Perplexity changelog, июль 2026](https://docs.perplexity.ai/docs/resources/changelog); [Agent presets](https://docs.perplexity.ai/docs/agent-api/presets))

4. **[established] Официальный `pplx` CLI — поисковый клиент, а не deep-research клиент.** Он умеет Search API и извлечение содержимого страниц, но не запускает `sonar-deep-research` и не генерирует полноценный отчёт. Как седьмая рельса он непригоден; оболочка `curl`/Python вокруг API пригодна, но это API, а не самостоятельная CLI-поверхность. ([CLI overview, наблюдение 10.08.2026](https://docs.perplexity.ai/docs/cli/overview); [official CLI repository](https://github.com/perplexityai/perplexity-cli))

5. **[established] Headless-driving платного web-аккаунта противоречит опубликованным ограничениям ToS.** Условия запрещают использование роботов, автоматических процессов и scraper-подобных средств для доступа и извлечения данных; Perplexity может приостановить или удалить аккаунт. ([Terms of Service, обновлено 23.01.2026](https://www.perplexity.ai/hub/legal/terms-of-service))

6. **[emerging] Качество Perplexity — сильное по поиску и цитированию, но не стабильно лучше по reasoning.** Независимые тесты 2025–2026 расходятся: старые DeepResearch Bench ставили Perplexity ниже OpenAI/Gemini в общем качестве, LiveResearchBench ставит Sonar DR чуть выше Gemini и OpenAI, но ниже Grok; по «analysis depth» Perplexity заметно слабее Gemini. ([DeepResearch Bench, 13.06.2025](https://arxiv.org/abs/2506.11763); [DeepResearch Bench II, 13.01.2026](https://arxiv.org/abs/2601.08536); [LiveResearchBench, октябрь 2025](https://arxiv.org/abs/2510.14240))

7. **[emerging] У API бывают опасные тихие деградации.** Есть пользовательские отчёты о `200 OK` и внешне нормальном тексте при фактически не сработавшем веб-поиске, а также о `COMPLETED` с большим отчётом, но пустыми `citations`/`search_results`. Поэтому `exit 0` и статус `COMPLETED` недостаточны. [single-source] [low-authority] ([forum report, 21.03.2026](https://community.perplexity.ai/t/sonar-deep-research-intermittently-fails-to-activate-web-search-via-the-api/4264); [async empty-citations report, 18.06.2026](https://community.perplexity.ai/t/sonar-deep-research-async-returns-empty-citations-search-results-despite-num-search-queries-10-full-content-no-source-attribution-2026-06-13/5401))

8. **[emerging] Стратегическое направление Perplexity — Agent API и multi-model orchestration, а не только собственная Sonar-линейка.** В актуальной навигации документации Sonar находится в разделе Legacy API, тогда как новые MCP presets работают через Agent API и сторонние модели. Это не означает немедленное закрытие Sonar, но повышает риск будущих миграций и переименований. ([Perplexity API documentation, наблюдение 10.08.2026](https://docs.perplexity.ai/); [Changelog](https://docs.perplexity.ai/docs/resources/changelog))

---

# C. The Four Surfaces

## C1. HTTP API: Sonar / `sonar-deep-research`

### Что существует

Perplexity официально предоставляет:

- синхронный Sonar endpoint;
- асинхронный Sonar endpoint;
- модель `sonar-deep-research`;
- отдельные счётчики input, output, citations, searches и reasoning;
- `reasoning_effort`;
- search filters и управление источниками. ([Sonar Deep Research model](https://docs.perplexity.ai/docs/sonar/models/sonar-deep-research); [Async Sonar POST](https://docs.perplexity.ai/api-reference/async-sonar-post))

Perplexity пишет, что API использует ту же поисковую систему и интернет-данные, что и пользовательский продукт, но конфигурации и модели могут отличаться. Поэтому нельзя считать API побитно идентичным web Advanced Deep Research. ([API FAQ](https://docs.perplexity.ai/docs/resources/faq))

### Авторизация

1. Создать аккаунт на [Perplexity API Platform](https://console.perplexity.ai/).
2. Добавить платёжный метод или API-кредиты.
3. Создать API key.
4. Передавать его как `Authorization: Bearer ...`. ([API Quickstart](https://docs.perplexity.ai/docs/getting-started/quickstart))

### Минимальный синхронный вызов

```bash
export PERPLEXITY_API_KEY='pplx-...'

curl -sS https://api.perplexity.ai/v1/sonar \
  -H "Authorization: Bearer $PERPLEXITY_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "sonar-deep-research",
    "messages": [
      {
        "role": "user",
        "content": "Produce a source-grounded deep research report on..."
      }
    ],
    "reasoning_effort": "high"
  }'
```

Endpoint, payload и model ID соответствуют официальной схеме на 10.08.2026. ([Sonar API reference](https://docs.perplexity.ai/api-reference/sonar-post); [Sonar Deep Research](https://docs.perplexity.ai/docs/sonar/models/sonar-deep-research))

### Рекомендуемый асинхронный вызов для cron

```bash
export PERPLEXITY_API_KEY='pplx-...'

JOB="$(
  curl -sS https://api.perplexity.ai/v1/async/sonar \
    -H "Authorization: Bearer $PERPLEXITY_API_KEY" \
    -H "Content-Type: application/json" \
    -d '{
      "request": {
        "model": "sonar-deep-research",
        "messages": [
          {
            "role": "user",
            "content": "Produce a source-grounded deep research report on..."
          }
        ],
        "reasoning_effort": "high"
      }
    }'
)"

ID="$(printf '%s' "$JOB" | jq -r '.id')"

while :; do
  RESULT="$(
    curl -sS "https://api.perplexity.ai/v1/async/sonar/$ID" \
      -H "Authorization: Bearer $PERPLEXITY_API_KEY"
  )"

  STATUS="$(printf '%s' "$RESULT" | jq -r '.status')"

  case "$STATUS" in
    COMPLETED)
      printf '%s\n' "$RESULT" | jq '.response'
      break
      ;;
    FAILED)
      printf '%s\n' "$RESULT" | jq . >&2
      exit 1
      ;;
  esac

  sleep 10
done
```

Документированные статусы включают `CREATED`, `IN_PROGRESS`, `COMPLETED` и `FAILED`; завершённое асинхронное задание хранится семь дней. ([Async POST](https://docs.perplexity.ai/api-reference/async-sonar-post); [Async GET](https://docs.perplexity.ai/api-reference/async-sonar-api-request-get))

### Что API умеет и не умеет

**Умеет:**

- запускать нативный `sonar-deep-research`;
- возвращать citations/search results;
- показывать `num_search_queries`, token usage и стоимость;
- работать без браузера;
- переживать перезапуск вызывающей машины благодаря server-side job ID;
- отделять запуск от polling. ([Sonar Deep Research](https://docs.perplexity.ai/docs/sonar/models/sonar-deep-research); [Async Sonar](https://docs.perplexity.ai/api-reference/async-sonar-post))

**Не гарантирует:**

- точное совпадение с web Advanced Deep Research;
- webhook/callback по завершении — публично документированный webhook не найден;
- SLA по времени завершения, uptime или recovery-time objective — официальный FAQ прямо говорит, что формальной гарантии uptime/failure rate/RTO нет. ([API FAQ](https://docs.perplexity.ai/docs/resources/faq); webhook: **[no source found]**)

### Вердикт по unattended production

**Да, условно production-viable.** Это единственная поверхность, которую разумно запускать неделями без человека. Нужны polling, ограниченный retry, бюджетный guard и содержательная проверка результата. [established]

---

## C2. MCP server

### Официальный сервер

Официальный репозиторий: [perplexityai/modelcontextprotocol](https://github.com/perplexityai/modelcontextprotocol), поддерживается организацией PerplexityAI, лицензия MIT. На 10.08.2026 GitHub показывал около **2,4 тыс. stars, 367 forks и 164 commits**; в истории репозитория видны изменения от 27.07.2026. [single-source] ([repository](https://github.com/perplexityai/modelcontextprotocol); [commit history](https://github.com/perplexityai/modelcontextprotocol/commits/main/))

Поддерживаются:

- remote MCP: `https://api.perplexity.ai/mcp`;
- Streamable HTTP;
- bearer API key;
- локальный stdio-сервер через npm;
- самостоятельный HTTP-hosting. ([MCP integration docs](https://docs.perplexity.ai/docs/getting-started/integrations/mcp-server))

### Текущие MCP tools

В актуальной версии заявлены:

- `perplexity_search` — Search API;
- `perplexity_ask` — Agent API preset `fast`;
- `perplexity_research` — Agent API preset `high`;
- `perplexity_reason` — Agent API preset `medium`. ([MCP documentation](https://docs.perplexity.ai/docs/getting-started/integrations/mcp-server))

Это важное изменение. Старые README и интеграции описывали прямые Sonar-модели, включая `sonar-deep-research`; июльский changelog 2026 фиксирует перевод model tools на Agent API presets. Поля вроде `reasoning_effort` и `strip_thinking` для MCP теперь игнорируются. ([Changelog, июль 2026](https://docs.perplexity.ai/docs/resources/changelog))

Preset `high` на 10.08.2026 использовал:

- `openai/gpt-5.6-sol`;
- до 15 agent steps;
- reasoning `medium`;
- максимальный output 128k;
- `web_search` и `fetch_url`. ([Agent presets](https://docs.perplexity.ai/docs/agent-api/presets))

Presets управляются Perplexity и могут изменяться. Для воспроизводимости Agent API позволяет заморозить явную конфигурацию, но MCP tool сам по себе привязан к динамическому preset. ([Agent presets](https://docs.perplexity.ai/docs/agent-api/presets))

### Подключение к Claude Code

Remote:

```bash
export PERPLEXITY_API_KEY='pplx-...'

claude mcp add \
  --transport http \
  perplexity \
  https://api.perplexity.ai/mcp \
  --header "Authorization: Bearer $PERPLEXITY_API_KEY"
```

Local stdio:

```bash
claude mcp add \
  perplexity \
  --env PERPLEXITY_API_KEY='pplx-...' \
  -- npx -yq @perplexity-ai/mcp-server
```

Форматы опубликованы официальным сервером и документацией Claude Code integration. ([official MCP repository](https://github.com/perplexityai/modelcontextprotocol); [MCP integration docs](https://docs.perplexity.ai/docs/getting-started/integrations/mcp-server))

### Claude Desktop

```json
{
  "mcpServers": {
    "perplexity": {
      "command": "npx",
      "args": ["-yq", "@perplexity-ai/mcp-server"],
      "env": {
        "PERPLEXITY_API_KEY": "pplx-..."
      }
    }
  }
}
```

После перезапуска клиента первый содержательный вызов — попросить Claude вызвать `perplexity_research` с исследовательским запросом. ([official MCP repository](https://github.com/perplexityai/modelcontextprotocol))

### Как тарифицируется MCP

MCP использует API key и API endpoints, поэтому расходует API meter, а не Pro/Max subscription. Поиск и Agent tools тарифицируются по API rate card. ([MCP docs](https://docs.perplexity.ai/docs/getting-started/integrations/mcp-server); [API pricing](https://docs.perplexity.ai/docs/getting-started/pricing))

### Исторические отказы

В мае 2026 года пользователь официального MCP сообщил, что deep-research вызовы обрывались в Claude Desktop примерно через 60–90 секунд из-за отсутствия промежуточных SSE-событий. Issue был закрыт; июльская версия добавила streaming progress и propagation отмены. [single-source] ([MCP issue #110, 19.05.2026](https://github.com/perplexityai/modelcontextprotocol/issues/110); [changelog](https://docs.perplexity.ai/docs/resources/changelog))

В MCP-сообществе отдельно обсуждалась проблема long-running operations: deep research занимает несколько минут, тогда как хост может оборвать tool call раньше. Предложение заключалось в разделении `create`/`get`, но это было предложение к протоколу, а не доказательство универсального исправления во всех клиентах. [single-source] ([MCP specification issue #1391, 26.08.2025](https://github.com/modelcontextprotocol/modelcontextprotocol/issues/1391))

Документированных Perplexity-специфичных данных по неделям работы, idle-timeout и zombie-процессам локального stdio-сервера не найдено. **[no source found]**

### Третьи стороны

| Проект | Сигнал принятия | Отличие | Вердикт |
|---|---:|---|---|
| [jsonallen/perplexity-mcp](https://github.com/jsonallen/perplexity-mcp) | около 307 stars, 47 forks, 30 commits на 10.08.2026 | Один MCP tool, можно выбрать Sonar model, включая SDR | Уступает официальному серверу; список моделей содержит устаревшие имена |
| [jacob-bd/perplexity-web-mcp](https://github.com/jacob-bd/perplexity-web-mcp) | около 163 stars, 27 forks, 159 commits; PyPI 0.14.7 от 28.07.2026 | Использует web-account/session вместо API, имеет CLI/MCP/API и research | Экономит API meter, но неофициален, ломок и несёт риск аккаунта |

GitHub-счётчики — [single-source]; точная дата последнего commit для `jsonallen/perplexity-mcp` из доступных индексированных страниц не установлена: **[no source found]**. Сам `jacob-bd` предупреждает, что проект неофициальный, зависит от недокументированной web-поверхности, может ломаться и привести к rate limit, suspension или termination аккаунта. [single-source] [low-authority] ([repository](https://github.com/jacob-bd/perplexity-web-mcp); [PyPI](https://pypi.org/project/perplexity-web-mcp-cli/))

### Вердикт по unattended production

**Не использовать MCP как основную cron-рельсу.** Он годится для интерактивных клиентов, но добавляет host timeout, tool lifecycle и изменяемый preset поверх API. Кроме того, текущий `research` — OpenAI-backed agent, а не нативный Sonar DR. Для расписания прямой async API проще и воспроизводимее. [established]

---

## C3. CLI / terminal

### Официальный CLI

Официальный проект: [perplexityai/perplexity-cli](https://github.com/perplexityai/perplexity-cli), поддерживается PerplexityAI. На 10.08.2026 репозиторий показывал около **84 stars, 13 forks и 7 commits**; релиз `v0.2.3` опубликован 30.07.2026. [single-source] ([repository](https://github.com/perplexityai/perplexity-cli); [v0.2.3 release](https://github.com/perplexityai/perplexity-cli/releases/tag/v0.2.3))

Документация перечисляет готовые сборки для macOS Apple Silicon и Linux x86_64/ARM64. Готовый Windows binary в этой матрице не указан. ([CLI overview](https://docs.perplexity.ai/docs/cli/overview))

### Установка и авторизация

```bash
curl -fsSL https://raw.githubusercontent.com/perplexityai/perplexity-cli/main/install.sh | sh

export PERPLEXITY_API_KEY='pplx-...'
pplx auth login
```

### Первый вызов

```bash
pplx search web \
  "Perplexity sonar deep research API reliability" \
  -n 10 \
  --output-dir ./pplx-out
```

Для извлечения содержимого:

```bash
pplx content snippets https://docs.perplexity.ai/docs/getting-started/pricing
```

CLI поддерживает JSON-friendly output и сохранение артефактов. В content-командах отдельные URL могут завершиться ошибкой внутри результата, даже если общий процесс не упал; вызывающая автоматизация должна проверять поле ошибки каждого результата. ([CLI overview](https://docs.perplexity.ai/docs/cli/overview); [CLI repository](https://github.com/perplexityai/perplexity-cli))

### Ограничение

У официального `pplx` на 10.08.2026 нет команды, которая запускает `sonar-deep-research` и возвращает исследовательский отчёт. Он обращается к Search API и content endpoints. Поэтому:

- как building block для собственного RAG/search pipeline — да;
- как седьмой deep-research rail — нет;
- как замена браузеру — только для raw search, не для Advanced Research. ([CLI docs](https://docs.perplexity.ai/docs/cli/overview))

### Неофициальный CLI через web-сессию

`perplexity-web-mcp-cli` предлагает:

```bash
uv tool install perplexity-web-mcp-cli
pwm login
pwm research "Produce a deep research report on..." --json
```

`pwm login` требует email/OTP при первичной авторизации и повторно после истечения сессии. Автор заявляет, что session tokens обычно живут около 30 дней, но это не официальная гарантия Perplexity. [single-source] [low-authority] ([GitHub](https://github.com/jacob-bd/perplexity-web-mcp); [PyPI](https://pypi.org/project/perplexity-web-mcp-cli/))

### Вердикт по unattended production

- Официальный `pplx`: **надёжен, но не deep research rail**.
- Собственный `pplx-dr` wrapper над async API: **да**, но архитектурно это API.
- Неофициальный `pwm`: **нет**, потому что OTP/session expiry, недокументированный endpoint и риск аккаунта.

---

## C4. Headless browser / logged-in perplexity.ai / Comet

### Позиция Terms of Service

Условия Perplexity, обновлённые 23.01.2026:

- предоставляют потребительский сервис для личного, некоммерческого использования;
- запрещают automation software, bots и неавторизованные сторонние средства;
- отдельно запрещают robots, spiders, crawlers, scrapers и другие автоматические процессы, которые получают доступ к сервису для мониторинга, извлечения, копирования или сбора данных;
- позволяют Perplexity приостанавливать, отключать или удалять аккаунты и контент при нарушении или подозрении на нарушение. ([Terms of Service, 23.01.2026](https://www.perplexity.ai/hub/legal/terms-of-service))

Для Selenium/Playwright, который логинится и регулярно извлекает отчёты, риск попадает прямо в область этой формулировки. Это не юридическая консультация, но договорный риск высокий. [established]

### Практическая устойчивость

Perplexity официально использует Cloudflare-проверки и рекомендует отключать VPN/proxy при проблемах доступа. ([Cloudflare and VPNs, обновлено 16.07.2026](https://www.perplexity.ai/help-center/en/articles/10354884-cloudflare-vpns))

В мае 2026 года пользователь серверного web-provider сообщил о `403` и Cloudflare-странице «Just a moment» при запросах с VPS. Имитация browser TLS fingerprint вернула `200`, что одновременно показывает и техническую возможность обхода, и его хрупкость: защита зависит от сетевого отпечатка, а не только cookies. [single-source] [low-authority] ([OmniRoute issue #2459, 21.05.2026](https://github.com/diegosouzapw/OmniRoute/issues/2459))

Надёжных публичных продольных тестов 2025–2026, где Selenium/Playwright непрерывно управлял perplexity.ai неделями и измерялась частота поломок, не найдено. **[no source found]** Поэтому нельзя честно назвать средний срок до поломки.

### Comet

Comet — Chromium-based браузер Perplexity, поддерживающий большинство Chrome extensions. Enterprise policies включают `UiAutomationProviderEnabled`, но это системная accessibility/UI-automation возможность Chromium, а не документированный Perplexity API для запуска Advanced Research из cron. ([Getting started with Comet, обновлено 16.07.2026](https://www.perplexity.ai/help-center/en/articles/11172798-getting-started-with-comet); [Comet policies, 16.07.2026](https://www.perplexity.ai/help-center/en/articles/13529668-comet-policies-and-controls))

Публичного Comet automation SDK, headless API, scheduler API, remote agent endpoint или официального CDP workflow для безлюдного deep research не найдено. **[no source found]**

Технически Comet можно пробовать вести через CDP, desktop automation или extension, но это неподдерживаемый hack с теми же session, CAPTCHA, anti-bot и ToS-рисками.

### Что реально может случиться

- CAPTCHA или Cloudflare challenge;
- протухшая сессия;
- OTP и отсутствие человека;
- изменившийся DOM;
- отчёт не завершился, но wrapper сохранил промежуточный экран;
- ограничение аккаунта;
- suspension/deletion аккаунта и потеря доступа к истории/библиотеке;
- блокировка IP или fingerprint. ([Terms](https://www.perplexity.ai/hub/legal/terms-of-service); [Cloudflare help](https://www.perplexity.ai/help-center/en/articles/10354884-cloudflare-vpns))

### Вердикт по unattended production

**Нет.** Не потому что браузер невозможно автоматизировать технически, а потому что поддерживаемого контракта нет, ToS неблагоприятны, а отказ часто требует человека. Это не соответствует определению autonomous rail. [established]

---

# D. Cost Model

## D1. Подписки и потребительские планы

Цены наблюдались 10.08.2026.

| План | Текущая цена | Web Deep Research | API credits | Computer credits |
|---|---:|---:|---:|---:|
| Free | $0 | 1 research query/month | Нет | Нет заявленного регулярного пакета |
| Pro | $20/month или $200/year | Monthly limit для average use; точное число публично не указано | Нет | Нет регулярного пакета; 4,000 разовый bonus на 30 дней |
| Education Pro | $10/month | Как Pro | Нет | Публичная регулярная квота не найдена |
| Max | $200/month или $2,000/year | Более высокий monthly limit; точное число публично не указано | Нет | 10,000/month + 35,000 разово на 30 дней |
| Enterprise Pro | $40/seat/month или $400/year | 50/month | Нет | 500/month + 8,000 разово |
| Enterprise Max | $325/seat/month или $3,250/year | 500/month | Нет | 15,000/month + 40,000 разово |

Источники: [plan comparison, обновлено 22.07.2026](https://www.perplexity.ai/help-center/en/articles/11187416-which-perplexity-subscription-plan-is-right-for-you); [Perplexity hub](https://www.perplexity.ai/hub); [Max FAQ, 16.07.2026](https://www.perplexity.ai/help-center/en/articles/11680686-perplexity-max); [Enterprise FAQ, 22.07.2026](https://www.perplexity.ai/help-center/en/articles/10352986-enterprise-pricing-and-billing-frequently-asked-questions); [credits, 06.08.2026](https://www.perplexity.ai/help-center/en/articles/13838041-how-credits-work-on-perplexity).

**Comet не является отдельным тарифным планом:** это браузер; доступные функции определяются подпиской аккаунта. ([Getting started with Comet](https://www.perplexity.ai/help-center/en/articles/11172798-getting-started-with-comet))

Computer credits тарифицируются как 100 credits = $1. Неиспользованные ежемесячные credits не переносятся; купленные credits истекают после года неактивности. Эти credits оплачивают Perplexity Computer, а не Sonar API. ([How credits work, 06.08.2026](https://www.perplexity.ai/help-center/en/articles/13838041-how-credits-work-on-perplexity))

### Subscription-vs-API verdict

**Ни один перечисленный consumer/enterprise plan не является оплаченной API-рельсой.** Max за $200 в месяц не даёт $200 API allowance. Enterprise также не включает complimentary API credits. [established] ([Max FAQ](https://www.perplexity.ai/help-center/en/articles/11680686-perplexity-max); [Enterprise billing FAQ](https://www.perplexity.ai/help-center/en/articles/10352986-enterprise-pricing-and-billing-frequently-asked-questions))

---

## D2. Полная текущая цена Sonar

Цены наблюдались 10.08.2026.

| Модель | Input | Output | Citation | Search queries | Reasoning | Request fee |
|---|---:|---:|---:|---:|---:|---:|
| `sonar` | $1/M tokens | $1/M | — | — | — | $5/$8/$12 за 1,000 requests при low/medium/high search context |
| `sonar-pro` | $3/M | $15/M | — | — | — | $6/$10/$14 за 1,000 requests |
| `sonar-reasoning-pro` | $2/M | $8/M | — | — | — | $6/$10/$14 за 1,000 requests |
| `sonar-deep-research` | $2/M | $8/M | $2/M citation tokens | $5/1,000 queries | $3/M reasoning tokens | Отдельного standard context request fee нет |

Источник: [Perplexity API Pricing, наблюдение 10.08.2026](https://docs.perplexity.ai/docs/getting-started/pricing).

Дополнительно:

- Search API: $5 за 1,000 успешных `POST /search`; один billable request может содержать до пяти query phrasings.
- Agent API `web_search`: $0.0025 за вызов.
- `fetch_url`: $0.00025 за вызов.
- sandbox: $0.03 за сессию, плюс модельные токены. ([API Pricing](https://docs.perplexity.ai/docs/getting-started/pricing))

---

## D3. Реальная стоимость пяти отчётов в день

Пять отчётов в день = примерно 150 отчётов за 30 дней.

Официальный пример `sonar-deep-research` показывает:

- 33 prompt tokens;
- 11,395 completion tokens;
- 19,028 citation tokens;
- 21 search query;
- 193,947 reasoning tokens;
- итог **$0.816123**;
- 28 возвращённых URL. ([Sonar Deep Research example](https://docs.perplexity.ai/docs/sonar/models/sonar-deep-research))

При 150 аналогичных отчётах:

**$0.816123 × 150 = $122.42/month.**

### Модель трёх нагрузок

Это расчёт по официальной тарифной формуле, а не заявленная Perplexity фиксированная цена.

| Сценарий | Output | Citation | Searches | Reasoning | Цена/отчёт | 150/month |
|---|---:|---:|---:|---:|---:|---:|
| 30-source/light | 6k | 25k | 30 | 150k | **$0.6982** | **$104.73** |
| 50-source/medium | 8k | 40k | 50 | 250k | **$1.1442** | **$171.63** |
| 100-source/heavy | 12k | 80k | 100 | 500k | **$2.2562** | **$338.43** |

Расчёт medium:

- input: 100 × $2/M = $0.0002;
- output: 8,000 × $8/M = $0.064;
- citations: 40,000 × $2/M = $0.08;
- searches: 50 × $5/1,000 = $0.25;
- reasoning: 250,000 × $3/M = $0.75;
- итого: **$1.1442**.

Главный драйвер цены — reasoning tokens, затем число внутренних search queries.

### Сравнение с соседними API

Gemini Deep Research API указывает ориентир **$1–3 на standard task** и **$3–7 на Max-quality task**. При 150 задачах это приблизительно $150–450 или $450–1,050 соответственно. ([Gemini Deep Research API, обновлено 14.07.2026](https://ai.google.dev/gemini-api/docs/deep-research))

OpenAI публикует для `o3-deep-research` $10/M input и $40/M output, а для `o4-mini-deep-research` — $2/M input и $8/M output, к которым добавляются tool calls. Без точного числа search/tool calls корректную стоимость одного вашего отчёта вычислить нельзя. ([o3 Deep Research pricing](https://developers.openai.com/api/docs/models/o3-deep-research); [o4-mini Deep Research pricing](https://developers.openai.com/api/docs/models/o4-mini-deep-research))

По вашим вводным существующие шесть рельс уже оплачены подписками, поэтому их **предельная** стоимость конкретного ночного задания близка к нулю, пока вы не выходите за квоты. Perplexity API создаёт новый денежный meter даже при уже оплаченной Max-подписке.

---

## D4. Rate limits

API tier определяется совокупно купленными credits и не понижается:

| Tier | Куплено credits | `sonar-deep-research` RPM | Обычные Sonar RPM |
|---|---:|---:|---:|
| 0 | $0 | 5 | 50 |
| 1 | $50 | 10 | 150 |
| 2 | $250 | 20 | 500 |
| 3 | $500 | 40 | 1,000 |
| 4 | $1,000 | 60 | 4,000 |
| 5 | $5,000 | 100 | 4,000 |

Для async API опубликованы те же POST limits; чтение списка разрешает до 3,000 RPM, чтение отдельного job — до 6,000 RPM. Пять заданий в сутки помещаются даже в Tier 0. ([Rate limits and usage tiers](https://docs.perplexity.ai/docs/admin/rate-limits-usage-tiers))

---

# E. Quality Comparison

## E1. Независимые сравнительные результаты

| Evaluation | Perplexity | OpenAI DR | Gemini DR | Grok | Что важно |
|---|---:|---:|---:|---:|---|
| DeepResearch Bench, июнь 2025, 100 PhD-level tasks | RACE **42.25** | **46.98** | **48.88** | **40.24** | Perplexity уступил OpenAI/Gemini overall |
| Citation accuracy в том же тесте | **90.24%** | 77.96% | 81.44% | 83.59% | Perplexity лидировал по точности citations |
| Effective citations/task | **31.26** | 40.79 | 111.21 | 8.15 | Gemini дал намного более широкое покрытие |
| DeepResearch Bench II, январь 2026 | total **38.58** | o3 **45.40** | Gemini 3 Pro **44.60** | **39.23** | Perplexity снова ниже OpenAI/Gemini |
| LiveResearchBench | average **67.1** | o3 62.9 | 66.4 | **69.4** | Perplexity немного выше Gemini/OpenAI, ниже Grok |
| LiveResearchBench citation association | **52.1** | 30.9 | **64.6** | Не указан здесь | Gemini лучше по связи claim↔citation |
| LiveResearchBench analysis-depth wins | **14.3%** | Не указан здесь | **44.8%** | Не указан здесь | Perplexity retrieval сильнее, чем глубокий анализ |

Источники: [DeepResearch Bench paper, 13.06.2025](https://arxiv.org/abs/2506.11763) и [paper PDF](https://deepresearch-bench.github.io/static/papers/deepresearch-bench.pdf); [DeepResearch Bench II, 13.01.2026](https://arxiv.org/abs/2601.08536) и [leaderboard](https://agentresearchlab.com/benchmarks/deepresearch-bench-ii/index.html); [LiveResearchBench paper](https://arxiv.org/abs/2510.14240) и [live leaderboard, наблюдение 10.08.2026](https://livedeepresearch.github.io/). Каждый benchmark — [single-source] для своей методики; результаты относятся к протестированным версиям, а не автоматически к любому будущему model revision.

ResearcherBench исследовал 65 frontier-вопросов из 35 предметных областей и сообщил, что OpenAI и Gemini статистически превосходили остальные протестированные системы; доступный abstract не содержит достаточной таблицы для честного восстановления точных Perplexity scores. [single-source] ([ResearcherBench, 22.07.2025](https://arxiv.org/abs/2507.16280))

### Противоречащий результат: DRACO

Perplexity опубликовала DRACO — 100 задач в десяти доменах. Там web Advanced Deep Research с Opus 4.6 получил normalized score **70.5**, с Opus 4.5 — **67.2**, Gemini — 59.0, OpenAI o3 — 52.1, OpenAI o4-mini — 41.9. Citation Quality составил соответственно 64.6, 62.5, 51.5, 45.8 и 42.5. [single-source] ([DRACO paper, 12.02.2026](https://arxiv.org/abs/2602.11685); [DRACO PDF](https://r2cdn.perplexity.ai/pplx-draco.pdf))

Стальная версия аргумента в пользу DRACO: тест использует более свежие системы и может лучше отражать современный web-product.

Ограничение: DRACO создан Perplexity и тестирует прежде всего **Perplexity web orchestration поверх Claude Opus**, а не чистый `sonar-deep-research` API. Поэтому он подтверждает силу поискового и агентного контура Perplexity, но не доказывает превосходство нативного Sonar rail.

## E2. Надёжность цитирования

Columbia Journalism Review протестировал восемь AI-search продуктов на идентификации источников новостных фрагментов. Perplexity ошибался в 37% запросов, тогда как Grok 3 — в 94%. Это говорит, что Perplexity лучше многих конкурентов в source attribution, но тест не является прямой проверкой поддержки каждого research-утверждения цитатой. [single-source] ([CJR/Tow Center, 06.03.2025](https://www.cjr.org/tow_center/we-compared-eight-ai-search-engines-theyre-all-bad-at-citing-news.php))

Исследование 2026 года десяти моделей и research agents обнаружило в совокупности:

- 3–13% hallucinated citation URLs;
- 5–18% URL, которые не открывались;
- deep-research agents создавали больше citations, но имели более высокий hallucination rate;
- автоматическая URL-health/self-correction снижала долю неоткрывающихся ссылок до менее 1%. ([Citation URL hallucinations study, 03.04.2026](https://arxiv.org/abs/2604.03173))

Публичный abstract не даёт надёжного отдельного текущего числа именно для `sonar-deep-research`; поэтому точный «hallucinated-URL rate Perplexity 2026» — **[no source found]**.

## E3. Что Perplexity добавляет к вашим шести рельсам

**Добавляет:**

- сильную поисковую специализацию;
- хороший source attribution;
- свежую web-выборку;
- относительно дешёвый нативный deep-research API;
- машиночитаемый breakdown поисков и reasoning;
- потенциально новые источники, не найденные другими vendors. [emerging] ([DeepResearch Bench](https://arxiv.org/abs/2506.11763); [LiveResearchBench](https://livedeepresearch.github.io/); [API pricing](https://docs.perplexity.ai/docs/getting-started/pricing))

**Не добавляет убедительно:**

- лучшую глубину анализа;
- гарантированно более широкий охват, чем Gemini;
- нулевую marginal cost;
- независимую reasoning-модель, если использовать текущий MCP или web Advanced Research, поскольку они опираются на OpenAI/Claude. ([Agent presets](https://docs.perplexity.ai/docs/agent-api/presets); [Advanced Deep Research](https://www.perplexity.ai/help-center/en/articles/13600190-what-s-new-in-advanced-deep-research))

**Итог:** Perplexity полезен как **retrieval-and-citation diversity rail**, а не как ещё один великий мыслитель. Если post-synthesis уже объединяет ChatGPT, Gemini, Grok и Claude, добавочная ценность должна измеряться числом новых подтверждённых источников и новых поддержанных фактов, а не красотой седьмого отчёта.

---

# F. Failure Modes & Silent-Degradation Detection

## F1. Задокументированные проблемы

### Тихое отсутствие веб-поиска

В марте 2026 года пользователь сообщил, что после 318 успешных задач `sonar-deep-research` начал периодически возвращать обычный ответ без веб-поиска: 13 из 83 запросов, или 15.7%. HTTP status оставался успешным, текст выглядел связно, но содержал фразы про knowledge cutoff и отсутствие real-time search. Другой участник темы подтвердил похожее поведение. [single-source] [low-authority] ([community report, 21.03.2026](https://community.perplexity.ai/t/sonar-deep-research-intermittently-fails-to-activate-web-search-via-the-api/4264))

### `COMPLETED`, длинный ответ, пустые citations

В июне 2026 года пользователь описал два async jobs со статусом `COMPLETED` и отчётами более 10 тыс. слов, где `num_search_queries=10`, но `citations` и `search_results` были пустыми. Сотрудник Perplexity не смог воспроизвести сбой. [single-source] [low-authority] ([community report, 18.06.2026](https://community.perplexity.ai/t/sonar-deep-research-async-returns-empty-citations-search-results-despite-num-search-queries-10-full-content-no-source-attribution-2026-06-13/5401))

### Empty search results у других Sonar-моделей

Есть отдельный отчёт мая 2026 года о пустых или нерелевантных `search_results` у `sonar-pro`/`sonar-reasoning-pro` при отсутствии зарегистрированного status-page incident. Это не прямое доказательство сбоя SDR, но показывает, что search payload требует собственной валидации. [single-source] [low-authority] ([community report, 05.05.2026](https://community.perplexity.ai/t/sonar-pro-and-sonar-reasoning-pro-returning-empty-search-results-across-all-queries-2026-05-01/5074))

### «Подключили и выкинули»

Публичного, хорошо документированного postmortem команды, которая внедрила Perplexity в production agent fleet и затем полностью удалила его, найти не удалось. **[no source found]** Ближайшее доказательство — отдельные issue и forum reports о timeouts, empty citations и web-search dropout, но выдавать их за полноценный corporate rip-out было бы неправильно.

## F2. Переименования и миграции

История модели уже содержит несколько изменений:

| Дата | Старое | Новое |
|---|---|---|
| 12.08.2024 | Llama 3 Sonar 32k small/large | Llama 3.1 Sonar 128k |
| 22.02.2025 | `llama-3.1-sonar-small/large/huge-128k-online` | `sonar` / `sonar-pro` |
| 01.08.2025 | `r1-1776` | Sonar Pro Reasoning |
| 15.12.2025 | `sonar-reasoning` | `sonar-reasoning-pro` |
| Июль 2026 | MCP Sonar tools | Agent API presets |

Источник: [Perplexity Changelog](https://docs.perplexity.ai/docs/resources/changelog).

`sonar-deep-research` остаётся документированной моделью на 10.08.2026, но Sonar API уже помещён в документации под Legacy API. [emerging] ([Sonar Deep Research docs](https://docs.perplexity.ai/docs/sonar/models/sonar-deep-research); [documentation navigation](https://docs.perplexity.ai/))

## F3. Machine-checkable acceptance gate

Cron job должен считать результат успешным только при выполнении всех обязательных проверок:

```text
status == COMPLETED
response.model == "sonar-deep-research"
usage.num_search_queries >= 10
usage.citation_tokens > 0
citations/search_results are non-empty
unique_url_count >= configured minimum
completion_tokens >= configured minimum
reasoning_tokens >= configured minimum
no degradation phrases
URL health pass
sampled claim-to-citation support pass
cost <= job budget
```

Рекомендуемый первоначальный профиль для многостраничного отчёта:

| Проверка | Shadow threshold |
|---|---:|
| Search queries | ≥10 |
| Unique URLs | ≥30 |
| Unique domains | ≥5 |
| Completion tokens | ≥2,500 |
| Reasoning tokens | ≥50,000 |
| Citation tokens | >0 |
| Dead/non-resolving URLs | ≤5% |
| Retry | Один раз, с jitter |
| Circuit breaker | После 3 последовательных semantic failures |

Пороги — **[emerging]**, а не официальная гарантия. Они выведены из формы billing telemetry и официального примера с 21 search query, 11,395 completion tokens и 193,947 reasoning tokens. Их надо откалибровать на 30–50 ваших задачах. ([Official SDR example](https://docs.perplexity.ai/docs/sonar/models/sonar-deep-research))

Отдельно отклонять ответы, содержащие:

```text
real-time web search is not available
knowledge cutoff
0 queries executed
unable to access current sources
no sources were available
```

Это защита от задокументированного класса «нормальный текст без research». [single-source] [low-authority] ([March 2026 report](https://community.perplexity.ai/t/sonar-deep-research-intermittently-fails-to-activate-web-search-via-the-api/4264))

Необходимо сохранять:

- raw request;
- raw response;
- async job ID;
- timestamps;
- model ID;
- все `usage` counters;
- итоговую стоимость;
- список URL;
- причину acceptance/rejection.

`exit 0`, HTTP 200 и `COMPLETED` сами по себе не доказательство deep research.

---

# G. Strategic Recommendation

## Option 1: API-only native Sonar rail

**Конструкция:** async `sonar-deep-research`, отдельный adapter, schema/quality gate, raw artifacts, hard budget, один retry.

**Плюсы:**

- официально поддерживается;
- без браузера и человека;
- нативный Sonar даёт больше vendor diversity;
- стоимость предсказуема;
- сохраняется server-side job ID;
- хорошо наблюдается через usage counters.

**Минусы:**

- отдельный pay-as-you-go meter;
- нет формального SLA;
- модельная линейка уже переживала rename/migration;
- возможны тихие shallow results;
- не очевидно, что седьмой отчёт улучшит synthesis после уже существующих шести.

**Оценка:** лучший технический вариант.

## Option 2: MCP только в интерактивных сессиях

**Конструкция:** официальный remote MCP для Claude Code/Desktop.

**Плюсы:**

- простая установка;
- Perplexity search/research прямо внутри агента;
- remote transport;
- официальная поддержка.

**Минусы:**

- `perplexity_research` сейчас использует OpenAI-backed Agent preset;
- dynamic preset может измениться;
- MCP host timeouts;
- всё равно отдельный API meter;
- слабее воспроизводимость cron run.

**Оценка:** полезный интерактивный инструмент, но не autonomous seventh rail.

## Option 3: Official CLI rail

**Плюсы:** маленький бинарник, JSON, хороший raw search client.

**Минусы:** нет deep-research generation.

**Оценка:** не кандидат.

## Option 4: Web/Comet subscription rail

**Плюсы:**

- теоретически расходует уже оплаченную Pro/Max-квоту;
- даёт Advanced Research web experience.

**Минусы:**

- ToS-риск;
- CAPTCHA/Cloudflare/session expiry;
- нет поддерживаемого headless API;
- human OTP;
- DOM drift;
- web Advanced Research использует Claude Opus, уменьшая vendor diversity;
- возможна блокировка аккаунта.

**Оценка:** не строить.

## Option 5: Не интегрировать Perplexity

**Плюсы:**

- нулевая новая стоимость;
- нет седьмой точки отказа;
- существующие Gemini/ChatGPT/Grok/Claude уже перекрывают synthesis;
- не нужно обслуживать очередную мигрирующую модель.

**Минусы:**

- теряется шанс на независимую поисково-цитатную выборку;
- меньше source diversity.

**Оценка:** правильный выбор, если новое per-call расходование запрещено или shadow test не покажет измеримой добавочной ценности.

### Финальная рекомендация

**Запустить ограниченный shadow-пилот Option 1, не включать Perplexity сразу в quorum.** После 30–50 заданий оставить rail только если:

- ≥95% заданий проходят semantic gate после максимум одного retry;
- средняя стоимость остаётся ≤$2.25 за отчёт;
- Perplexity добавляет ≥10% новых, открывающихся и действительно поддерживающих утверждения источников либо новых подтверждённых фактов относительно текущих шести rails;
- нет трёх последовательных silent-degradation failures.

Эти пороги — предложенные управленческие критерии, не отраслевой стандарт.

Если обязательное условие — «сжигать только уже оплаченные подписочные баки», **сразу выбрать Option 5 и не пилотировать browser hack**.

**Confidence: [established]** для выбора API против browser/MCP/CLI; **[emerging]** для экономической ценности седьмого rail, потому что её можно установить только сравнительным shadow-прогоном на вашем корпусе.

---

# H. Action Plan

1. Создать отдельный Perplexity API project и key в [API console](https://console.perplexity.ai/). Установить небольшой hard spending limit.

2. Выполнить один синхронный smoke test `sonar-deep-research`; проверить `model`, citations, search results и полный `usage`.

3. Реализовать async `POST /v1/async/sonar` → сохранить job ID → polling `GET` до terminal status.

4. Добавить timeout вызывающего процесса, но не терять job ID: после рестарта продолжать polling.

5. Сохранять raw JSON и отдельно нормализованный отчёт. Никогда не считать Markdown единственным артефактом.

6. Внедрить semantic acceptance gate из раздела F, включая URL-health и sampled claim-support verification.

7. Прогнать 30–50 тех же prompt IDs, которые уже отправлялись существующим шести rails.

8. Измерить для каждого задания:

   - стоимость;
   - latency;
   - число searches;
   - количество citations и уникальных domains;
   - долю мёртвых URL;
   - source overlap/Jaccard с шестью rails;
   - новые подтверждённые факты;
   - число shallow/empty-citation failures;
   - результат после одного retry.

9. **Decision point:** отказаться от интеграции, если valid-report rate ниже 95%, медианная стоимость выше согласованного бюджета или incremental verified value ниже 10%.

10. При прохождении gate добавить Perplexity как **non-voting shadow rail** ещё на две недели. Только после стабильного периода разрешить ему считаться одним голосом quorum.

11. Не подключать официальный MCP к ночному scheduler. Использовать его лишь для ручных исследовательских сессий.

12. Не устанавливать `perplexity-web-mcp` и не строить Selenium/Playwright/Comet automation на production-аккаунте.

13. Ежемесячно проверять [changelog](https://docs.perplexity.ai/docs/resources/changelog), model ID, pricing и статус Sonar Legacy API. Любая смена модели должна заново запускать shadow calibration.

---

# I. Sources

1. [Perplexity API Pricing](https://docs.perplexity.ai/docs/getting-started/pricing) — актуальное состояние наблюдалось 10.08.2026.  
2. [Sonar Deep Research model](https://docs.perplexity.ai/docs/sonar/models/sonar-deep-research) — наблюдалось 10.08.2026.  
3. [API Quickstart](https://docs.perplexity.ai/docs/getting-started/quickstart) — наблюдалось 10.08.2026.  
4. [Async Sonar POST](https://docs.perplexity.ai/api-reference/async-sonar-post) — наблюдалось 10.08.2026.  
5. [Async Sonar GET](https://docs.perplexity.ai/api-reference/async-sonar-api-request-get) — наблюдалось 10.08.2026.  
6. [API FAQ](https://docs.perplexity.ai/docs/resources/faq) — наблюдалось 10.08.2026.  
7. [Rate limits and usage tiers](https://docs.perplexity.ai/docs/admin/rate-limits-usage-tiers) — наблюдалось 10.08.2026.  
8. [Perplexity Changelog](https://docs.perplexity.ai/docs/resources/changelog) — наблюдалось 10.08.2026.  
9. [Agent API presets](https://docs.perplexity.ai/docs/agent-api/presets) — наблюдалось 10.08.2026.  
10. [Official MCP documentation](https://docs.perplexity.ai/docs/getting-started/integrations/mcp-server) — наблюдалось 10.08.2026.  
11. [Official MCP repository](https://github.com/perplexityai/modelcontextprotocol) — наблюдалось 10.08.2026.  
12. [Official MCP commit history](https://github.com/perplexityai/modelcontextprotocol/commits/main/) — последнее наблюдавшееся изменение 27.07.2026.  
13. [MCP timeout issue #110](https://github.com/perplexityai/modelcontextprotocol/issues/110) — 19.05.2026.  
14. [MCP long-running operations proposal #1391](https://github.com/modelcontextprotocol/modelcontextprotocol/issues/1391) — 26.08.2025.  
15. [Official CLI documentation](https://docs.perplexity.ai/docs/cli/overview) — наблюдалось 10.08.2026.  
16. [Official CLI repository](https://github.com/perplexityai/perplexity-cli) — наблюдалось 10.08.2026.  
17. [CLI v0.2.3 release](https://github.com/perplexityai/perplexity-cli/releases/tag/v0.2.3) — 30.07.2026.  
18. [jsonallen/perplexity-mcp](https://github.com/jsonallen/perplexity-mcp) — наблюдалось 10.08.2026.  
19. [jacob-bd/perplexity-web-mcp](https://github.com/jacob-bd/perplexity-web-mcp) — наблюдалось 10.08.2026.  
20. [perplexity-web-mcp-cli on PyPI](https://pypi.org/project/perplexity-web-mcp-cli/) — v0.14.7, 28.07.2026.  
21. [Subscription plan comparison](https://www.perplexity.ai/help-center/en/articles/11187416-which-perplexity-subscription-plan-is-right-for-you) — обновлено 22.07.2026.  
22. [Perplexity Max FAQ](https://www.perplexity.ai/help-center/en/articles/11680686-perplexity-max) — обновлено 16.07.2026.  
23. [Enterprise pricing and billing FAQ](https://www.perplexity.ai/help-center/en/articles/10352986-enterprise-pricing-and-billing-frequently-asked-questions) — обновлено 22.07.2026.  
24. [How credits work](https://www.perplexity.ai/help-center/en/articles/13838041-how-credits-work-on-perplexity) — обновлено 06.08.2026.  
25. [Advanced Deep Research](https://www.perplexity.ai/help-center/en/articles/13600190-what-s-new-in-advanced-deep-research) — обновлено 16.07.2026.  
26. [Perplexity Terms of Service](https://www.perplexity.ai/hub/legal/terms-of-service) — обновлено 23.01.2026.  
27. [Getting started with Comet](https://www.perplexity.ai/help-center/en/articles/11172798-getting-started-with-comet) — обновлено 16.07.2026.  
28. [Comet policies and controls](https://www.perplexity.ai/help-center/en/articles/13529668-comet-policies-and-controls) — обновлено 16.07.2026.  
29. [Cloudflare and VPNs](https://www.perplexity.ai/help-center/en/articles/10354884-cloudflare-vpns) — обновлено 16.07.2026.  
30. [VPS/Cloudflare issue](https://github.com/diegosouzapw/OmniRoute/issues/2459) — 21.05.2026.  
31. [DeepResearch Bench](https://arxiv.org/abs/2506.11763) — 13.06.2025.  
32. [DeepResearch Bench PDF](https://deepresearch-bench.github.io/static/papers/deepresearch-bench.pdf) — 2025.  
33. [ResearcherBench](https://arxiv.org/abs/2507.16280) — 22.07.2025.  
34. [DeepResearch Bench II](https://arxiv.org/abs/2601.08536) — 13.01.2026.  
35. [DeepResearch Bench II leaderboard](https://agentresearchlab.com/benchmarks/deepresearch-bench-ii/index.html) — наблюдалось 10.08.2026.  
36. [LiveResearchBench](https://arxiv.org/abs/2510.14240) — октябрь 2025.  
37. [LiveResearchBench leaderboard](https://livedeepresearch.github.io/) — наблюдалось 10.08.2026.  
38. [DRACO paper](https://arxiv.org/abs/2602.11685) — 12.02.2026.  
39. [DRACO PDF](https://r2cdn.perplexity.ai/pplx-draco.pdf) — февраль 2026.  
40. [CJR/Tow Center citation study](https://www.cjr.org/tow_center/we-compared-eight-ai-search-engines-theyre-all-bad-at-citing-news.php) — 06.03.2025.  
41. [Citation URL hallucinations study](https://arxiv.org/abs/2604.03173) — 03.04.2026.  
42. [SDR intermittent no-search report](https://community.perplexity.ai/t/sonar-deep-research-intermittently-fails-to-activate-web-search-via-the-api/4264) — 21.03.2026.  
43. [Async SDR empty-citations report](https://community.perplexity.ai/t/sonar-deep-research-async-returns-empty-citations-search-results-despite-num-search-queries-10-full-content-no-source-attribution-2026-06-13/5401) — 18.06.2026.  
44. [Sonar empty-search-results report](https://community.perplexity.ai/t/sonar-pro-and-sonar-reasoning-pro-returning-empty-search-results-across-all-queries-2026-05-01/5074) — 05.05.2026.  
45. [Gemini Deep Research API](https://ai.google.dev/gemini-api/docs/deep-research) — обновлено 14.07.2026.  
46. [OpenAI o3 Deep Research](https://developers.openai.com/api/docs/models/o3-deep-research) — наблюдалось 10.08.2026.  
47. [OpenAI o4-mini Deep Research](https://developers.openai.com/api/docs/models/o4-mini-deep-research) — наблюдалось 10.08.2026.
