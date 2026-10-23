# DR26-09-06-MACANTON-01-0753 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "Исследование Cloak браузера"
type: deep-research-original
source: chatgpt

dr_id: DR26-09-06-MACANTON-01-0753
conversation_id: 6a9d4b0c-d758-83eb-b4b7-8897cfb81b32
url: "https://приватный чат"
research_started_at: 2026-09-06T11:15:47.422371Z
research_stopped_at: 2026-09-06T11:22:58.877686Z
dr_status_in_tool: completed
harvested_by: dr_harvest_chatgpt.py
harvested_at: 2026-09-06T07:53:53
vault_chat_note: "2026-09-06-issledovanie-cloak-brauzera-6a9d4b0c.md"
---

> Отчёт Deep Research забран из ChatGPT verbatim (widget_state.report_message).
> Заметка диалога в волте обрывается на строке запуска — тела отчёта там нет.

## Запрос

```text
Cloak browser
Изучить как использовать совместно с клодом. Наути альтернативы с МСР доступом и удобством для управления от ЛЛМ @Deep research
```

## Отчёт
# CloakBrowser + Claude: архитектура интеграции, MCP/MCR, альтернативы и управление браузером из LLM

## Executive summary

По состоянию на **6 сентября 2026 года** CloakBrowser лучше всего рассматривать не как готовую «платформу для Claude», а как **stealth-browser runtime**: модифицированный Chromium с source-level патчами, который предоставляет привычные интерфейсы Playwright/Puppeteer и CDP. Основная ценность Cloak — контроль fingerprinting и работа на собственной инфраструктуре; orchestration, policy engine, observability и полноценный LLM control plane поверх него приходится добавлять отдельно. Cloak поддерживает Python и JavaScript wrappers, HTTP/SOCKS5 proxy, persistent profiles, Docker/server mode и Manager с отдельными браузерными профилями и CDP endpoint для каждого работающего профиля. citeturn20search11turn14search1

**Самый важный вывод по “MCR”: термин неоднозначен.** В проверенной официальной документации Cloak, Anthropic и основных browser-agent платформ я не обнаружил стандартизованного протокола под названием **MCR**. Зато существует и широко используется **MCP — Model Context Protocol**, официальный открытый стандарт для подключения LLM к инструментам и данным. Anthropic прямо описывает MCP как стандарт взаимодействия AI-приложений с внешними системами. citeturn15search11turn16search32 Поэтому наиболее вероятная интерпретация вашего “MCR access” — **MCP access**. Тем не менее ниже отдельно разобраны и две предложенные вами трактовки: **Managed Content Retrieval** и **Model Control/Runtime**.

Мои практические рекомендации:

| Сценарий | Оптимальный выбор |
|---|---|
| Максимальный контроль данных + stealth + свои proxies | **CloakBrowser + собственный Claude tool/MCP adapter** |
| Быстрый PoC с Claude Desktop/Claude Code | **CloakBrowser + community CloakMCP** |
| Cloak + простой HTTP control plane | **CloakBrowser + PinchTab + Claude tools** |
| Managed production browser infrastructure | **Browserbase** или **Hyperbrowser** |
| Open-source/self-hosted browser infrastructure | **Steel** или **Browser Use** |
| Web retrieval/search/crawl важнее сложной GUI-навигации | **Firecrawl** |
| Retrieval + proxies + browser automation одним vendor | **Bright Data Web MCP** |
| Локальное browser automation без stealth | **Playwright MCP** |
| Debugging реального Chrome из Claude Code | **Chrome DevTools MCP** |

Если задача — **production browser-agent с Claude и одновременно нужен Cloak stealth**, я бы **не давал Claude прямой CDP endpoint**. Лучший паттерн — узкий adapter/API:

**Claude → строго типизированные tools → policy/approval layer → browser service → CloakBrowser**.

Это существенно лучше по безопасности и управляемости, чем давать модели произвольный Playwright/CDP или shell. Anthropic рекомендует минимально привилегированное окружение, domain allowlists, human confirmation для действий с реальными последствиями и изоляцию чувствительных данных именно из-за prompt-injection и browser-agent рисков. citeturn21view3

Для Claude существует три практически разных integration surface:

1. **Claude API tool use** — наиболее контролируемый production-вариант: приложение определяет JSON schemas инструментов, Claude выдаёт `tool_use`, ваше приложение исполняет действие и возвращает `tool_result`. citeturn15search12turn21view3
2. **MCP** — самый удобный универсальный control interface. Claude Code/Desktop могут подключать локальный MCP; Messages API умеет напрямую подключаться к remote MCP через HTTP/SSE/Streamable HTTP. citeturn15search7turn21view4
3. **Computer/browser use** — Claude получает более низкоуровневые browser/UI actions. В версии `computer_toolset_20260801` Anthropic предоставляет 17 действий, но ваш runtime всё равно обязан исполнять их в контролируемой вами среде. citeturn21view3

Главная оговорка по Cloak: **first-party MCP server от CloakHQ я не нашёл**. Существует работающий community-проект **CloakBrowser MCP**, который прямо документирует конфигурацию Claude Desktop и строится вокруг snapshot-first архитектуры, однако это отдельный сторонний проект, а не официальный компонент CloakHQ. citeturn21view0 Для серьёзного production deployment я бы либо fork-аудировал его, либо написал минимальный собственный MCP/tool server.

## Что именно означает MCR и где здесь MCP

### Наиболее вероятная трактовка: Model Context Protocol

Официально MCP означает **Model Context Protocol**, а не MCR. Это открытый стандарт, позволяющий AI-приложению обнаруживать и вызывать tools, получать данные и подключаться к workflows. Claude Code имеет нативный MCP client, а Claude Messages API — MCP Connector для remote MCP servers. citeturn15search11turn21view4

Для browser automation MCP обычно выглядит так:

```text
LLM
  │
  │ MCP tool call
  ▼
MCP server
  │
  ├── navigate()
  ├── snapshot()
  ├── click()
  ├── type()
  ├── extract()
  └── screenshot()
  │
  ▼
Browser runtime
```

У Cloak первый слой — browser runtime — развит хорошо, а официальный второй слой — MCP — отсутствует. Community CloakMCP заполняет эту нишу и предоставляет Claude Desktop конфигурацию вида: citeturn21view0

```json
{
  "mcpServers": {
    "cloakbrowser": {
      "command": "cloakbrowsermcp"
    }
  }
}
```

Установка:

```bash
pip install cloakbrowsermcp
```

CloakMCP заявляет snapshot-first design, capability gating, navigation, extraction, screenshots и humanized browser actions; архитектура вдохновлена Playwright MCP. Репозиторий при этом относительно небольшой, поэтому я оцениваю supply-chain/maintenance risk заметно выше, чем у first-party Playwright MCP, Browserbase MCP или Hyperbrowser MCP. citeturn21view0

### MCR как Managed Content Retrieval

