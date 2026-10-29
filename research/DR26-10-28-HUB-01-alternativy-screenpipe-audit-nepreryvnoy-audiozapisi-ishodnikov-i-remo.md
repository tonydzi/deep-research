---
dr_id: DR26-10-28-HUB-01
title: "Альтернативы Screenpipe: аудит непрерывной аудиозаписи, исходников и ремонтопригодности"
date: 2026-10-28
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-10-28-HUB-01): Альтернативы Screenpipe: аудит непрерывной аудиозаписи, исходников и ремонтопригодности

> Есть ли open-source замена Screenpipe для 24/7 audio-only записи (mic + system) на macOS Intel/ARM и Windows, и стоит ли чинить или заменять Screenpipe.

## Ключевые выводы
- Полного готового OSI-open-source аналога Screenpipe, у которого публично доказана вся комбинация (macOS Intel+ARM, Windows, 24/7 unattended, mic+system loopback, audio-only, долговечные локальные файлы, независимость от STT/LLM, сохранение тишины и non-speech, recovery после sleep/reboot/device switch), не найдено; не хватает именно доказательства эксплуатационной части «72 часа без человека».
- Screenpipe больше не OSI-open-source: на pinned commit d8589e6 (App v2.7.125, 26.10.2026) LICENSE.md это Screenpipe Commercial License, README называет продукт source-available. Разрешены personal non-commercial, research/education/nonprofit и 7-дневная оценка организацией; коммерческая работа с исходниками требует коммерческой лицензии; ранее выпущенные MIT-версии остаются MIT. Commercial License присутствует уже на v2.7.79 (29.09.2026); последнюю MIT-ревизию установить не удалось.
- Лучший кандидат: MeetRec (mmaximov97/meeting-recorder): MIT, сборки для Windows, macOS Intel и Apple Silicon, отдельные mic.wav и system.wav, запись работает без transcription server, VAD относится только к опциональной STT. Минус: ориентирован на сессии/звонки, 24/7, ротация и auto-resume после reboot не доказаны.
- Второй кандидат: ActaVoces (AGPL-3.0-or-later): Windows x64 и обе архитектуры Mac, mic+system, WAV остаются обычными файлами, summaries опциональны, faster-whisper локально. Минус: сам проект pre-1.0, неполный multi-platform QA, unattended-режим не документирован. OBS Studio годится как контроль зрелости capture-стека, но не доказан как audio-only 24/7 daemon.
- Остальные не проходят строгие требования: Meetily (VAD, noise removal, RMS mixing могут уничтожать non-speech; Intel Mac не доказан), Sanka Recorder (запись только по кнопке Record, только Japanese/English), Vexa Desktop (MVP, сегментация по паузам, зависимость от transcription endpoint), Amanu и Hark (привязка к встречам), Audacity (на Mac нужен отдельный routing). OpenRecall исключён (screenshots), Hearsay только Windows, akou macOS-ориентирован, sonus-auris только мобильный.
- Свежие документированные проблемы Screenpipe: #7059 (Windows, WAL ~40k pending pages, операции БД до ~16.8 с, лаги при выключенном audio), #7704 (macOS, quick_check на 77 GB базе 24–60 мин, открыт, коммит 3f77658), #7700 (Windows, рост handles в audiodg.exe из-за device inventory через CPAL WASAPI, открыт), #7679 (capture встал, а /health остаётся OK), #7626 (старые процессы держат порты 3030/11435), #7693 (HTTP 403 для pipes). Prevalence оценить нельзя: нет знаменателя.
- Старые баги нельзя автоматически переносить на текущий билд: #1626 (остановка audio после ночи/sleep) закрыт в феврале 2026, v2.7.125 включает исправление native AEC microphone recovery. Нагрузка не сводится к локальной STT/LLM: в #7059 audio был выключен, #7700 относится к WASAPI-enumeration.
- Ремонтопригодность (аналитическая шкала автора отчёта): MeetRec 8.2/10, ActaVoces 7.7, OBS 7.5, Sanka 7.0, Meetily 6.4, Vexa 5.9, Screenpipe 5.4 для независимого коммерческого сопровождения. Исходники Screenpipe модульны (crates/screenpipe-audio, screenpipe-db, Tauri-приложение), но привязка к SQLite, ML, UI и API повышает стоимость самостоятельного ремонта; сборка из исходников в аудите не выполнялась.

## Рекомендации / решения
- Не объявлять ни один найденный проект production replacement сейчас; Screenpipe держать как incumbent/reference.
- Провести 72-часовой A/B canary на изолированных Mac и Windows хостах: первым MeetRec, вторым ActaVoces, OBS как контроль capture-стека, Screenpipe для сравнения.
- Приёмку строить на декодируемом аудио: известный stimulus-план (EN/RU фразы, тоны каждые 5 мин, музыка, ≥30 мин тишины), vision_artifacts=0, ≥99.9% маркеров, нет необъяснённых разрывов >5 с, неизменность записи при выключенных STT/LLM, 0 действий человека после reboot для строгого 24/7.
- Тестировать отказы: sleep/wake, USB/Bluetooth переключение, смена sample rate, kill, reboot, недоступность STT-эндпоинта на 4–6 часов; для Screenpipe сравнивать /health с timestamp последнего реально воспроизводимого аудио-чанка и следить за размером WAL и handles audiodg.exe.
- Для Meetily сверять сохранённый вывод с эталоном на музыке, ambient и тишине; для MeetRec подтвердить на длинной записи, что VAD влияет только на STT, а не на WAV.
- Для корпоративного использования Screenpipe провести отдельный лицензионный review; не строить закупку на допущении, какая версия была последней MIT.
- Если требование ослабить до «одна вручную запущенная длинная запись», MeetRec становится сильнейшей найденной open-source заменой; при обязательном авто-старте и самовосстановлении ни один кандидат пока не подтверждён.

## Сущности
- **Люди:** —
- **Компании:** Screenpipe, Zackriya-Solutions, Vexa-ai, Sanka, EzyGang, OBS Project, Audacity Team
- **Продукты/инструменты:** Screenpipe, MeetRec (mmaximov97/meeting-recorder), ActaVoces, OBS Studio, Audacity, Meetily, Sanka Recorder, Vexa Desktop, Amanu, Hark, OpenRecall, Hearsay, akou, sonus-auris Audio Dashcam, Whisper, faster-whisper, Ollama, SQLite, Tauri, CPAL, WASAPI, ScreenCaptureKit, Silero VAD, Deepgram

## Открытые вопросы
- Выдержат ли MeetRec и ActaVoces 72 часа unattended-записи: публичных доказательств нет.
- Какая именно ревизия Screenpipe была последней под MIT.
- Воспроизводима ли чистая сборка Screenpipe из исходников на Windows/macOS (подпись, нотаризация, хэши model assets, соответствие официальному бинарнику): не проверялось.
- Реальные CPU/RAM/disk-данные для альтернатив: честных кроссплатформенных бенчмарков нет.
- Не идентифицирован публичный проект «Amure»; отдельно рассмотрен Amanu.
- Частота сбоев Screenpipe у пользователей неизвестна: issues не дают prevalence.
- Закреплены ли в релизе исправления #7704, #7679, #7693 и работает ли recovery после sleep на Windows.

## Источник
- DR-ID `DR26-10-28-HUB-01` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- Screenpipe
- непрерывная аудиозапись 24/7
- source-available vs open source
- ремонтопригодность ПО
- canary-тестирование
- system loopback audio
- VAD и сохранение non-speech
- WAL/SQLite нагрузка

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-10-29 через gh: 7 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 7 · 🤷 0.
