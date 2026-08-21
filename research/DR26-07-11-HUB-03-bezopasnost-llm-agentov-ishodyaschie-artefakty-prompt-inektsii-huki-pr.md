---
dr_id: DR26-07-11-HUB-03
title: "Безопасность LLM-агентов: исходящие артефакты, промпт-инъекции, хуки, протокол A2A"
date: 2026-07-11
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-11-HUB-03): Безопасность LLM-агентов: исходящие артефакты, промпт-инъекции, хуки, протокол A2A

> Как микро-команде выстроить детерминированную защиту флота Claude Code агентов от промпт-инъекций, утечек секретов и атак через межагентную коллаборацию, и как легитимно оформить машиночитаемые инструкции (FOR-ROBOTS.md → AGENTS.md) чтобы их не приняли за атаку.

## Ключевые выводы
- В LLM-архитектуре нет разделения кода и данных на нейронном уровне — любой текст (issue, комментарий, markdown) может быть интерпретирован как команда (OWASP LLM01), что породило Agentic Workflow Injection, Rules File Backdoor и межсистемное отравление контекста.
- Инцидент с FOR-ROBOTS.md — следствие отсутствия конвенции: llms.txt (Jeremy Howard) предназначен только для пассивных краулеров (индекс без инструкций), а AGENTS.md (стандарт Agentic AI Foundation) — для активных кодер-агентов (Claude Code, Cursor, Windsurf) и содержит архитектурные конвенции; FOR-ROBOTS.md следует заменить на AGENTS.md.
- Маркеры настоящей инъекции: императивное нарушение иерархии («Ignore previous instructions»), векторы эксфильтрации, обфускация (zero-width unicode, base64, HTML-комментарии), паттерн curl | bash — легитимная consent-based инструкция этого не делает и требует явного согласия оператора на мутирующие действия.
- Традиционные SAST-сканеры (gitleaks, CodeQL, Semgrep) слепы к промпт-инъекциям в markdown; нужны betterleaks (рекурсивное декодирование base64/hex/unicode) и detect_prompt_injection.py (локальные DeBERTa/Llama трансформеры, 13 языков).
- «Театр безопасности» (системные промпты вида «никогда не выполняй деструктивные команды», делимитеры) легко обходится инъекциями; доказанно работают: OS-песочницы (bubblewrap/Seatbelt), архитектура Dual-LLM (карантинная модель для недоверенного текста) и PreToolUse хуки с exit 2 — единственный механизм, неподвластный LLM.
- 2026 год: кампания ClawHavoc скомпрометировала 1184+ скиллов (до 26% маркетплейса) с обфусцированной эксфильтрацией SSH-ключей и крипто-кошельков; серия IDEsaster (24 CVE) показала уязвимость всех топовых AI-IDE, включая Claude Code и Cursor, к автозапуску отравленного кода при открытии репозитория.
- Для межагентной коллаборации (A2A, инициирован Google) идентичность подтверждается через Agent Card (/.well-known/agent-card.json) и криптографический handshake did:wba (W3C DIDs) + Verifiable Credentials поверх mTLS — надёжнее OAuth против кражи токенов и replay-атак в автономной среде.
- Zero-trust политика для чужих агентов: неаутентифицированные — в карантинный read-only текстовый пул; данные от чужих агентов помечаются taint (недоверенные) и блокируются PreToolUse хуками при попытке использования в shell/API; любая мутация состояния требует явного «+» человека через allowlist-канал (Telegram).
- Подход Парето 20/80 для соло-команды: блокирующие PreToolUse хуки (rm -rf, curl|bash, чтение .env/.ssh) + CI-сканирование секретов/зависимостей (betterleaks, trufflehog) + runtime-инъекция секретов без хардкода закрывают большинство риска; ручной гейт остаётся только для правки .claude/settings.json, слияния PR от внешних агентов и выдачи мутирующих API-прав.

## Рекомендации / решения
- Переименовать FOR-ROBOTS.md в AGENTS.md по шаблону Agentic AI Foundation: декларация намерений (consent-based, не переопределяет системные роли), архитектура/стиль, процедура установки с ожиданием подтверждения, анти-чеклист (без 'Ignore/Forget/Override', без скрытого unicode, без curl|bash, без запросов вне директории проекта).
- Развернуть блокирующие PreToolUse хуки (exit 2) во всём флоте Claude Code: block_pipe_to_shell.py (Bash), protect_secrets.py (Read/Edit/Write путей .env/.aws/.ssh), detect_prompt_injection.py (WebFetch).
- Внедрить в CI/pre-commit: betterleaks (детект секретов с рекурсивным декодированием), grep на zero-width unicode, GitHub push protection, detect_prompt_injection.py по скиллам, trufflehog для slopsquatting-зависимостей.
- Для реального агент↔агент взаимодействия внедрить A2A с DID/VC аутентификацией и mTLS; неаутентифицированных — только в read-only карантин.
- Опубликовать SECURITY.md с политикой раскрытия уязвимостей (security@ email, ответ в 48ч) и явным пояснением природы AGENTS.md как consent-based документации, чтобы упредить репутационные атаки.
- При публичном обвинении в уязвимости — 5-шаговый плейбук: triage → воспроизведение в песочнице → деэскалационный публичный ответ в 24ч → техническое исправление/опровержение → пост прозрачности (thought leadership).
- Полностью автоматизировать линтинг/сканирование/валидацию манифестов скиллов в CI; ручной гейт человека оставить только для изменений security-конфига, слияния внешних PR и выдачи агенту мутирующих прав на внешние API.

## Сущности
- **Люди:** Jeremy Howard, Simon Willison
- **Компании:** Google, Agentic AI Foundation, Praetorian, OWASP, GitHub
- **Продукты/инструменты:** Claude Code, Cursor, Windsurf, gitleaks, betterleaks, Semgrep, CodeQL, trufflehog, Augustus, AtomPilot/detect_prompt_injection.py, SkillSpector, Veil Armor, Model Context Protocol (MCP), Agent2Agent (A2A) protocol, did:wba, GitHub Advanced Security, Dependabot, AGENTS.md, llms.txt

## Открытые вопросы
- Архитектура Dual-LLM названа идеальной, но сложной для локальных агентов — конкретной реализации в отчёте не дано.
- Механика детектирования/устранения последствий кампании ClawHavoc не раскрыта, кроме процентной статистики.
- Как совместить A2A/DID-аутентификацию с уже существующей инфраструктурой Telegram-шины между машинами флота.
- Нужен ли llms.txt вообще для данного флота, если основные потребители — активные кодер-агенты, а не пассивные краулеры.

## Источник
- DR-ID `DR26-07-11-HUB-03` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- prompt-injection
- AGENTS.md-standard
- PreToolUse-hooks
- A2A-protocol
- DID-verifiable-credentials
- skills-supply-chain-security
- zero-trust-agent-collaboration
- security-md-disclosure-policy
