# DR26-08-06-HUB-03-1725 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr_id: DR26-08-06-HUB-03-1725
rail: glm
model: GLM (chat.z.ai, анонимно, без логина)
mode: web-search + autoThink
door: browser-inapp-pane (mcp__Claude_Browser__*)

chat_url: https://приватный чат
started: 2026-08-10T23:35-07:00
collected: 2026-08-11T00:22-0700
note: заказу до кворума не хватало ОДНОЙ рельсы (были codex+grok+gemini). Прогон завершился за ночь целиком: конец на месте, список источников есть, обрыва нет.
---

Show full message

Отличный, глубоко проработанный запрос. Я провёл системное исследование рынка агент-оркестраторов, проанализировал юридические аспекты и методы работы с подписками. Ниже — полный отчёт.

1. 📊 Каталог оркестраторов: полная сравнительная таблица

Основные критерии: поддержка подписочной аутентификации (OAuth/device-flow/reuse CLI), маршрутизация по квотам, headless-режим и практические ограничения.

ИМЯ (ЛИЦЕНЗИЯ, ЗА РАЗРАБОТЧИКАМИ, ПОСЛЕДНИЙ РЕЛИЗ)
	
ANTHROPIC
	
OPENAI
	
XAI/GROK
	
GOOGLE GEMINI
	
МАРШРУТИЗАЦИЯ ПО КВОТЕ/FALLBACK
	
HEADLESS (CRON/АГЕНТ)
	
ДЁРГАЕТ ЛОКАЛЬНЫЙ CLI
	
КЛЮЧЕВЫЕ ОГРАНИЧЕНИЯ И ОТЗЫВЫ

OpenClaw (MIT, OpenClaw Inc., 2026-07-28)	✅ Claude CLI reuse (claude -p) / API key
github
+1
	✅ Codex OAuth (PKCE) / API key
openrouter
	❌ Только API key	❌ Только API key	✅ Встроенная стратегия (round-robin, least_used) через credential_pool_strategies
hermes-agent.nousresearch
	✅ Да (сервис openclaw start / ccr start)
npmjs
	✅ Да (паттерн Claude CLI reuse)
github
+1
	Требует Claude CLI версии >=2.1.206 для msg_lifecycle_v1 capability
github
+1
 . Docker-контейнеры требуют монтирования «внутренний путь лаборатории»
github
+1
 .
Claude Code Router (CCR) (MIT, musistudio, 2026-07-22)	✅ Claude CLI reuse (импорт профиля из локального Claude Code) / API key
github
+1
	✅ Codex OAuth (через GitHub Copilot token) / API key
hermes-agent.nousresearch
	✅ Grok CLI reuse / API key	✅ Gemini CLI reuse / API key	✅ Встроенная маршрутизация (по моделям, правилам, приоритетам)
npmjs
+1
	✅ Да (команда ccr serve для супервизора)
npmjs
	✅ Да (импорт профиля из Claude CLI)
github
+1
	Требует Node.js 22+. Настройка через JSON-конфиг или Web UI
npmjs
+1
 .
Hermes Agent (MIT, Nous Research, 2026-07-30)	⚠️ Только API key (Claude CLI reuse не поддерживается)
hermes-agent.nousresearch
+1
	✅ Copilot OAuth (gho_ токен) / API key
hermes-agent.nousresearch
	❌ Только API key	❌ Только API key	✅ Credential Pools с ротацией (round-robin, least_used) и fallback
hermes-agent.nousresearch
	✅ Да (CLI и gateway)
hermes-agent.nousresearch
	❌ Нет	Критический баг: Anthropic OAuth-токены для Claude Pro/Max направляются в отдельный, часто пустой extra_usage пул, что приводит к ошибкам авторизации
github
 .
Goose (Apache 2.0, Block, 2026-07-25)	⚠️ Только API key (подписка Claude Code/Anthropic не поддерживается)
goose-docs
+1
	✅ ChatGPT Subscription OAuth (Pro/Plus) / API key
goose-docs
	❌ Только API key	❌ Только API key	❌ Нет (только через внешний шлюз, например, Zuplo или TrueFoundry)
zuplo
+1
	✅ Да (CLI goose)
goose-docs
	❌ Нет	Подписка Anthropic Claude Pro/Max напрямую не поддерживается. Для Anthropic требуется API key
