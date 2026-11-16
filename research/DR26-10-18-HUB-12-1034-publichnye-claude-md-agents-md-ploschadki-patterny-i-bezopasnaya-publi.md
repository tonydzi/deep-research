---
dr_id: DR26-10-18-HUB-12-1034
title: "Публичные CLAUDE.md/AGENTS.md: площадки, паттерны и безопасная публикация"
date: 2026-10-18
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-10-18-HUB-12-1034): Публичные CLAUDE.md/AGENTS.md: площадки, паттерны и безопасная публикация

> Где владельцы публикуют файлы инструкций для агентов, какие паттерны в них работают и как безопасно опубликовать очищенную версию наших правил с проверяемой канарейкой.

## Ключевые выводы
- Дом публикации уже есть: github.com/tonydzi/claude-bible (MIT), релиз v0.1.9 от 2026-10-18 содержит очищенную публичную версию 20 основных правил. Новый репозиторий не нужен; внутренний файл целиком публиковать нельзя (личные пути, идентификаторы, устройство приватной системы).
- Проверено 13 площадок/источников: наш claude-bible, Gist (jerdaw), GitHub code search по path:AGENTS.md, agents.md, awesome-agent-conventions, awesome-agents-md (marco-trotta1 и Ischca), awesome-claude-code, awesome-claude-md (YqqLikeGit и TakatoPhy), awesome-cursorrules, Cursor Directory, claude.md. tairov/awesome-agents.md архивирован (только чтение), claude.md редиректит на документацию вендора. Открытая форма подачи не равна принятию.
- awesome-claude-code принимает рекомендации только человеком через web-форму (не gh, не PR), условие: минимум 14 дней и активность либо 100 stars. Агентом подавать нельзя.
- Прочитано 9 содержательных файлов на зафиксированных commit SHA (OpenAI Agents SDK, FastMCP, mcp-go, Supabase, Bun, Gemini CLI, Airflow, Temporal, LangGraph); Supabase CLAUDE.md содержит только `@AGENTS.md` (адаптер), у LangGraph CLAUDE.md отдаёт 404. Лицензии по GitHub API: MIT (Agents SDK, mcp-go, LangGraph), Apache-2.0 (FastMCP, Supabase, Gemini CLI, Airflow, Temporal), у Bun NOASSERTION (нужно читать LICENSE отдельно). Публично читаемое не значит лицензированное для копирования.
- Полезные паттерны из реальных файлов: security checklist и проверка документов по типам (OpenAI), команды плюс выбор существующего скилла (FastMCP), три уровня проверки: один тест / пакет / всё (mcp-go), вложенные инструкции, генерируемые файлы и path-filtered CI (Supabase), сборка с изменённым кодом (Bun), дорогая итоговая проверка один раз (Gemini CLI), команда неотделима от окружения (Airflow), контракт совместимости API и review checklist (Temporal), карта зависимых модулей (LangGraph).
- Эмпирика неоднозначна: Lulla et al. (arXiv:2601.20404v2; 10 репозиториев, 124 PR) сообщают меньшее медианное время и output tokens с AGENTS.md; Gloaguen et al. (arXiv:2602.11988v3) показывают, что лишние инструкции могут давать дополнительные затраты без общего улучшения. Универсальный вывод «короче = лучше» недопустим; пересчёт статистики не делался.
- Переносимость определяется клиентом, а не именем файла: Claude Code читает AGENTS.md напрямую с v2.1.277, но зависит от наличия CLAUDE.md и настройки Project instructions; существующий адаптер `@AGENTS.md` допустим. Размер live CLAUDE.md 90380 байт против зеркала Codex 98280 байт не доказывает ни смысловое расхождение, ни фактически загруженный контекст.
- Практический выход: draft PR №10 в claude-bible (commit 351e82a2), ровно три файла (README.md, docs/publishing-agent-instructions.md, templates/project-agent-instructions.md). verify_candidate.py вернул rc=0 (10 снимков, 3 файла, negative control сработал); независимое ревью Claude Opus — APPROVE. Удалённый CI не настроен («no checks reported»), merge и release не выполнялись.
- Внешние рельсы DR: кворум 4/6 максимальных режимов НЕ достигнут. ChatGPT Pro — нативный DR не доказан; Claude Research идёт; Mistral — weak-нога (отчёт 50531 байт, ошибочное обобщение «прямых исследований нет» исключено, порог «ядро ≤8KB» не принят); GLM — capacity; Gemini — недоступен в стране; Grok Heavy требует оплаты (не платили). На диске 1 из 6 ног.