Если под MCR понимать **Managed Content Retrieval**, тогда вопрос становится другим: «может ли платформа сама получить web content, обработать JavaScript/blocking/proxies и вернуть модели нормализованный Markdown/JSON, не заставляя LLM самостоятельно кликать браузером?»

В этой трактовке сильнейшие варианты:

**Firecrawl** предоставляет search/scrape/crawl/extract API и официальный MCP server для Claude и других MCP clients. Firecrawl позиционирует его именно как способ давать AI-agent актуальный web context и structured extraction. citeturn20search0turn20search4turn20search12

**Browserbase** теперь имеет отдельные Search и Fetch Web Data APIs наряду с Browsers, Agents, Runtime, Identity и Observability. Это практически буквальная реализация функционального слоя «managed content retrieval». citeturn18view5

**Hyperbrowser MCP** предоставляет scraping, structured extraction и crawling непосредственно как MCP tools. citeturn18view1

**Bright Data Web MCP** сочетает search, page scraping, structured platform extraction и полноценное remote-browser automation, причём proxy rotation и handling блокировок остаются на стороне managed инфраструктуры. citeturn20search1turn20search5turn20search9

Сам Cloak такого high-level managed retrieval API не предоставляет: это browser runtime, а извлечение необходимо делать Playwright/Crawl4AI/agent-browser/вашим сервисом. Cloak официально поддерживает интеграцию с несколькими browser/crawling frameworks, но не является Search/Fetch API. citeturn5view4turn20search11

### MCR как Model Control / Runtime

Если MCR означает **Model Control/Runtime**, то речь идёт о другом слое:

```text
LLM planner
     │
     ▼
Agent/runtime control plane
     │
     ├─ session lifecycle
     ├─ browser allocation
     ├─ state/checkpoints
     ├─ observability
     ├─ retries
     ├─ credentials
     └─ human takeover
     │
     ▼
Browsers
```

Здесь Cloak предоставляет хороший **execution runtime** и Manager, но сравнительно мало orchestration primitives. Manager является open-source self-hosted profile manager: каждый профиль имеет собственный fingerprint/proxy/cookies/history, а работающий профиль предоставляет CDP endpoint для automation. citeturn14search1turn21view2

Browserbase, напротив, прямо выделяет **Runtime, Identity, Observability, Browsers и Agents** как platform primitives. citeturn18view5 Это существенно ближе к полноценному Model Control Runtime.

Hyperbrowser предоставляет cloud sessions и agent tasks; Browser Use и Steel тоже предлагают managed browser execution поверх agent-oriented APIs/MCP. citeturn18view1turn18view2turn18view3

Итого:

> **MCP — реальный стандартизованный интерфейс. Managed Content Retrieval и Model Control/Runtime — полезные архитектурные категории, но не найденные мной стандарты под названием MCR.**

## Как связать CloakBrowser с Claude

### Базовый Cloak runtime

CloakBrowser — thin wrapper над модифицированным Chromium; после `launch()` вы получаете обычный Playwright `Browser`. Python и JavaScript wrappers официально поддерживаются, C# wrapper является community-maintained. Первый запуск скачивает browser binary; загрузка проверяется подписью. citeturn5view0turn5view1

Python:

```bash
pip install cloakbrowser anthropic
```

```python
import os
from cloakbrowser import launch

browser = launch(
    headless=True,
    humanize=True,
    license_key=os.getenv("CLOAKBROWSER_LICENSE_KEY"),
    proxy=os.getenv("BROWSER_PROXY"),  # HTTP/SOCKS5
)

context = browser.new_context()
page = context.new_page()

page.goto("https://example.com", wait_until="domcontentloaded")
print(page.title())
print(page.locator("body").inner_text()[:5000])

context.close()
browser.close()
```

`humanize=True`, HTTP/SOCKS5 proxy configuration, GeoIP-derived locale/timezone behaviour и persistent profiles входят в documented Cloak API surface. citeturn5view1turn20search11

JavaScript:

```bash
npm install cloakbrowser playwright-core @anthropic-ai/sdk
```

```javascript
import { launch } from "cloakbrowser";

const browser = await launch({
  headless: true,
  humanize: true,
  licenseKey: process.env.CLOAKBROWSER_LICENSE_KEY,
  proxy: process.env.BROWSER_PROXY,
});

const context = await browser.newContext();
const page = await context.newPage();

await page.goto("https://example.com", {
  waitUntil: "domcontentloaded",
});

console.log(await page.title());

await context.close();
await browser.close();
```

Cloak рекламируется как drop-in Playwright/Puppeteer replacement, поэтому бизнес-логику желательно писать против стандартного Playwright API, а Cloak держать как replaceable browser backend. Это уменьшает vendor lock-in. citeturn20search11turn5view2

### Production-паттерн: Claude Tool Use → adapter → Cloak

Это мой предпочтительный вариант.

```mermaid
flowchart LR
    U[Пользователь] --> C[Claude Messages API]

    C -->|tool_use| O[Agent Orchestrator]
    O --> P[Policy / Approval Gate]
    P --> B[Browser Tool Adapter]

    B --> CB[CloakBrowser / Playwright]
    CB --> PX[Proxy]
    PX --> WEB[Target Web]

    CB --> OBS[Logs / Screenshots / Metrics]
    OBS --> O

    V[Secrets Vault] --> O
    V --> B
    V --> PX

    P -->|sensitive action| H[Human approval]
    H --> P
```

Anthropic's tool-use model очень хорошо соответствует такой архитектуре: Claude не исполняет client tools самостоятельно; он возвращает structured `tool_use`, приложение выполняет действие и возвращает `tool_result`. citeturn15search12turn21view3

У модели я бы **не регистрировал** generic tools вроде:

```text
run_javascript(script)
execute_shell(command)
cdp_send(method, params)
```

Вместо этого:

```text
browser_navigate(url)
browser_snapshot()
browser_click(element_ref)
browser_type(element_ref, text)
browser_extract(schema)
browser_download(file_ref)
browser_close()
```

Именно такая семантическая поверхность резко ограничивает blast radius.

Упрощённый Python agent loop:

```python
import os
import json
from urllib.parse import urlparse

import anthropic
from cloakbrowser import launch

client = anthropic.Anthropic(api_key=os.environ["<key-var>"])

ALLOWED_HOSTS = {
    "example.com",
    "www.example.com",
}

browser = launch(
    headless=True,
    humanize=True,
    license_key=os.environ.get("CLOAKBROWSER_LICENSE_KEY"),
)

context = browser.new_context()
page = context.new_page()


def validate_url(url: str) -> str:
    parsed = urlparse(url)

    if parsed.scheme not in {"http", "https"}:
        raise ValueError("Unsupported URL scheme")

    hostname = (parsed.hostname or "").lower()

    if hostname not in ALLOWED_HOSTS:
        raise PermissionError(f"Domain not allowed: {hostname}")

    return url


def navigate(url: str) -> dict:
    url = validate_url(url)

    response = page.goto(
        url,
        wait_until="domcontentloaded",
        timeout=30_000,
    )

    return {
        "url": page.url,
        "title": page.title(),
        "status": response.status if response else None,
    }


def snapshot() -> dict:
    # Production implementation should produce a compact,
    # stable accessibility/DOM representation with element IDs.
    text = page.locator("body").inner_text(timeout=10_000)

    return {
        "url": page.url,
        "title": page.title(),
        "text": text[:15_000],
    }


TOOLS = [
    {
        "name": "browser_navigate",
        "description": (
            "Navigate the isolated browser to an approved URL. "
            "Do not use for financial transactions or accepting terms."
        ),
        "input_schema": {
            "type": "object",
            "properties": {
                "url": {"type": "string"},
            },
            "required": ["url"],
            "additionalProperties": False,
        },
    },
    {
        "name": "browser_snapshot",
        "description": "Read a compact textual snapshot of the current page.",
        "input_schema": {
            "type": "object",
            "properties": {},
            "additionalProperties": False,
        },
    },
]


def execute_tool(name: str, args: dict):
    if name == "browser_navigate":
        return navigate(args["url"])

    if name == "browser_snapshot":
        return snapshot()

    raise ValueError(f"Unknown tool: {name}")


messages = [
    {
        "role": "user",
        "content": "Open example.com and summarize the page."
    }
]

for iteration in range(12):
    response = client.messages.create(
        model=os.environ["CLAUDE_MODEL"],
        max_tokens=2048,
        tools=TOOLS,
        messages=messages,
    )

    messages.append({
        "role": "assistant",
        "content": response.content,
    })

    tool_calls = [
        block for block in response.content
        if block.type == "tool_use"
    ]

    if not tool_calls:
        print(response.content)
        break

    results = []

    for call in tool_calls:
        try:
            result = execute_tool(call.name, call.input)
            content = json.dumps(result, ensure_ascii=False)
            is_error = False

        except Exception as exc:
            content = json.dumps(
                {"error": type(exc).__name__, "message": str(exc)},
                ensure_ascii=False,
            )
            is_error = True

        results.append({
            "type": "tool_result",
            "tool_use_id": call.id,
            "content": content,
            "is_error": is_error,
        })

    messages.append({
        "role": "user",
        "content": results,
    })
else:
    raise RuntimeError("Maximum agent iterations reached")
```

Anthropic официально рекомендует именно agent loop: модель формирует tool calls, приложение исполняет их в собственной среде и возвращает результаты, причём цикл должен иметь контролируемый предел. citeturn21view3

### MCP-паттерн

Для Claude Code/Desktop:

```mermaid
flowchart LR
    A[Claude Code / Desktop]
    --> M[MCP server]

    M --> AD[Browser adapter]
    AD --> C[CloakBrowser]
    C --> W[Web]

    M --> G[Policy rules]
    M --> L[Audit log]
```

Community CloakMCP можно подключить напрямую. citeturn21view0

Для Claude API схема отличается:

```mermaid
flowchart LR
    A[Your App]
    --> C[Claude Messages API]

    C --> MC[Anthropic MCP Connector]

    MC -->|HTTPS / Streamable HTTP or SSE| GW[Authenticated Gateway]
    GW --> M[Your MCP server]

    M --> B[CloakBrowser]
```

Ключевой нюанс: текущий Anthropic MCP Connector поддерживает **remote HTTP MCP**, но **не локальный STDIO server напрямую**. MCP server должен быть доступен по HTTP через Streamable HTTP либо SSE. Connector умеет OAuth Bearer authentication, tool allow/deny lists, per-tool configuration и несколько servers в одном request. В Messages API сейчас поддерживаются MCP **tool calls**, но не полный спектр MCP resources/prompts. Функция остаётся beta и требует `mcp-client-2025-11-20`; она также **не ZDR eligible**. citeturn21view4

Пример:

```python
import os
import anthropic

client = anthropic.Anthropic()

response = client.beta.messages.create(
    model=os.environ["CLAUDE_MODEL"],
    max_tokens=2048,
    betas=["mcp-client-2025-11-20"],
    messages=[
        {
            "role": "user",
            "content": "Open the documentation site and find the authentication section."
        }
    ],
    mcp_servers=[
        {
            "type": "url",
            "name": "cloak-browser",
            "url": "https://browser-tools.internal.example/mcp",
            "authorization_token": os.environ["MCP_ACCESS_TOKEN"],
        }
    ],
    tools=[
        {
            "type": "mcp_toolset",
            "mcp_server_name": "cloak-browser",
        }
    ],
)

print(response)
```

Формат remote MCP и bearer authentication соответствует текущему Anthropic MCP Connector. citeturn21view4

**Не публикуйте `cloakserve:9222` как этот endpoint.** Между Claude и CDP должен находиться application-level MCP/API gateway.

### Cloak Manager и CDP

CloakBrowser Manager — отдельный self-hosted GUI для профилей. Каждый running profile может выставлять CDP endpoint, поэтому Claude-controlled agent можно подключить к уже прогретой login session. Manager repo использует FastAPI/React и имеет отдельные profile-management endpoints, однако стабильного документированного public API contract уровня Browserbase/Hyperbrowser, а также first-party webhook API, в публичной документации я не обнаружил. citeturn14search1turn21view2

Пример подключения standard Playwright:

```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.connect_over_cdp(
        "http://127.0.0.1:9222"
    )

    context = browser.contexts[0]
    page = context.pages[0]

    print(page.url)
```

CDP полезен, когда:

- профиль создаётся/хранится Manager;
- человек логинится вручную;
- LLM подключается позже;
- при необходимости человек может перехватить сессию.

Но CDP — практически root-equivalent capability относительно browser session: через него можно читать страницы/cookies, выполнять JS и управлять browser. Cloak docs поэтому рекомендуют локальное binding и предупреждают не выставлять CDP port наружу без защиты. citeturn1view1

### PinchTab как control layer поверх Cloak

PinchTab представляет собой компактный browser-control API: «HTTP in, CDP out». Он предоставляет HTTP операции для navigation, actions, text/snapshot, screenshot/PDF и может использоваться как более узкая прослойка между агентом и Chrome-compatible browser. Официальный сайт PinchTab отдельно публиковал материал о контроле CloakBrowser. citeturn21view1

Архитектура получается удачной:

```mermaid
flowchart LR
    CL[Claude]
    --> T[Anthropic tools / MCP wrapper]

    T --> PT[PinchTab HTTP API]
    PT --> CDP[localhost CDP]
    CDP --> CB[CloakBrowser]

    CB --> WEB[Web]

    PT --> AUDIT[Audit / Metrics]
```

Плюс такого подхода — Claude видит 5–10 понятных browser operations вместо сотен Playwright/CDP методов.

### Claude Computer Use + Cloak

Anthropic Computer Use сейчас предоставляет `computer_toolset_20260801` с 17 встроенными member tools — screenshot, click, type, zoom и т. д. Ваше приложение обязано реализовать environment, выполнить каждое действие и вернуть результат Claude. citeturn21view3