goose-docs
 .
Cline/Roo Code (Apache 2.0, Cline org / Zoo Code / Kilo Code, 2026-05-15 (архив))	✅ Claude Code (Subscription) reuse
docs.cline
	✅ API key / (устаревший) OAuth	❌ Только API key	❌ Только API key	❌ Нет (встроенной)	✅ Да (в VS Code / как CLI)	✅ Да (вызов claude CLI как подпроцесса)
docs.cline
	Roo Code закрыт 15 мая 2026. Используйте форки: Zoo Code, Kilo Code или Cline (рекомендация Anthropic)
buildthisnow
 . Ограничения подписки: нет стриминга токенов, ограничен prompt caching
docs.cline
 .
OpenHands (MIT, All-Hands-AI, 2026-07-20)	⚠️ Только API key (нетClaude CLI reuse)
docs.openhands
	✅ Subscription Login (ChatGPT Plus/Pro)
docs.openhands
	❌ Только API key	❌ Только API key	❌ Нет (встроенной)	✅ Да (CLI и SDK)	❌ Нет	Для Anthropic требуется Anthropic API key. Подписка Claude Pro/Max не используется напрямую
docs.openhands
 .
Zed (AGPL-3.0, Zed Industries, 2026-07-30)	⚠️ Claude Agent (встроенный) / API key
zed
+1
	✅ API key (Zed Agent)	❌ Только API key	❌ Только API key	❌ Нет (встроенной)	❌ Нет (только в IDE)	❌ Нет	Claude Agent в Zed использует собственную аутентификацию. API key для Anthropic в Zed не работает для Claude Agent
zed
 . Требуется отдельная подписка Claude Agent или API key.
Crush (MIT, Charm, 2026-07-15)	⚠️ Только API key (Anthropic API / Bedrock)
github
+1
	✅ API key	❌ Только API key	❌ Только API key	❌ Нет (только через внешние провайдеры)	✅ Да (CLI)	❌ Нет	Не поддерживает Claude Pro/Max подписку напрямую. Требуется Anthropic API key или доступ через AWS Bedrock
github
+2
 .
Warp Agent Mode (Proprietary, Warp, 2026-07-28)	✅ Claude Code CLI integration
docs.warp
+1
	✅ Codex CLI integration	✅ Grok CLI integration	❌ Только API key	✅ Встроенная маршрутизация между агентами	✅ Да (в терминале Warp)	✅ Да (вызов Claude Code/Grok/Codex CLI)	Warp не проксирует и не модифицирует сетевые вызовы Claude Code. Аутентификация обрабатывается самим Claude Code
docs.warp
 .
Aider (Apache 2.0, Paul Gauthier, 2026-07-10)	⚠️ Только API key	✅ API key	❌ Только API key	❌ Только API key	❌ Нет	✅ Да (CLI)	❌ Нет	Фокус на работе с репозиториями. Подписки не поддерживаются.
Continue (Apache 2.0, Continue Dev, 2026-07-20)	⚠️ Только API key	✅ API key	❌ Только API key	❌ Только API key	❌ Нет	✅ Да (в VS Code/JetBrains)	❌ Нет	Интеграция с IDE. Подписки не поддерживаются.
Amp (MIT, Amp, 2026-07-05)	⚠️ Только API key	✅ API key	❌ Только API key	❌ Только API key	❌ Нет	✅ Да (CLI)	❌ Нет	Фокус на многоконтекстном чате. Подписки не поддерживаются.
Kilo (MIT, Kilo-Org, 2026-07-12)	✅ Claude Code CLI reuse (форк Roo Code)
github
	✅ API key / (устаревший) OAuth	❌ Только API key	❌ Только API key	❌ Нет	✅ Да (в VS Code / CLI)	✅ Да (форк Cline/Roo)	Активно развиваемый форк Roo Code с поддержкой Claude CLI.

Ключевые выводы по таблице:

Поддержка Anthropic подписки (Claude CLI reuse): OpenClaw, Claude Code Router (CCR), Cline/Zoo Code/Kilo Code, Warp. Это ваш целевой класс инструментов.
Маршрутизация по квоте/fallback: OpenClaw, CCR, Hermes (для API-ключей) имеют встроенные механизмы. Для других требуется внешний шлюз (OpenRouter, Zuplo, TrueFoundry).
Headless-режим: Все перечисленные поддерживают (кроме Zed, который привязан к IDE).
Основной риск: Для Anthropic — блокировка за неавторизованное использование OAuth. Для OpenAI — блокировка за превышение лимитов Codex (если вы дёргаете CLI не через официально поддерживаемый путь).
2. ⚖️ Карта риска: юридическая и ToS сторона
2.1 Anthropic: что разрешено, а что нет

Ключевые документы (по состоянию на 2026-08-11):

Consumer Terms of Service (для Free, Pro, Max)
code.claude
Claude Code Legal and Compliance
code.claude
Use the Claude Agent SDK with your Claude plan (статья поддержки, обновлена 15.06.2026)
support.claude

Факты из ToS и официальных заявлений:

OAuth-токены Claude Pro/Max предназначены ТОЛЬКО для официального Claude Code (клиент Anthropic).

"OAuth authentication (Free, Pro, Max plans) is now EXCLUSIVELY for Claude Code. Developers must use API key authentication only"
reddit
 .
"Using OAuth tokens in third-party tools or unofficial integrations now constitutes a violation of Anthropic's Consumer Terms of Service"
lalatenduswain.medium
 .

Использование Claude CLI (claude -p) или Agent SDK для программного доступа допустимо, но с ограничениями.

"Anthropic's current Claude Code docs describe that mode (claude -p) as Agent SDK/programmatic usage"
github
+1
 .
"Your subscription usage limits stay the same and stay reserved for interactive use of Claude Code, ... Programmatic usage through the Claude Agent SDK, the claude -p command, ... will be billed at standard API pricing ($3/$15 per million tokens)"
support.claude
+2
 .
Важно: Anthropic приостановил внедрение отдельного кредита для Agent SDK (объявлено 15.06.2026, отложено на неопределённый срок)
github
+3
 . Это означает, что вся программаclaude -p пока списывается с подписочного лимита. Это критический момент для вашей стратегии.

Ключевое отличие, на котором строится обходной путь:

Запрещено: Встраивать OAuth-токен Claude Pro/Max в свой собственный код (т.е. создавать свой OAuth-клиент, который обращается к API Anthropic с токеном пользователя).
Разрешено: Вызывать официальный Claude Code CLI (который сам управляет аутентификацией) как подпроцесс (claude -p). Anthropic не запрещает автоматизировать сам CLI, а только неавторизованный доступ к API.
2.2 OpenAI: что разрешено, а что нет

Ключевые документы:

OpenAI Platform Usage Policies (общие правила)
ChatGPT Release Notes (информация о лимитах)
help.openai

Факты из практики и отзывов:

Codex CLI и OAuth ChatGPT Pro/Plus официально поддерживаются для некоторых агентов.

"OpenAI Codex OAuth is explicitly supported for use outside the Codex CLI, including OpenClaw workflows"
openrouter
 .
"If you have a ChatGPT Plus or Pro subscription, you can use LLM.subscription_login() to authenticate"
docs.openhands
 .

Лимиты Codex динамические и непубликуемые.

"The GPT-5 Pro limit on Pro Lite is dynamic, unpublished, and not visible"
community.openai
 .
Пользователи жалуются на "временные ограничения" без объяснения причин
community.openai
 .

2.3 Карта рисков и обнаружение
РИСК
	
ВЕРОЯТНОСТЬ
	
ПОСЛЕДСТВИЯ
	
КАК ОБНАРУЖИВАЕТСЯ
	
СМЯГЧЕНИЕ

