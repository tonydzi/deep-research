---
dr_id: DR26-07-04-HUB-06
title: "X API v2 posting + FB Graph reading own posts — July 2026 setup, automatable vs owner-only"
date: 2026-07-04
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-04-HUB-06): X API v2 posting + FB Graph reading own posts — July 2026 setup, automatable vs owner-only, payment gating

> Deep-research запрос про точные шаги июля 2026 для X API v2 (постинг твитов) и FB Graph API (чтение своих постов), с классификацией шагов на agent-can-do vs owner-only и вопросом о платёжном гейте.

## Ключевые выводы
- В отчёте зафиксирован только сам запрос на Deep Research (промпт), фактические результаты исследования (ответы ChatGPT Deep Research) в предоставленном тексте отсутствуют.
- Запрос требует ordered checklist по X API v2: создание dev-аккаунта на developer.x.com, требуется ли способ оплаты ДО создания app / генерации OAuth 1.0a credentials / только перед первым POST, генерация 4 OAuth 1.0a credentials с read+write, текущий pay-per-use прайсинг POST /2/tweets со ссылкой, наличие бесплатного месячного лимита записи.
- Запрос требует ordered checklist по Facebook Graph API: жив ли в июле 2026 GET /me/posts для личного профиля или это deprecated (Pages-only), точные шаги создания Meta app + User access token с user_posts (или текущий эквивалент) в dev mode, нужен ли App Review для owner, читающего свои посты в dev-mode, процедура long-lived token (обмен, срок жизни, refresh).
- Целевой юзкейс: соло-фаундер хочет ассистента, который (1) авто-постит один твит/день с тизером+ссылкой в X, (2) READ-ONLY читает собственные посты в Facebook для детекта постов без тизера; бюджет-чувствительно, ищет самый дешёвый легальный путь.
- Итоговый формат ответа должен был содержать: два чеклиста (A, B) с тегами [AGENT-CAN-DO]/[OWNER-ONLY] на каждый шаг, финальный вердикт «% автоматизации браузерным агентом без владельца» + единственный owner-only блокер на платформу, и заметку «что изменилось в 2025-2026» по каждой платформе.

## Рекомендации / решения
- Для получения реальных ответов нужно дождаться/забрать фактический отчёт Deep Research из ChatGPT (в текущем материале сохранён только исходный промпт, без синтеза) — деп-рекёрч не завершён/не приложен.
- При повторном запуске зафиксировать явные источники (developer.x.com, Meta for Developers docs) и цитаты, так как именно это требовалось ROLE-инструкцией.

## Сущности
- **Люди:** Anton
- **Компании:** X (Twitter), Meta, OpenAI
- **Продукты/инструменты:** X API v2, Facebook Graph API, developer.x.com, OAuth 1.0a, GET /me/posts, POST /2/tweets, gpt-5-5-pro, gpt-5-5-instant, Deep Research App

## Открытые вопросы
- Требуется ли платёжный метод перед созданием X app / генерацией токенов / только перед первым POST — не отвечено в приложенном тексте.
- Текущий pay-per-use прайсинг POST /2/tweets и наличие бесплатного месячного лимита — не отвечено.
- Работает ли GET /me/posts для личного профиля в июле 2026 или полностью deprecated в пользу Pages — не отвечено.
- Нужен ли App Review для owner, читающего собственные посты в dev-mode — не отвечено.
- Итоговый % автоматизации агентом и единственный owner-only блокер по каждой платформе — не получен (отчёт Deep Research отсутствует в материале).

## Источник
- DR-ID `DR26-07-04-HUB-06` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- chrome-autonomy-self-drive
- social-auth-autonomous
- fb-post
- telegram-account-identities
- credential-store