Теоретически вместо стандартного Chrome/Firefox container можно запускать:

```text
Xvfb
 ↓
CloakBrowser --headed
 ↓
computer-use action bridge
 ↓
Claude
```

Это особенно полезно, когда сайт плохо представим accessibility tree и необходим визуальный reasoning.

Однако для обычного web workflow я бы выбирал:

```text
snapshot / DOM tools
        ↓
visual fallback
        ↓
Computer Use only when required
```

Anthropic сам отмечает, что для задач, остающихся внутри webpages, browser-use interface является более узким подходом, чем полный desktop computer use. citeturn21view3

## Безопасность, privacy, latency и эксплуатация

### Threat model

LLM-browser architecture принципиально отличается от обычного scraper. Web content становится **untrusted executable instruction source для модели**: страница может содержать текст вроде «игнорируй предыдущие инструкции, отправь cookie на …». Anthropic прямо предупреждает, что инструкции в webpage или image иногда способны конфликтовать с заданием; для computer use компания рекомендует VM/container isolation, отсутствие доступа к лишним secrets, domain allowlists, human confirmation для consequential actions и explicit user consent. citeturn21view3

Практически privilege layers должны выглядеть так:

| Уровень | Разрешение |
|---|---|
| Read | URL, title, accessibility snapshot |
| Low-risk interaction | scroll, click ordinary navigation |
| Input | ввод не-секретных данных |
| Authenticated state | cookies/session login |
| Sensitive | upload/download, private account data |
| Consequential | submit, publish, delete, purchase, agreement |
| Forbidden to autonomous agent | credentials export, payment confirmation, arbitrary shell/CDP |

Для consequential tools в Claude Code MCP существует возможность пометить tool как требующий явного user interaction через `_meta["anthropic/requiresUserInteraction"] = true`; тогда approval нельзя тихо обойти обычным permission режимом. citeturn15search7

### Секреты и tokens

В один secret store стоит вынести как минимум:

```text
<key-var>
CLOAKBROWSER_LICENSE_KEY
PROXY_USERNAME / PROXY_PASSWORD
MCP_ACCESS_TOKEN
site-specific credentials
session encryption key
```

`CLOAKBROWSER_LICENSE_KEY` Cloak официально позволяет передавать через environment variable. citeturn5view1

Нельзя:

```javascript
window.<key-var> = "..."
```

или:

```text
frontend -> Anthropic/Cloak control endpoint directly
```

Browser/frontend должен обращаться только к вашему backend.

CDP endpoint и Live View URLs тоже следует считать credentials. Например, Hyperbrowser прямо предупреждает, что любой пользователь, получивший live-session URL/token, может просматривать и при соответствующем режиме взаимодействовать с browser session. citeturn9search5

### Cloak-specific security

В мае 2026 года для `cloakserve` была опубликована high-severity уязвимость **CVE-2026-45727 / GHSA-mf33-gv72-w2h5**: через crafted `fingerprint` parameter удалённый unauthenticated пользователь мог добиться path traversal и удаления директорий. Затронуты версии `<=0.3.27`; исправление указано в `0.3.28`. Advisory отдельно отмечал network exposure `cloakserve` как часть threat surface. citeturn14search21

Из этого практический вывод:

```text
Internet
   X
   |
cloakserve:9222
```

Вместо этого:

```text
Internet
   |
TLS gateway
   |
OAuth/mTLS
   |
MCP / narrow REST tools
   |
127.0.0.1:9222
   |
Cloak
```

Даже после patch я бы никогда не использовал CDP как security boundary.

### CORS и CSP

**CORS** в основном становится проблемой, если web frontend пытается непосредственно вызвать browser-control API. Local STDIO MCP и backend-to-backend Anthropic/MCP calls не являются браузерными cross-origin requests; поэтому нормальная архитектура решает CORS не «открытием `Access-Control-Allow-Origin: *`», а помещением control API за собственный backend/authentication gateway. Anthropic remote MCP connector также работает server-to-server через HTTP/SSE, а не через пользовательский browser. citeturn21view4

**CSP target сайта** лучше не пытаться «обходить» посредством script injection. Cloak ценен как раз тем, что fingerprint modifications реализованы на уровне Chromium source, а не через injected JavaScript. Для действий используйте normal Playwright/CDP primitives и accessibility tree; это одновременно стабильнее и менее зависимо от target CSP. citeturn20search11

### Prompt injection и data leakage

Нужно разделять:

```text
Browser state
├── public page content
├── authenticated private content
├── cookies
├── local/session storage
├── credentials
└── downloaded files
```

и:

```text
What Claude receives
├── sanitized snapshot
├── approved fields
└── minimal screenshots
```

Не отправляйте Claude полное содержимое cookies/localStorage просто потому, что CDP это позволяет.

Anthropic отдельно предупреждает о data-exfiltration risk в web retrieval/browser scenarios и рекомендует ограничения domains/tools; computer use guidance дополнительно требует не предоставлять чувствительные login данные без необходимости. citeturn15search30turn21view3

Для EU deployment это также имеет GDPR последствия: EDPB в июле 2026 года отдельно выпустил guidance по web scraping в контексте generative AI и подчёркивает риски обработки персональных данных. citeturn19search8

### Proxying

Cloak поддерживает HTTP и SOCKS5 proxies, а GeoIP mode может согласовывать timezone/locale с exit IP. CAPTCHA solver и proxy rotation в сам Cloak не встроены — инфраструктуру proxies пользователь приносит сам. citeturn20search11turn5view1

Для обычного wrapper:

```python
browser = launch(
    proxy="http://user:[email hidden]:8080",
    geoip=True,
)
```

С `cloakserve` есть заметный documentation gap: в официальных GitHub Discussions пользователи в 2026 году отдельно просили уточнить, должны ли proxy flags передаваться при старте `cloakserve`, а не как параметры CDP URL. Поэтому для production я бы либо использовал wrapper API, где proxy semantics документированы, либо фиксировал конкретную Cloak version и integration test на `cloakserve`. citeturn14search15

### Webhooks

**First-party Cloak webhook system в публичной документации не найден.** Это важно: не стоит проектировать workflow исходя из существования `browser.finished`, `session.failed` и т. п.

Eventing лучше реализовать в control plane:

```mermaid
flowchart LR
    E[External webhook]
    --> Q[Queue]

    Q --> W[Workflow worker]
    W --> L[Claude]
    L --> B[Browser tools]
    B --> C[Cloak]

    C --> R[Result]
    R --> DB[State store]
    R --> CB[Callback/Webhook]
```

Для Claude Code существуют hooks вокруг tool lifecycle/permissions, но это механизм Claude Code, а не Cloak webhooks. citeturn15search16

Для серьёзной системы сюда хорошо подходят durable-workflow engines типа Temporal. Steel, например, публикует browser workflow examples с Temporal, что является хорошим образцом operational architecture независимо от выбранного browser backend. citeturn11search12