Блокировка Anthropic за использование OAuth в стороннем агенте	Высокая (если использовать OAuth-токен напрямую)	Блок аккаунта Claude Pro/Max, потеря доступа к подписке	Anthropic анализирует User-Agent, заголовки запросов, частоту и паттерны вызовов (отличие от интерактивного Claude Code)	Использовать ТОЛЬКО паттерн Claude CLI reuse. Не хранить и не пересылать OAuth-токены.
Блокировка Anthropic за программное использование подписки	Средняя (находится в серой зоне)	Перевод на платное API-биллинг, возможное предупреждение	Анализ метрик использования (слишком много claude -p запросов, отсутствие интерактивных сессий)	Соблюдать правило "одна машина - одна подписка". Не распараллеливать claude -p с одного аккаунта на несколько узлов без явного разрешения.
Блокировка OpenAI за превышение лимитов Codex	Средняя (если дёргать CLI не через официальный путь)	Временное ограничение доступа к Codex, возможная блокировка аккаунта	Подсчёт запросов к API Codex, анализ заголовков ответов с лимитами	Использовать официальный OpenAI Codex CLI или агенты, которые официально поддерживают OAuth (OpenClaw, Hermes). Не писать свой клиент для Codex API.
Блокировка за "неавторизованное использование" (общий риск)	Низкая-Средняя	Предупреждение, временная блокировка, анализ аккаунта	Системы автоматического обнаружения аномалий (аномальные паттерны использования, нетипичные загрузки)	Минимизировать аномальное поведение. Не запускать агенты 24/7 без перерывов. Использовать official SDKs и CLI.

Ключевой вывод: Паттерн "оркестратор дёргает локальный Claude CLI" является на данный момент наиболее безопасным и легитимным способом использования Anthropic подписки в сторонних агентах. Anthropic сам это подтвердил для OpenClaw
npmjs
 . Однако это серая зона, и ToS могут измениться в любой момент.

3. 🧩 Практика распределения нагрузки между вендорами
3.1 Устоявшаяся таксономия классов работы

На основе анализа инженерных блогов и конфигов крупных команд, сложилась следующая практика распределения задач по моделям/подпискам:

Да (Анализ, Рефакторинг,
Сложная отладка)

Нет

Да

Нет

Да

Нет

Задача агента

Требуется ли
глубокий контекст
и сложное рассуждение?

🎯 Сильная модель
Claude Opus / GPT-5 Turbo
Anthropic Max / OpenAI Pro

Требуется ли
выполнение shell-команд
и простые правки?

⚡ Быстрая/дешёвая модель
Claude Haiku / Gemini Flash
 Anthropic Max / Gemini Pro

Это поиск или
работа с файлами?

🔍 Специализированная модель
Контекстные поиск/чтение
 Anthropic Max / Gemini Pro

🔄 Балансировщик / Fallback
Остаток квоты любой модели
 Anthropic Max / xAI SuperGrok

3.2 Что применимо к вашей установке (6 машин, 4 подписки)

Ваша текущая ситуация (по вашим цифрам):

Shell-команды (54%): Идеальный кандидат для Claude Haiku (быстро, дёшево, достаточно умно для команд).
Правка кода (16%): Требует Claude Sonnet/Opus (качество важно).
Чтение/поиск по файлам (12%): Claude Haiku или Gemini Flash (хватит контекста и скорости).
Вызовы MCP-коннекторов (11%): Зависит от сложности. Можно начать с Haiku, откатываться к Sonnet при ошибках.
Остальное (7%): Fallback-пул (Grok, Gemini, остаток Claude).

Рекомендуемая стратегия маршрутизации (для OpenClaw/CCR):

Основная модель (Primary): anthropic/claude-sonnet-4-6 (баланс качества и стоимости).
Модель для быстрых задач (Fast): anthropic/claude-haiku-4-5 (shell-команды, простые правки, поиск).
Модель для рассуждений (Think): anthropic/claude-opus-5 (только для сложного рефакторинга, анализа архитектуры).
Fallback-цепочка: xai/grok-2 -> google/gemini-2.5-pro -> (повторить попытку Claude).

Как измерять деградацию качества:

Метрики успешности задач: Доля задач, завершённых без ручного вмешательства (Automation Success Rate).
Время на задачу: Среднее время от старта до успешного завершения.
Количество итераций: Сколько раз агенту пришлось исправлять свой же код.
Субъективная оценка: Владелец регулярно просматривает логи сложных задач и оценивает качество.

Что можно безопасно переносить на дешёвые рельсы:

✅ Shell-команды (выполнение скриптов, сборка, тесты) → Claude Haiku / Gemini Flash.
✅ Чтение и поиск по файлам (grep, чтение конфигов, логов) → Claude Haiku / Gemini Flash.
✅ Простые правки кода (форматирование, добавление логов, мелкие фиксы) → Claude Haiku.

Что всегда откатывают назад на сильную модель:

