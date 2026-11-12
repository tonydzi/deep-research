---
dr_id: DR26-08-28-MACANTON-01-1114
title: "Dual-agent Codex CLI + Claude Code над общим волтом/монорепо: инструкции, конкурентность,"
date: 2026-08-28
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-08-28-MACANTON-01-1114): Dual-agent Codex CLI + Claude Code над общим волтом/монорепо: инструкции, конкурентность, память

> Исследование выясняет, как безопасно совмещать Codex CLI и Claude Code над одним недокументированным Python-монорепо и non-git волтом Антона — механику загрузки инструкций, hooks, конкурентность записи и bootstrapping legacy-кода.

## Ключевые выводы
- Codex CLI переписал загрузчик инструкций (agents_md.rs): project_root_markers теперь реально работает (issue #12128 исправлен), можно указать не-.git маркер для non-git волта Антона
- Бюджет обрезки AGENTS.md (project_doc_max_bytes, default 32KiB) — ОБЩИЙ кумулятивный лимит на все файлы разом, а не per-file; глобальный ~/.codex/AGENTS.md учитывается первым, самые глубокие/специфичные файлы обрезаются первыми при переполнении — раздутый глобальный файл активно вреден
- Ни один CLI-вендор (Codex, Claude Code, Copilot CLI, Meta Muse) не выпустил cross-vendor файловую блокировку июнь-август 2026; вся индустрия сошлась на git-worktree изоляции, которая НЕ подходит для non-git Syncthing-волта — существующая advisory lease-доска Антона остаётся state-of-the-art решением
- Codex subagents (multi_agent v2) делят ОДНУ рабочую директорию — параллельные записи коллидируют (issue #23095 открыт); нет worktree-изоляции для CLI subagents (в отличие от Desktop-приложения)
- Codex PreToolUse hook срабатывает ТОЛЬКО на Bash/shell-инструмент (не на Edit/Write/Read/MCP) и может только запретить, не модифицировать — в отличие от Claude Code, где matcher шире и поддерживается updatedInput
- Научный спор о пользе AGENTS.md: Gloaguen et al. (arXiv:2602.11988, ETH Zurich) — контекстные файлы НЕ улучшают success rate, но увеличивают стоимость inference на >20%; Lulla et al. (arXiv:2601.20404) — куррированные AGENTS.md снижают runtime на 28.64% и токены на 16.58% при том же completion rate; консенсус — минимальные, человеком написанные файлы
- Claude Code НЕ читает AGENTS.md нативно (issue #6235, 5270+ реакций, Anthropic подтвердил в мае 2026 'not planned for now') — обходной мост: @AGENTS.md импорт в CLAUDE.md или symlink
- Cross-vendor session reuse невозможен: Codex-сессии — это внутренний JSONL rollout-формат, Claude Code его прочитать не может; рабочий паттерн — общая SQLite task-доска + handoff-документы, а не шаринг session state
- Symlinked skills пропускаются Codex CLI (issues #17344, #8943); 0.147 также пропускает symlinks при установке плагинов (#36967) — рекомендация использовать реальные директории вместо symlink-шаринга скиллов между харнессами

## Рекомендации / решения
- Для волта (non-git): держать advisory lease-доску (SQLite) как единственный сериализатор записи, добавить project_root_markers сентинел (например ["AGENTS.md"]) чтобы Codex резолвил корень волта
- Для монорепо: worktree-per-writer — Codex запускать как `codex exec -s workspace-write -a never` в отдельном worktree с trust_level="trusted", Claude Code — с флагом --worktree
- Сжать глобальный ~/.codex/AGENTS.md (у Антона он ~54KB) до минимума (только стиль/предпочтения), канон/инварианты держать в компактном корневом AGENTS.md, вложенные overrides — только где конвенции реально различаются
- Построить единый идемпотентный hook-скрипт (stdin-JSON, только exit 0/2), зарегистрировать на PostToolUse+Stop в обоих харнессах как замену CI (её нет в этом монорепо)
- При bootstrapping legacy-кода (69% docstrings, 5% тестов, без CI): приоритет — быстрые детерминированные тесты на путях, которые трогают агенты (для дешёвой self-verification), docstring-бэкфилл — низкий приоритет/оппортунистически
- Использовать repo-map/project-graph инструменты для архитектуры по запросу вместо раздувания AGENTS.md
- Мост между Claude Code и Codex: @AGENTS.md импорт в CLAUDE.md (кроссплатформенно) вместо symlink на Windows

## Сущности
- **Люди:** Gloaguen (ETH Zurich SRI Lab), Lulla, Lindenbauer
- **Компании:** OpenAI, Anthropic, GitHub (Copilot), Meta (Muse Code), mem0
- **Продукты/инструменты:** Codex CLI, Claude Code, AGENTS.md, CLAUDE.md, codex-rs/core/src/agents_md.rs, MCP, git worktree, Copilot CLI /worktree, Nx nx-ai-agents-config, AGENTbench, SWE-bench Lite

## Открытые вопросы
- project_root_markers default значение — расхождение в документации: config-sample указывает [".git"], config-advanced показывает [".git",".hg",".sl"]
- Issue #24016 (exec resume требует промпт для активных goals) — статус не переверифицирован из первичного источника
- Issue #11750 (headless fork) — статус не переверифицирован
- Стабильность steer-vs-queue фичи (Enter/Tab routing) — версия/PR не привязаны к первичному источнику
- Нет строгого 2026-бенчмарка, изолированно измеряющего эффект docstring-backfill или test-scaffolding кампаний — трактовать как недоказанное, мерить локально

## Источник
- DR-ID `DR26-08-28-MACANTON-01-1114` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- AGENTS.md
- instruction-loader budget
- git-worktree isolation
- lease-board advisory lock
- context-engineering (less-is-more)
- cross-vendor session continuity
- hooks parity Claude vs Codex
- task-2026-08-03-pilot-konveyera-autsorsa-kodinga