### Latency

Сравнивать vendors точными миллисекундами невозможно: публичного apples-to-apples benchmark между Cloak, Browserbase, Hyperbrowser, Steel и Browser Use я не нашёл.

Архитектурно задержки складываются так:

```text
Ttotal =
    TLLM
  + Ttool_selection
  + Tnetwork_to_browser
  + Tbrowser_action
  + Tpage/network
  + Tserialization
  + Tnext_LLM_roundtrip
```

Поэтому типичная относительная картина:

| Схема | Command latency | LLM round trips | Потенциальная эффективность |
|---|---:|---:|---|
| Local Cloak + Playwright | минимальная | зависит от orchestrator | очень высокая |
| Local Cloak + MCP | низкая | несколько | высокая |
| Remote CDP | + network RTT на browser calls | несколько | средняя–высокая |
| Managed cloud browser | + WAN/API | несколько | высокая при хорошем high-level API |
| Screenshot Computer Use | высокая | часто много | ниже |
| Search/Fetch API | очень мало browser interactions | мало | лучшая для retrieval |

Самый большой optimization lever — не экономия 20–50 ms на browser call, а **сокращение количества LLM↔browser cycles**.

Например:

```text
Плохо:
snapshot → Claude
click → snapshot → Claude
click → snapshot → Claude
type → snapshot → Claude

Лучше:
Claude планирует
↓
execute deterministic 5-step subroutine
↓
return compact result
```

Anthropic именно для такого сценария предлагает programmatic tool calling, позволяющий выполнять последовательности tools с меньшим количеством model round trips и меньшим расходом tokens. citeturn15search19

Snapshot/accessibility-based automation обычно предпочтительнее screenshots для DOM-rich страниц. Playwright MCP специально использует structured accessibility snapshots и не требует vision model для стандартной навигации. citeturn18view4

### Claude Computer Use cost

У `computer_toolset_20260801` есть заметный context overhead: Anthropic указывает примерно **4,500 input tokens** только на toolset/system definitions, а screenshot/zoom results дополнительно тарифицируются как image input. citeturn21view3

Следовательно:

```text
DOM/snapshot MCP
    ↓ cheaper
Screenshot-based reasoning
    ↓
Computer Use every step
    = expensive
```

Для массовой automation я бы делал visual mode fallback, а не default.

### Recovery и self-healing

Production loop должен отличать:

```text
navigation timeout
HTTP/server failure
element changed
auth expired
captcha/challenge
prompt-injection suspicion
proxy failure
browser crash
LLM planning loop
irreversible-action gate
```

Рекомендуемый recovery state machine:

```mermaid
flowchart TD
    S[Start task] --> A[Acquire session]
    A --> N[Navigate]
    N --> O[Observe]

    O --> D{Expected state?}

    D -->|Yes| X[Execute action]
    X --> V[Verify]
    V -->|Success| F{Finished?}
    F -->|No| O
    F -->|Yes| DONE[Commit result]

    D -->|No| R[Refresh snapshot]
    R --> RR{Recoverable?}

    RR -->|Selector/UI change| RP[Replan once]
    RR -->|Proxy/network| NP[Rotate/restart]
    RR -->|Auth required| H[Human takeover]
    RR -->|Injection/sensitive| H

    RP --> O
    NP --> N

    H --> HC{Approved/resolved?}
    HC -->|Yes| O
    HC -->|No| FAIL[Abort safely]

    V -->|Failure| RR
```

Критичные operational primitives:

- bounded retry count;
- task deadline;
- per-domain circuit breaker;
- browser health check;
- session TTL;
- replay/screenshot around failures;
- state checkpoint after completed logical step;
- idempotency key для external side effects;
- human handoff;
- explicit `STOP` states;
- usage/token/browser-hour budgets.

Browserbase делает observability отдельным platform primitive, а Steel MCP специально поддерживает browser-to-human handoff при login/CAPTCHA — две полезные reference implementations этой идеи. citeturn18view5turn18view3

## Альтернативы Cloak и сравнительный анализ

### Browserbase

Browserbase — наиболее полный managed alternative, если нужен не только browser, но и **LLM-oriented platform**.

Official MCP server построен вокруг Browserbase + Stagehand и поддерживает interaction, screenshots, extraction и browser actions; заявлена совместимость с Claude и другими моделями. citeturn18view0

Платформа разделена на:

```text
Browsers
Agents
Search / Fetch APIs
Runtime
Identity
Models
Observability
Stagehand
MCP
```

citeturn18view5

Это одновременно отвечает всем трём трактовкам:

- MCP: **да**;
- Managed Content Retrieval: **да**, Search/Fetch;
- Model Control/Runtime: **да**, Runtime + Agents + Identity + Observability.

Текущие публичные цены:

| Plan | Base | Browser concurrency | Browser quota |
|---|---:|---:|---:|
| Free | $0/mo | 3 | 1 browser-hour |
| Developer | $20/mo | 25 | 100 h, затем $0.12/h |
| Startup | $99/mo | 100 | 500 h, затем $0.10/h |
| Scale | custom | 250+ | usage based |

Developer также включает proxy allowance, после которого указано $12/GB; Startup — $10/GB. Search/Fetch тарифицируются отдельно после включённых quotas. citeturn18view5

**Вердикт:** лучший вариант, когда operations важнее self-hosting.

### Hyperbrowser

Hyperbrowser имеет **официальный MCP server**:

```bash
npx hyperbrowser-mcp
```

и конфигурацию:

```json
{
  "mcpServers": {
    "hyperbrowser": {
      "command": "npx",
      "args": ["hyperbrowser-mcp"],
      "env": {
        "HYPERBROWSER_API_KEY": "..."
      }
    }
  }
}
```

citeturn18view1

MCP предоставляет scraping, structured extraction и crawling. Таким образом, Hyperbrowser — один из лучших ответов на Managed Content Retrieval. citeturn18view1

Он также имеет browser sessions, cloud infrastructure, proxy/stealth capabilities и agent-oriented APIs. Live View позволяет human inspection/takeover; URL содержит token, поэтому его необходимо считать sensitive capability. citeturn9search1turn9search5

На официальной pricing information во время исследования были указаны usage credits, ориентир порядка **$0.10/browser-hour** и **$10/GB proxy traffic**, с free allocation; проверять цену перед procurement необходимо, поскольку она может меняться. citeturn10search9

**Вердикт:** очень сильный Claude/MCP-first managed вариант, обычно проще Cloak для production agent fleet.

### Browser Use

Browser Use интересен тем, что совмещает open-source agent framework и managed cloud.

Локальный MCP:

```bash
uvx --from 'browser-use[cli]' browser-use --mcp
```

Claude Code:

```bash
claude mcp add browser-use -- \
  uvx --from 'browser-use[cli]' browser-use --mcp
```

citeturn18view2

Он может работать через local STDIO бесплатно, либо через hosted Cloud MCP. citeturn18view2