❌ Сложный рефакторинг (изменение архитектуры, выделение общих компонентов).
❌ Отладка сложных багов (требуется глубокий контекст и рассуждение).
❌ Написание новой логики (особенно бизнес-критичной).
4. 📊 Измеримость остатка квоты: что можно снять, а что нет
ВЕНДОР
	
ПОДПИСКА
	
ЕСТЬ МАШИННО-ЧИТАЕМЫЙ ИСТОЧНИК ОСТАТКА КВОТЫ?
	
КАК СНЯТЬ (ЕСЛИ ЕСТЬ)
	
КОММЕНТАРИЙ

Anthropic	Claude Max	❌ НЕТ (прямого аналога rate_limits.primary.used_percent у Claude CLI нет)	—	Квота видна только в веб-интерфейсе Claude.ai. Нельзя программно проверить остаток до выполнения запроса. Это главный минус Anthropic подписки.
OpenAI	ChatGPT Pro (Codex)	✅ ДА	В транскрипте Codex CLI: rate_limits.primary.used_percent
openrouter
	Единственный вендор с таким удобным API. OpenClaw и CCR могут использовать это для маршрутизации.
xAI	SuperGrok	❌ НЕТ (публичной информации нет)	—	Лимиты динамические и не публикуются
reddit
 . Единственный способ — ловить ошибки 429 Too Many Requests в ответе API.
Google	Gemini Pro	❌ НЕТ (для CLI подписки)	—	Для Gemini API (платного) есть quotaUser и заголовки с лимитами
ai.google
 . Но для бесплатного CLI подписки (Google AI Pro/Ultra) лимиты фиксированные (1000 запросов/день для Pro) и нечитаемы программно
geminicli
 .

Ключевой вывод: Программно отслеживать остаток квоты Anthropic Claude Max нельзя. Это делает автоматическую маршрутизацию на основе остатка квоты невозможной для Anthropic. Вы можете только угадывать или использовать эвристики (например, считать токены сами).

5. 🚨 Три вещи, которые вы, по-моему, делаете неправильно

Основываясь на ваших цифрах и описанной установке:

❌ Недоиспользуете подписки OpenAI (Grok) и Google (Gemini).

Проблема: Вы платите $300/мес за SuperGrok и $20/мес за Gemini Pro, но их утилизация "не измерялась ни разу". Это прямые финансовые потери.
Почему это плохо: Вы упускаете возможность разгрузить Claude Max и сэкономить.
Как исправить: Немедленно интегрируйте Grok и Gemini в свой оркестратор (OpenClaw/CCR). Настройте маршрутизацию: все shell-команды и поиск → Gemini Flash / Grok. Оставьте Claude только для сложной логики.

❌ Не измеряете и не контролируете использование Anthropic Max.

Проблема: Вы "упирались в лимит Claude 4 раза за неделю", но не знаете, почему и как распределяется нагрузка внутри подписки (какая доля идёт на shell-команды, какая на код). Без данных нельзя оптимизировать.
Почему это плохо: Вы платите за подписку, но не управляете её использованием. Это как ездить на машине без спидометра.
Как исправить: Настройте сбор метрик. Логируйте каждый вызов модели (вендор, модель, класс задачи, кол-во токенов, успех/ошибка). Используйте OpenClaw/CCR для маршрутизации и анализа. Даже если Anthropic не даёт остаток квоты, вы можете сами считать токены и эмулировать лимит.

❌ Слишком сильно привязаны к одному инструменту (Claude Code) как единой точке входа.

Проблема: Вся работа идёт через Claude Code. ЕслиAnthropic заблокирует ваш аккаунт (даже по ошибке) или изменит ToS, вы остановитесь.
Почему это плохо: Это единственная точка отказа. Ваша лаборатория парализована.
Как исправить: Разделите точки входа. Используйте OpenClaw или Claude Code Router как единый оркестратор, который внутри себя может дёргать несколько агентов (Claude Code, Codex CLI, Grok CLI, Gemini CLI). Таким образом, если один вендор заблокирует вас, оркестратор автоматически переключится на другого.

Итоговая рекомендация: Ваша установка идеально подходит для перехода на OpenClaw или Claude Code Router. Они решат все три проблемы: позволят использовать все подписки, дадут контроль над метриками и обеспечат отказоустойчивость через маршрутизацию и fallback.

Sources
Generated by AI. For reference only.
Terms of Service
and
Privacy Policy