## Рекомендации / решения
- Использовать существующий claude-bible как единственный источник истины; обновлять его небольшим PR, публичный экспорт строить из проверенного allowlist правил, а не публиковать приватный документ дословно.
- Дополнить claude-bible руководством по публикации, сравнением рабочих файлов, проверкой загрузки и заполняемым шаблоном (draft PR №10); слияние только после независимого ревью и человеческого approval.
- Gist использовать только как короткий самостоятельный пример со ссылкой на основной репозиторий, без второго канона.
- Подачи в каталоги делать адресно и только после чтения актуального CONTRIBUTING: awesome-agent-conventions, awesome-agents-md (marco-trotta1), awesome-claude-md (YqqLikeGit, TakatoPhy). В awesome-claude-code не подавать агентом, только человеком через форму. В awesome-cursorrules отправлять лишь отдельную переносимую .mdc-адаптацию. Веер заявок не делать.
- Не заимствовать абсолютные лимиты «N правил/строк для любой модели», чужие модели/команды, автоматическое разрешение исходящих действий и переписывание owner-правил агентом.
- Внедрять улучшения как обратимую канарейку: baseline и candidate, одинаковые задачи в свежих сессиях минимум двух клиентов (обычная задача, вложенный репозиторий, missing dependency, генерируемый файл, чужая инструкция в данных, граница Tier-2, отчёт о проверках), повторы, критерии приёмки заранее (нет потери требований и нарушений гейтов, задача выполнена, evidence корректно; токены и время вторичны), откат на baseline.
- Для платформенных утверждений указывать client/version/date и источник проверки; различать простой pointer, unconditional import и условный loader; мерить реально загруженные файлы и байты у каждого клиента.
- Канон не менять самим: правку живого CLAUDE.md и вынос длинных историй инцидентов в detail-слой предлагать владельцу и проверять на канарейке.
- Довести внешний DR до кворума: дождаться Claude, добрать ноги через пиров (запрос #public-agent-instructions-20261018), не отмечать applied/delivered до проверки режима каждой ноги.

## Сущности
- **Люди:** Lulla, Gloaguen, Антон
- **Компании:** OpenAI, Prefect, Supabase, Bun (Oban/oven-sh), Google, Apache Airflow, Temporal, LangChain, Anthropic, Cursor
- **Продукты/инструменты:** claude-bible, CLAUDE.md, AGENTS.md, GEMINI.md, .cursorrules, agents.md, awesome-claude-code, awesome-agent-conventions, awesome-agents-md, awesome-claude-md, awesome-cursorrules, Cursor Directory, GitHub Gist, GitHub code search, Claude Code, Codex, Gemini CLI, FastMCP, mcp-go, OpenAI Agents SDK, LangGraph, verify_candidate.py, collect_sources.py, primary-sources.json, /codex-review

## Открытые вопросы
- Улучшает ли очищенная публичная версия реальное поведение агентов: живой пилот и эксперимент с baseline/candidate не проведены, выигрыш в токенах и времени не заявлен.
- Какой контекст фактически загружают разные клиенты (размер live CLAUDE.md против зеркала Codex, роль адаптера `@AGENTS.md`): не измерено.
- Принимают ли каталоги (awesome-agent-conventions, awesome-agents-md, awesome-claude-md, Cursor Directory) наши заявки и как быстро: внешние заявки не отправлены, активность владельцев не доказана; формат добавления в Cursor Directory требует проверки.
- Лицензия Bun (NOASSERTION) и фактическая свежесть правок именно instruction-файлов (pushed_at относится ко всему репозиторию) не подтверждены.
- Кворум 4/6 максимальных режимов DR не набран: итоговые отчёты ChatGPT/Claude не получены, GLM/Gemini/Grok недоступны; режим нативного Deep Research у ChatGPT не доказан.
- Нет проверки визуального отображения дашборда (file:// запрещён политикой браузера) и нет удалённого CI для PR №10.
- Полный статистический пересчёт работ Lulla et al. и Gloaguen et al. не выполнялся, прямое сравнение их постановок остаётся открытым.

## Источник
- DR-ID `DR26-10-18-HUB-12-1034` · реестр _DR-Registry
- оригинал: «внутренний архив лаборатории»
- оригинал: `E:\!CLAUDE-PaloAltoPC-June26\scratchpad\public-agent-instructions-20261018\research-report.md`

## Связано
- claude-bible публичный репозиторий
- AGENTS.md vs CLAUDE.md переносимость
- allowlist-экспорт публичной версии канона
- обратимая канарейка и независимое ревью
- лицензирование и атрибуция чужих instruction-файлов
- длина инструкций против качества агента
- мультивендорный Deep Research и кворум 4/6
- безопасность и границы доверия в agent instructions