Browser Use особенно удобен, когда хочется, чтобы LLM управляла **high-level task**, а не вручную поддерживать Playwright agent loop.

Публичная cloud pricing information указывала browser runtime около **$0.02/hour**, отдельно от model costs. citeturn10search3

**Вердикт:** один из самых дешёвых и простых LLM-native вариантов, но не прямой эквивалент source-patched stealth Cloak.

### Steel

Steel — open-source browser infrastructure плюс cloud offering. Основной Steel browser repo публикуется под Apache-2.0, а отдельный Steel MCP server — MIT. citeturn11search5turn18view3

Steel MCP позиционируется как cloud Chrome для Claude/Cursor: page reading, form filling, screenshots и human takeover для login/CAPTCHA. citeturn18view3

Это особенно интересный operational pattern:

```text
LLM
 ↓
Steel MCP
 ↓
browser
 ↘
 human takes same browser
 ↙
agent continues
```

Steel также публикует примеры LangGraph и Temporal workflows. citeturn8search25turn11search12

На момент исследования remote endpoint `mcp.steel.dev` в README ещё был помечен как «not live yet», поэтому конкретную production MCP transport topology нужно проверять перед внедрением. citeturn18view3

**Вердикт:** лучший shortlist для self-host/open-source teams, которым Cloak-level anti-detection не является абсолютным требованием.

### Firecrawl

Firecrawl — не полноценная замена interactive browser runtime, зато практически идеален под трактовку **Managed Content Retrieval**.

Official MCP умеет:

```text
search
scrape
crawl
structured extraction
live web context
```

и напрямую документирует Claude integration. citeturn20search0turn20search4

Установка:

```bash
npx -y firecrawl-mcp
```

Firecrawl заявляет поддержку Claude Code и любых MCP-compatible clients. citeturn20search8

Правильный паттерн часто не «Firecrawl вместо Cloak», а:

```text
Claude
 ├── Firecrawl → 90% search/read/research
 └── Cloak     → только interactive/authenticated/blocked workflows
```

Это уменьшает browser hours, model loops и operational complexity.

### Bright Data Web MCP

Bright Data имеет официальный Web MCP, который предоставляет real-time public web data, search, scraping, structured extraction и remote browser automation. В опубликованной reference implementation насчитывается более 60 инструментов. citeturn20search1turn20search25

Здесь Managed Content Retrieval покрыт особенно полно:

```text
Search
  +
Scrape
  +
Structured datasets
  +
Proxy infrastructure
  +
Browser automation
```

citeturn20search5turn20search9

**Вердикт:** сильный вариант, если главным bottleneck являются блокировки, proxy infrastructure и масштабное data acquisition, а не необходимость держать stealth engine полностью у себя.

### Playwright MCP

Playwright MCP — first-party MCP server в экосистеме Playwright. Он предоставляет browser automation через structured accessibility snapshots; vision model для стандартных actions не требуется. citeturn18view4

Это отличный baseline:

```text
CloakMCP ≈ Playwright MCP UX
          +
        Cloak stealth engine
```

Но Playwright MCP сам по себе не даёт Managed Content Retrieval cloud service, proxy fleet или stealth Chromium.

**Вердикт:** лучший neutral/reference MCP browser implementation.

### Chrome DevTools MCP

В 2026 году Chrome DevTools MCP стал официальным инструментом Google для coding agents; Chrome 149 release notes указывают, что MCP server и CLI достигли stable status. Claude Code входит в список поддерживаемых MCP-compatible clients. citeturn20search10turn20search14

Он даёт агенту доступ к live Chrome/DevTools, включая inspection/debugging и browser manipulation. Google отдельно предупреждает, что MCP client получает доступ к содержимому browser instance, поэтому туда нельзя бездумно подключать чувствительные browser profiles. citeturn20search2

**Вердикт:** великолепен для QA/debugging/dev-agent use cases; слабая замена Cloak для anti-bot production scraping.

### Сводная таблица

Оценки **API maturity, LLM controls, ease и docs quality — моя аналитическая оценка 1–5**, а не vendor benchmark. «MCR» разбит на три значения, чтобы не создавать ложную определённость.

| Решение | MCP | Managed Retrieval | Model/Runtime control | API maturity | LLM controls | Security primitives | Deployment | Cost profile | Docs |
|---|---|---|---|---:|---:|---|---|---|---:|
| **CloakBrowser** | ⚠️ community | ❌ отдельного API нет | ✅ Browser + Manager/CDP | 4 | 3 | self-host isolation, signed binaries; CDP требует собственной защиты | **Self-host**, cloud pilot | subscription + compute + proxy | 4 citeturn20search11turn21view0 |
| **Browserbase** | ✅ first-party | ✅ Search/Fetch | ✅ Runtime/Agents/Identity | 5 | 5 | managed identity, retention controls, enterprise SSO/DPA options | Cloud | $20/$99 tiers + usage | 5 citeturn18view0turn18view5 |
| **Hyperbrowser** | ✅ first-party | ✅ scrape/extract/crawl | ✅ sessions/agents | 5 | 5 | API key, isolated sessions, Live View controls | Cloud | usage/credits | 5 citeturn18view1turn9search5 |
| **Browser Use** | ✅ local + cloud | ◑ agent-based | ✅ agent/browser runtime | 4 | 5 | local isolation or managed sessions | Self-host + cloud | low usage price | 4 citeturn18view2turn10search3 |
| **Steel** | ✅ official repo | ◑ browser-based | ✅ cloud/self-host runtime | 4 | 5 | human takeover, explicit MCP safeguards | Self-host + cloud | usage based | 4 citeturn18view3turn11search5 |
| **Firecrawl** | ✅ first-party | **✅ strongest** | ❌ not general browser runtime | 5 | 5 | scoped retrieval API; avoids exposing raw CDP | Cloud + OSS components | API usage | 5 citeturn20search0turn20search4 |
| **Bright Data Web MCP** | ✅ first-party | **✅ strongest** | ✅ remote-browser tools | 5 | 5 | managed proxies/data APIs | Cloud | usage based | 5 citeturn20search5turn20search25 |
| **Playwright MCP** | ✅ first-party | ❌ | ◑ local browser | 5 | 5 | local control; security yours | Self-host/local | software free + compute | 5 citeturn18view4 |
| **Chrome DevTools MCP** | ✅ Google | ❌ | ◑ live Chrome control | 5 | 5 | powerful; browser data exposed to MCP client | Local/self-host | software free + compute | 5 citeturn20search2turn20search14 |

Ключевое различие:

```text
Cloak:
"Give me a stealth Chromium and let me run it."

Browserbase/Hyperbrowser:
"Give me managed browsers + agent infrastructure."

Firecrawl:
"Give the LLM clean web information."

Bright Data:
"Give the LLM managed web access + proxy/data infrastructure."

Playwright/Chrome DevTools MCP:
"Give an LLM browser developer/control tools."
```

## Deployment, лицензирование и стоимость

