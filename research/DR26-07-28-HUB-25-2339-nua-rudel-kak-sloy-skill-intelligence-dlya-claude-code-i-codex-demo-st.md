---
dr_id: DR26-07-28-HUB-25-2339
title: "Nua/Rudel как слой skill-intelligence для Claude Code и Codex — демо-сценарий и риски"
date: 2026-07-28
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-28-HUB-25-2339): Nua/Rudel как слой skill-intelligence для Claude Code и Codex — демо-сценарий и риски

> Deep Research разбирает, как работает продукт Nua (наследник Rudel) — слой наблюдаемости и управления agent skills для Claude Code и Codex — и что категорически нельзя делать в демо-показе команде.

## Ключевые выводы
- Nua публично позиционируется не как отдельный агент-runtime, а как слой наблюдаемости/управления библиотекой agent skills: сканирует skill-файлы по команде и git, связывает их с сессиями, помечает работающие/неработающие, помогает распространять лучшие skills
- Nua — вероятная эволюция открытого продукта Rudel (лендинг Nua ссылается на Rudel как 'previous chapter', Rudel публично объявил о закрытии 'for what comes next'), но архитектурная документация самой Nua не опубликована
- Открытый код Rudel подтверждает механику: CLI `rudel enable` ставит hooks для Claude Code и Codex, читает локальные транскрипты (~/.claude/projects, ~/.codex/sessions), загружает ПОЛНЫЕ session-транскрипты в hosted-backend (ClickHouse)
- Официально поддерживаемые в публичном коде adapters — только claude-code и codex, других провайдеров нет
- Интеграция строится через event-hooks (Claude: SessionEnd hook; Codex: notify на agent-turn-complete) — Nua должна быть 'out-of-band telemetry', не в критическом пути исполнения агента
- README Rudel прямо предупреждает: uploaded transcripts могут содержать prompts, tool output, file contents, command output, URLs и secrets — фактически неизбежная утечка при live-demo на боевых данных
- Формула skill-scoring, API и rate limits Nua публично не задокументированы — это 'продуктовая гипотеза', а не проверяемая метрика, поэтому нельзя продавать usage/activity как эквивалент качества результата ('skill works' ≠ 'agent produced activity')
- Термин 'килы' в запросе Антона распакован как наиболее вероятно 'skills', но с равнозначными опасными альтернативными чтениями: keys/secrets, kills-как-KPI, internal artifacts — для каждого чтения свой список жёстких запретов
- Claude hooks: если несколько hooks совпадают по событию, исполняются ВСЕ — один 'deny' не отменяет side-effects другого hook'а (риск для демо-скрипта)
- Codex: ряд ключей (включая notify) игнорируется в project-local `.codex/config.toml` — Nua нельзя включить 'скрытно' через репозиторий, только через user/admin scope

## Рекомендации / решения
- Для демо использовать только synthetic/sanitized repo и сессии — никогда production-код, реальные API keys, deploy-креды, внутренние артефакты
- Показывать малую кураторскую библиотеку из 3-5 skills с чёткой границей применимости (api-testing, schema-check, deploy-staging и т.п.), explicit invocation вместо implicit
- Строить демо как двойной контур: self-hosted Rudel (полный контроль, предзагруженные synthetic-сессии) + hosted Nua-аккаунт как параллельный вариант, плюс prerecorded fallback (скриншоты/JSON) на случай сетевых сбоев
- Не подавать Nua как замену Claude/Codex или как строгий бенчмарк продуктивности — только как 'signal amplifier для skill library governance'
- Держать canonical skill source в одном репозитории с лёгкими platform-wrapper'ами под Claude (.claude/skills) и Codex (.agents/skills), маппить usage по каноническому skill ID, а не по пути файла
- Использовать read-mostly MCP и человеческое approval на write-действия; не давать демо-агенту write-capable MCP или доступ к чужим неотсортированным skill-репозиториям
- Для OpenAI-интеграций рядом с Codex — short-lived credentials (workload identity federation) вместо долгоживущих API keys

## Сущности
- **Люди:** —
- **Компании:** Nua, Rudel, Anthropic, OpenAI, NIST, OWASP
- **Продукты/инструменты:** Claude Code, OpenAI Codex, MCP, ClickHouse, SKILL.md, .agents/skills, Rudel CLI, IngestSessionInput API

## Открытые вопросы
- Публичная формула/логика skill-scoring Nua ('works or doesn't') не найдена — остаётся продуктовой гипотезой
- Точный внешний API, rate limits и SLA у Nua не задокументированы публично
- Не разрешена окончательно исходная двусмысленность 'килы' — трактовка как skills наиболее вероятна, но не подтверждена автором запроса
- Насколько глубоко Nua отличается от Rudel архитектурно (не только маркетингово) — неясно без доступа к закрытому коду/докам Nua

## Источник
- DR-ID `DR26-07-28-HUB-25-2339` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-28-HUB-25-2339-izuchenie-produkta-nua-chatgpt.md`
- оригинал: `E:\Obsidian\Anton-Knowledge\01-Conversations\ChatGPT\conversations\2026-06-23-izuchenie-produkta-nua-6a3a3cc8.md`

## Связано
- [[agent-skills-management]]
- [[claude-code-hooks]]
- [[codex-hooks]]
- [[prompt-injection-risk]]
- [[mcp-security]]
- [[skill-library-governance]]
- [[session-telemetry-ingest]]
