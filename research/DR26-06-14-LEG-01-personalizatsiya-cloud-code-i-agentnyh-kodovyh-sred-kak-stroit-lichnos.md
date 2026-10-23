---
dr_id: DR26-06-14-LEG-01
title: "Персонализация Cloud Code и агентных кодовых сред: как строить 'личность' AI-агента безопа"
date: 2026-06-14
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-06-14-LEG-01): Персонализация Cloud Code и агентных кодовых сред: как строить 'личность' AI-агента безопасно

> Как проектировать персонализацию (tone, память, ritual, UX-cues) облачного кодового агента так, чтобы повышать вовлечённость разработчиков, не скатываясь в манипулятивные dark patterns.

## Ключевые выводы
- Индустрия строит 'личность' агента не через один system prompt, а через слоистую конфигурацию: repo instructions + user preferences + long-term memory + task prompts + tool permissions + org policy — видно на GitHub Copilot, Cursor (.cursor/rules), Claude Code (CLAUDE.md/.claude/rules), Windsurf, Continue, Gemini CLI.
- Claude Code docs прямо разделяют мягкий контекст (CLAUDE.md/instructions) и жёсткий enforcement (hooks) — personality не должна обходить safety; при этом транскрипты и история хранятся в plain text без шифрования at rest.
- GitHub Copilot: приоритет личных инструкций выше repo, repo instructions видны в references конкретного ответа — паттерн прозрачности источника персонализации.
- CASA-теория (Nass, Moon) и relational-agents литература (Bickmore, Picard) показывают: люди автоматически применяют социальные ожидания к агентам; память + повторяемость + умеренное self-disclosure усиливают вовлечённость, но избыточная 'интимность' воспринимается как intrusive и близка к dark patterns.
- Reward prediction error (Schultz): дофаминовый эффект даёт не постоянная награда, а разница между ожиданием и результатом — редкие уместные positive surprises полезнее, чем constant reward; variable reward schedules эффективны, но попадают в зону манипуляции по FTC/OECD dark-pattern логике.
- DORA-вывод: AI усиливает уже существующие сильные или слабые стороны инженерной системы, а не компенсирует их — тестировать persona лучше на командах с хорошим CI/delivery-baseline, иначе эффект personality нельзя отделить от хаоса платформы.
- Explicit user-set preferences должны иметь приоритет над inferred/model-guessed предпочтениями; ошибочная inferred память ('ты любишь X') опаснее отсутствия памяти, разрушает доверие быстрее.
- Оценка продукта требует гибрида метрик, не одной adoption-цифры: HEART/HaTS (UX), SPACE и DORA (инженерный контекст), SUS/UMUX-Lite (usability), NASA-TLX (workload), WHO-5 (wellbeing) + trust-метрики (memory opt-out/delete rate, 'felt manipulated').
- Replika — полезный, но опасный референс: companion-формат с полной memory-персонализацией создаёт эмоциональную привязанность, что для dev-инструмента этически рискованно (агент не должен подменять социальную привязанность).

## Рекомендации / решения
- Строить persona spec как отдельный версионируемый YAML-артефакт (tone, behavior, memory, rituals, guardrails, telemetry, localization) вместо одного prose-файла — чтобы его можно было lint'ить, diff'ить и откатывать.
- Строго разделять слои: policy (жёсткое, hooks) → persona (мягкое, prompt) → memory → UX post-processing; persona не должна иметь право менять tool permissions или safety thresholds.
- Запускать opt-in explicit memory раньше auto/inferred memory; обязательные view/edit/delete/disable/temporary-session controls.
- Добавлять reply transparency — короткую пометку в ответе, что использовалась память или repo-правило, повлиявшее на решение.
- Delight/celebration — только на milestone-уровне, не на каждое действие; никаких streak/badge-механик, завязанных на длительность сессии (высокий риск compulsive use).
- Проверять persona-изменения через eval harness (regression set, safety/style assertions, trajectory checks) и постепенный rollout 5%→25%→50%→100% с заранее зафиксированными stop-критериями.
- Локализовать не текст персоны, а её поведенческий эффект (отдельный localization pack на язык/культуру), не просто переводить prompt.
- После rollout вести persona governance council (продукт + платформа + дизайн + security) каждые 2–4 недели, отслеживая replay failures, wellbeing deltas и memory drift.

## Сущности
- **Люди:** Nass, Moon, Bickmore, Picard, Schultz, Kohavi
- **Компании:** GitHub, Cursor, Anthropic, Windsurf, Continue, Google, Cline, Letta, LangGraph, Replika, OpenAI
- **Продукты/инструменты:** Claude Code, CLAUDE.md, .cursor/rules, AGENTS.md, .github/copilot-instructions.md, .continue/rules, GEMINI.md, .clinerules, Windsurf Memories & Rules, LangGraph Store/checkpointer, Letta memory blocks, OpenAI Evals, Promptfoo, OpenTelemetry, HEART, SPACE, DORA, SUS/UMUX-Lite, NASA-TLX, WHO-5, ICU MessageFormat, Unicode CLDR, i18next

## Открытые вопросы
- Не определён целевой surface агента (terminal / IDE plugin / web console / multi-surface) — от этого зависит глубина microinteractions и тип телеметрии.
- Не определён privacy posture / regulatory contour (regulated enterprise vs обычная команда) — сужает допустимый набор memory-фич.
- Не определена культура конкретной команды (комфортна ли лёгкая персональность или предпочтителен максимально сухой стиль).
- 'Dopamine for developer' нельзя измерить продуктовыми метриками напрямую — на практике измеряют только proxy: delight, retention, workload, wellbeing.

## Источник
- DR-ID `DR26-06-14-LEG-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- persona-as-code
- agent-memory-architecture
- claude-code-customization
- dark-patterns-ux
- CASA-theory
- dora-metrics
- prompt-evals
- second-brain-northstar