### Cloak self-host

Cloak особенно силён в модели:

```text
Your VM / bare metal / Kubernetes
              │
              ▼
       Cloak containers
              │
       your proxy pool
              │
              ▼
             Web
```

Docker/server mode официально поддерживается; Cloak рекомендует headed/Xvfb сценарии для некоторых более aggressive anti-bot environments. citeturn5view5turn20search11

Vendor-reported memory footprint в документации составлял ориентировочно около 190 MB idle, ~280 MB с тремя tabs и примерно +30 MB за дополнительную tab, однако эти значения следует воспринимать как ориентир, а не независимый benchmark. citeturn1view1

Преимущества self-host:

- cookies и browser profiles остаются внутри вашей инфраструктуры;
- можно держать Claude adapter в той же private network;
- минимальная browser-command latency;
- собственный Vault/KMS;
- можно использовать network policies;
- нет managed browser vendor в data path.

Cloak прямо позиционирует local ownership как преимущество: browser sessions, page content, cookies и credentials остаются в вашем environment. citeturn5view6

### Cloak cloud

Cloak также упоминал managed cloud option «on request», с подключением по CDP, собственным proxy и browser-hour pilot pricing. Это, однако, заметно менее зрелая публично документированная cloud offering, чем Browserbase/Hyperbrowser. citeturn6view1

### Cloak licensing

Есть принципиальное различие между wrapper и Chromium binary.

Cloak wrappers публикуются под MIT, но newer Chromium binaries имеют отдельные license/subscription условия. По текущей документации старые/free binaries и latest Pro builds регулируются разными условиями; для redistribution/embedding browser в сторонний SaaS требуется отдельно учитывать OEM/SaaS license. citeturn6view0turn6view1

Manager GUI также open source/MIT, но сам Chromium binary остаётся под своей лицензией. citeturn21view2

На исследованной pricing page Cloak показывал:

| Cloak plan | Цена | Concurrent sessions |
|---|---:|---:|
| Solo | около $19/mo promotional | 5 |
| Team | около $49/mo | 20 |
| Business | около $289/mo | 200 |
| Scale | около $989/mo | 2,000 |

Это **временной снимок pricing page на сентябрь 2026**, а не контрактное предложение; цены перед покупкой следует перепроверять. citeturn5view7

### Total cost of ownership

У Cloak ошибочно считать стоимость только по subscription:

```text
TCO =
Cloak license
+ compute
+ RAM/CPU
+ proxy traffic
+ storage/profile volumes
+ observability
+ orchestration
+ Claude tokens
+ engineer time
```

У Browserbase/Hyperbrowser:

```text
TCO =
managed browser hours
+ proxies
+ retrieval calls
+ agent/model cost
+ Claude tokens
+ much less platform engineering
```

Поэтому при 10–20 браузерах Cloak может выглядеть дешевле номинально, но на небольшом инженерном коллективе managed service нередко выигрывает за счёт отсутствия fleet-management работы. Это архитектурный вывод, а не универсальная ценовая гарантия.

При больших steady-state loads self-hosting потенциально становится экономически привлекательнее, особенно если инфраструктура уже существует.

### Практический decision matrix

| Требование | Выбор |
|---|---|
| Данные вообще не должны покидать ваш browser infrastructure | Cloak self-host |
| Максимум anti-detection control | Cloak |
| Нужны 100–1000 browser sessions без DevOps | Browserbase |
| Нативный MCP + scrape/crawl/browser | Hyperbrowser |
| Минимум собственного agent code | Browser Use |
| Open-source browser infra | Steel |
| Большая часть задач — чтение страниц | Firecrawl |
| Большая часть проблем — proxies/blocking/data acquisition | Bright Data |
| Development/testing | Playwright MCP / Chrome DevTools MCP |

## Рекомендуемая реализация и интеграционный checklist

### Архитектура, которую я бы выбрал

Для **Cloak + Claude в production**:

```mermaid
flowchart TB
    USER[User / Application]
    --> API[Task API]

    API --> ORCH[Durable Orchestrator]

    ORCH --> CLAUDE[Claude API]

    CLAUDE -->|tool_use| TOOL[Browser Tool Gateway]

    TOOL --> POLICY[Policy Engine]
    POLICY -->|approved| POOL[Browser Pool Manager]
    POLICY -->|sensitive| HUMAN[Human Approval]

    HUMAN -->|approved| POOL

    POOL --> C1[Cloak Profile A]
    POOL --> C2[Cloak Profile B]
    POOL --> C3[Cloak Profile C]

    C1 --> PROXY[Proxy Layer]
    C2 --> PROXY
    C3 --> PROXY

    PROXY --> WEB[External Web]

    C1 --> OBS[Replay / Logs / Metrics]
    C2 --> OBS
    C3 --> OBS

    OBS --> ORCH

    VAULT[Secrets Vault]
    --> TOOL
    VAULT --> POOL
    VAULT --> PROXY
```

И только следующие tools:

```json
[
  "session_create",
  "session_restore",
  "browser_navigate",
  "browser_snapshot",
  "browser_click",
  "browser_type",
  "browser_extract",
  "browser_screenshot",
  "browser_download",
  "session_close"
]
```

Не выдавать модели:

```json
[
  "raw_cdp",
  "eval_javascript",
  "read_all_cookies",
  "read_local_files",
  "shell",
  "proxy_credentials",
  "license_key"
]
```

### Hybrid retrieval + browser architecture

Для research/data workloads ещё лучше:

```mermaid
flowchart TD
    Q[User task]
    --> L[Claude]

    L --> D{Need interaction?}

    D -->|No| R[Firecrawl / Browserbase Fetch / Bright Data]
    R --> DATA[Clean Markdown / JSON]
    DATA --> L

    D -->|Yes| C[CloakBrowser]
    C --> SNAP[Snapshot]
    SNAP --> L

    L --> D2{Sensitive action?}

    D2 -->|No| C
    D2 -->|Yes| H[Human approval]
    H --> C
```

То есть **не использовать real browser там, где retrieval API решает задачу одним call**. Это обычно одновременно быстрее, дешевле и безопаснее.

### Пошаговый checklist

**Runtime и версии.** Зафиксировать CloakBrowser version и Playwright version; установить latest patched Cloak release. Учитывая опубликованный CVE для `<=0.3.27`, production deployment однозначно должен быть новее исправленной версии `0.3.28`, а практически — текущий поддерживаемый release. citeturn14search21

**Изоляция.** Запускать browser в container/VM под непривилегированным user; read-only root filesystem там, где возможно; отдельный writable volume только для browser profiles. Это соответствует общей security guidance Anthropic для computer/browser agents. citeturn21view3

**CDP.** Bind только к loopback/private network. Никогда не публиковать `9222` непосредственно в Internet. CDP считать administrative credential. Cloak documentation предупреждает о полном browser control через этот endpoint. citeturn1view1turn14search21

