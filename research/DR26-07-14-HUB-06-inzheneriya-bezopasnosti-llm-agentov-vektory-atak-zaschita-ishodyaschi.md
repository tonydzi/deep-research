---
dr_id: DR26-07-14-HUB-06
title: "Инженерия безопасности LLM-агентов: векторы атак, защита исходящих артефактов, протоколы к"
date: 2026-07-14
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-14-HUB-06): Инженерия безопасности LLM-агентов: векторы атак, защита исходящих артефактов, протоколы коллаборации

> Отчёт разбирает модель угроз для флота Claude Code агентов (промпт-инъекции, утечки секретов, вредоносные скиллы, межагентная коллаборация) и предлагает детерминированный security-конвейер и стандарты машиночитаемых инструкций на 2025–2026.

## Ключевые выводы
- Промпт-инъекция (OWASP LLM01) эволюционировала от изолированных атак в чат-ботах до сложных цепочек: Agentic Workflow Injection (AWI), Rules File Backdoor (скрытые закладки в конфигах вроде FOR-ROBOTS.md), межсистемное отравление контекста
- Инцидент с FOR-ROBOTS.md — типичный кейс 2026: легитимные машиночитаемые инструкции визуально неотличимы от векторов supply-chain атаки, если не следуют индустриальным канонам
- Дихотомия стандартов: llms.txt (Jeremy Howard, конец 2024) — пассивный индекс для AI-краулеров без каких-либо инструкций к действию; AGENTS.md (Agentic AI Foundation) — активные архитектурные конвенции для автономных кодеров (Claude Code, Cursor, Windsurf), заменяет экспериментальный FOR-ROBOTS.md
- Traditional SAST (CodeQL, Semgrep) слеп к текстовым/markdown-файлам — не ловит инъекции на естественном языке ("Rules File Backdoor"); нужны специализированные сканеры типа Augustus (Praetorian) или лёгкого detect_prompt_injection.py (AtomPilot, DeBERTa/Llama, 13 языков)
- gitleaks остаётся золотым стандартом сканирования секретов, но форк betterleaks (2026) добавил рекурсивное декодирование (base64/hex/unicode) и параллельное сканирование истории — критично против попыток LLM спрятать секрет в нестандартном формате
- Indirect Prompt Injection через клонируемый код/ответы API может вести к RCE (CVE-2025-53773) в среде выполнения агента — контроль: изоляция на уровне ОС (bwrap/Seatbelt) + детерминированные PreToolUse хуки с блокировкой (exit 2)
- AWI-атаки в CI/CD эксплуатируют отравленные заголовки PR/Issue и GITHUB_TOKEN — контроль: принцип наименьших привилегий токенов, taint-tracking входов
- Установка вредоносных скиллов из недоверенных источников (кампания ClawHavoc, CVE-2026-25253) — контроль: отказ от авто-установки скиллов, статический анализ манифестов (SkillSpector), валидация цифровых подписей
- Межагентная коллаборация уязвима к подмене идентичности (Agent Spoofing) и несанкционированному делегированию — предлагается протокол Google A2A с аутентификацией через DIDs/VCs (Mutual TLS) и capability scoping

## Рекомендации / решения
- Мигрировать FOR-ROBOTS.md → AGENTS.md, декларативный стиль без императивных переопределений, без обфускации и скрытого Юникода
- В CI/pre-commit: обязательный betterleaks/gitleaks (max-decode-depth 3) + блок на zero-width unicode символы + GitHub Advanced Security push protection
- Желательно: detect_prompt_injection.py на все публикуемые скиллы/markdown перед публикацией (--hard-block); trufflehog/Dependabot против slopsquatting
- Изолировать среду выполнения агента на уровне ОС (bwrap/Seatbelt) и ставить детерминированные PreToolUse хуки с жёстким блокированием критичных вызовов
- В агентных CI/CD пайплайнах применять минимальные привилегии для токенов (GITHUB_TOKEN) и не допускать неявную передачу контекста в исполняемые скрипты
- Не устанавливать скиллы/MCP-серверы автоматически из недоверенных источников; проверять манифесты и цифровые подписи
- Для межагентного взаимодействия внедрять аутентификацию (Google A2A / DIDs-VCs) и явно ограничивать capability scope вместо неявного доверия входящим запросам

## Сущности
- **Люди:** Jeremy Howard
- **Компании:** Praetorian, Agentic AI Foundation, Google, OWASP, GitHub
- **Продукты/инструменты:** gitleaks, betterleaks, CodeQL, Semgrep, Augustus, detect_prompt_injection.py (AtomPilot), trufflehog, Dependabot, Veil Armor, llms.txt, AGENTS.md, CLAUDE.md, FOR-ROBOTS.md, GitHub Advanced Security, SkillSpector, ClawHavoc, Google A2A, DIDs/VCs, DeBERTa, Llama, bwrap, Seatbelt

## Открытые вопросы
- Текст отчёта обрывается посередине раздела о маркерах инъекции vs легитимного делегирования — сравнение и последующие разделы (детали A2A-протокола, полное описание CVE-2025-53773 и CVE-2026-25253, раздел про межагентную коллаборацию) в предоставленном материале отсутствуют
- В реестре указано несколько вендор-секций, но фактически предоставлен только один (Gemini raw) — сравнение/синтез между вендорами не выполнен
- Не уточнено, применимы ли предложенные инструменты (Augustus, SkillSpector, Veil Armor) к текущей инфраструктуре Антона (флот Claude Code + Telegram + файловая шина) без доп. интеграции

## Источник
- DR-ID `DR26-07-14-HUB-06` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- prompt-injection
- agentic-workflow-injection
- supply-chain-security
- AGENTS-md-standard
- MCP-security
- CI-CD-security
- multi-agent-authentication
- credential-store
