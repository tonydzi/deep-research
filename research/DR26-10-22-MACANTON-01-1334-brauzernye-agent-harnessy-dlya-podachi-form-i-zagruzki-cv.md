---
dr_id: DR26-10-22-MACANTON-01-1334
title: "браузерные агент-харнессы для подачи форм и загрузки CV"
date: 2026-10-23
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# DR26-10-22-MACANTON-01-1334: харнесс, который жмёт кнопки за Антона

Ноги на диске: chatgpt, claudeai, grok, mistral (4 из 6; gemini и glm нет). Все с URL и датами.

- Сильнейший вариант для живого залогиненного Chrome-профиля и локальной загрузки файла: Claude in Chrome через Claude Code. Авто-режим из контура убрать (default/manual + allow-правила по сайтам), перед Submit детерминированный гейт одобрения. Классификатор авто-режима блокирует и разрешённые инструменты: GitHub issues #97741, #98169.
- chatgpt ставит первым Browser Use / Browser Harness; ещё Manus Browser Operator, Stagehand+Browserbase, Skyvern для батчей.
- ChatGPT Work не умеет автозагрузку файлов; Atlas и Project Mariner закрыты.
- claudeai: Generator Residency запрещает AI-assisted письмо (учесть при подаче).
- grok честно пишет, что часть бенчмарков не открывал.

Потребитель: выбор харнесса для автоподач без ложных отказов. Связи: MOC-Deep-Research-2026-09.

Дубль-оговорка: синтез codex по тем же 4 ногам уже лежал в DR26-10-22-MACANTON-01-1334-agent-harnesses-browser-forms (04-Projects/DR), мой grep по 03-Insights/02-Decisions его не видел; эта заметка короче, источник правды = синтез codex.