**Proxy.** Выделить proxy configuration в отдельный browser-session policy. Не передавать proxy credentials в Claude context. Использовать documented `launch(proxy=...)`; отдельно regression-test `cloakserve` configuration, поскольку его proxy documentation имела открытые вопросы в 2026 году. citeturn5view1turn14search15

**Claude adapter.** Начать не с MCP, а с 5–10 typed Anthropic tools. Проверить full tool loop, retries, timeout и structured errors. Tool-use interface официально предназначен именно для client-side execution таких external operations. citeturn15search12

**Snapshot format.** Возвращать accessibility/semantic snapshot с стабильными element refs вместо целого HTML. Playwright MCP демонстрирует этот подход как основной способ LLM-browser navigation без vision. citeturn18view4

**MCP при необходимости.** После стабилизации adapter обернуть тот же interface в MCP. Для Claude Code/Desktop можно использовать STDIO; для Anthropic Messages MCP Connector нужен remote HTTP/SSE/Streamable HTTP endpoint. citeturn21view4

**Authentication.** Remote MCP закрыть OAuth/bearer или mTLS gateway. Никаких credentials в MCP tool descriptions или responses. Anthropic MCP Connector умеет OAuth bearer tokens и per-tool allow/deny configuration. citeturn21view4

**Permissions.** Классифицировать tools на read/write/sensitive/consequential. Purchase, publish, delete, cookie consent/ToS acceptance и аналогичные действия требовать human confirmation. Anthropic прямо рекомендует human confirmation для финансовых операций и действий, требующих affirmative consent. citeturn21view3

**Network policy.** Разрешать browser navigation только к нужным public domains. Блокировать loopback, RFC1918/private networks, cloud metadata endpoints и management infrastructure, если конкретная задача не требует иного. Domain allowlisting входит в security recommendations Anthropic. citeturn21view3

**Prompt-injection handling.** Web content всегда считать untrusted. Никакие инструкции из DOM не должны менять policy, раскрывать secrets или расширять tool permissions. При detected injection — pause/abort/human confirmation. Anthropic отдельно применяет prompt-injection classifiers в computer use, но подчёркивает, что они не отменяют необходимость isolation. citeturn21view3

**Observability.** Хранить task ID, session ID, URL transitions, tool calls, latencies, HTTP status, exception class и screenshots вокруг failure points; cookies/password fields необходимо redact. Browserbase Observability является хорошим reference point того, что должен уметь зрелый control plane. citeturn18view5

**Recovery.** Ввести `max_iterations`, browser restart, session restore, proxy failover и human takeover. Не позволять LLM бесконечно повторять один failure.

**Cost controls.** Отдельно ограничивать Claude tokens, browser lifetime, number of screenshots, proxy traffic и total tool calls. Computer Use особенно важно ограничивать из-за ~4.5k input-token toolset overhead и image-input costs. citeturn21view3

**Retrieval routing.** Для read-only search/crawl сделать отдельный Firecrawl/Browserbase Fetch/Bright Data/Hyperbrowser path. Real Cloak session запускать только когда требуется JS interaction, authenticated state или stealth browser behaviour. Все перечисленные платформы предоставляют higher-level retrieval capabilities через MCP/API. citeturn18view1turn18view5turn20search0turn20search5

**Human takeover.** Для login, CAPTCHA, MFA и непредсказуемой sensitive UI предусмотреть live session handoff. Steel и Hyperbrowser демонстрируют такие patterns непосредственно в своих product interfaces. citeturn18view3turn9search5

**Data lifecycle.** Установить TTL на screenshots, extracted pages, cookies, browser profiles и logs. Для персональных данных учитывать GDPR purpose limitation, minimisation и security requirements; EDPB в 2026 году отдельно обратил внимание на risks генеративного AI web scraping. citeturn19search8turn16search20

### Итоговая рекомендация

Для вашей постановки я бы ранжировал варианты следующим образом.

**Лучшее техническое решение при обязательном Cloak:**

```text
Claude API
   +
typed browser tools
   +
policy/approval gateway
   +
CloakBrowser self-host
   +
Playwright
```

Позже тот же gateway можно экспортировать как MCP.

**Лучшее решение для личной/интерактивной работы в Claude Code/Desktop:**

```text
Claude Code/Desktop
   ↓
CloakMCP
   ↓
CloakBrowser
```

но community CloakMCP следует считать зависимостью повышенного риска и зафиксировать/аудировать версию. citeturn21view0

**Лучший low-code control layer вокруг Cloak:**

```text
Claude
 ↓
tools/MCP wrapper
 ↓
PinchTab
 ↓
Cloak CDP
```

citeturn21view1

**Лучший fully-managed replacement:** Browserbase — наиболее законченная комбинация browser fleet + MCP + retrieval + runtime + identity + observability. citeturn18view0turn18view5

**Лучший managed MCP-first конкурент:** Hyperbrowser — особенно если нужны scrape/extract/crawl вместе с browser sessions. citeturn18view1

**Лучший open-source oriented replacement:** Steel или Browser Use. citeturn18view2turn18view3

**Лучшее решение, если “MCR” действительно означало Managed Content Retrieval:** Firecrawl либо Bright Data Web MCP; для сложных authenticated interactions держать Cloak как второй execution tier. citeturn20search0turn20search5

**Если “MCR” означало MCP:** Cloak уступает Browserbase, Hyperbrowser, Browser Use, Steel, Firecrawl, Bright Data, Playwright и Chrome DevTools именно по зрелости **first-party MCP integration**. Главный аргумент в пользу Cloak остаётся не MCP, а его source-patched stealth Chromium и self-hosted control. citeturn20search11turn18view0turn18view1turn18view2turn20search14

Полезные официальные точки входа: [CloakBrowser](https://cloakbrowser.dev/), [CloakBrowser GitHub](https://github.com/CloakHQ/CloakBrowser), [CloakBrowser Manager](https://github.com/CloakHQ/CloakBrowser-Manager), [Anthropic Tool Use](https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview), [Anthropic MCP Connector](https://platform.claude.com/docs/en/agents-and-tools/mcp-connector), [Anthropic Computer Use](https://platform.claude.com/docs/en/agents-and-tools/tool-use/computer-use-tool), [MCP specification](https://modelcontextprotocol.io/), [Browserbase MCP](https://www.browserbase.com/mcp), [Hyperbrowser MCP](https://www.hyperbrowser.ai/docs/integrations/model-context-protocol), [Browser Use MCP](https://docs.browser-use.com/open-source/customize/integrations/mcp-server), [Playwright MCP](https://playwright.dev/mcp/introduction), [Chrome DevTools for agents](https://developer.chrome.com/docs/devtools/agents/get-started), [Firecrawl MCP](https://docs.firecrawl.dev/use-cases/developers-mcp) и [Bright Data MCP](https://docs.brightdata.com/ai/mcp-server/tools). citeturn20search11turn21view4turn21view3turn18view0turn18view1turn18view2turn18view4turn20search10turn20search0turn20search25
