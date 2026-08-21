---
dr_id: DR26-07-07-HUB-09
title: "Мозг как сервис: RAG-поиск хаба через HTTP/MCP-API для флота Claude-агентов"
date: 2026-07-07
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-07-HUB-09): Мозг как сервис: RAG-поиск хаба через HTTP/MCP-API для флота Claude-агентов

> Как лучше отдать RAG-поиск (e5+reranker) хаба остальным машинам флота — REST, готовый self-hosted RAG, файлы/Syncthing или MCP-сервер — и как это развернуть под Windows.

## Ключевые выводы
- Сравнены 4 варианта: (a) FastAPI/Flask-обёртка над готовым пайплайном, (b) готовые self-hosted RAG (AnythingLLM, Haystack, RAGFlow, Dify, Open WebUI), (c) файлы/Syncthing-очередь, (d) MCP-сервер (Streamable HTTP)
- Лучший выбор — гибрид (a)+(d): один сервис FastAPI + FastMCP — REST для скриптов/отладки, MCP для LLM-агентов, реализуется поверх e5+reranker за минуты
- MCP в 2026: перешёл под Linux Foundation Agentic AI Foundation, есть Streamable HTTP transport, FastMCP в официальном Python SDK, поддержка в OpenAI Agents SDK и LangChain — MCP даёт нативный tool-calling для агентов вместо ручного HTTP+JSON
- (b) готовые RAG-платформы требуют re-ingest/переиндексацию и могут дать другую fidelity, чем тюнингованный e5+reranker пайплайн — не подходит без потери качества
- (c) файлы/Syncthing — самый простой и отказоустойчивый вариант (нет портов), но проигрывает в латентности (секунды vs 10-100мс) и не нативен для LLM-агентов
- Безопасность: bind сервиса только на Tailscale-IP хаба (не 0.0.0.0), API-key/mTLS, Tailscale ACL (tag:brain-hub → allow только от AI-узлов), firewall inbound только с Tailscale-интерфейса
- Устойчивость под Windows: NSSM (Non-Sucking Service Manager) вместо Task Scheduler — Recovery Restart×3, Startup Automatic Delayed Start (даёт GPU подняться) + отдельный watchdog-скрипт с polling /health каждые 30-60с
- Fallback при офлайн-хабе реализуется на стороне клиента: try hub → except → local_lexical_search (ripgrep по Syncthing-копии волта), с timeout и retry+backoff

## Рекомендации / решения
- Развернуть единый сервис FastAPI+FastMCP поверх существующей функции semantic_search(), не переходить на готовые self-hosted RAG-платформы (избежать переиндексации и потери fidelity тюнинга)
- Bind сервиса строго на Tailscale-IP хаба, закрыть порт 8000 firewall'ом кроме Tailscale-интерфейса, добавить API-key и Tailscale ACL по тегу tag:brain-hub
- Поставить сервис через NSSM с Recovery-политикой и Delayed Start, добавить отдельный watchdog-скрипт с polling /health
- Дать каждому клиенту (агенту на другой машине) fallback на локальный ripgrep-поиск по Syncthing-копии волта на случай недоступности хаба
- Внедрять поэтапно за ~9-13 дней: аудит функции → сервис → сеть/безопасность → тест с узлов → NSSM+watchdog → MCP-интеграция в агентов → fallback/resilience → документация в Obsidian → мониторинг

## Сущности
- **Люди:** —
- **Компании:** Linux Foundation, OpenAI, Anthropic
- **Продукты/инструменты:** MCP (Model Context Protocol), FastMCP, FastAPI, NSSM, Tailscale, Syncthing, AnythingLLM, Haystack, RAGFlow, Dify, Open WebUI, OpenAI Agents SDK, LangChain, ripgrep, Claude Code, e5 (embeddings)

## Открытые вопросы
- Точная latency и стабильность MCP Streamable HTTP transport под реальной нагрузкой флота не протестированы, только оценены
- Не проверено поведение при частых обрывах Tailscale (насколько быстро и надёжно клиенты переключаются на fallback)
- Нужно решить, добавлять ли OAuth 2.1 по MCP-спеке или достаточно API-key + Tailscale ACL

## Источник
- DR-ID `DR26-07-07-HUB-09` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- vault-data-architecture
- second-brain-northstar
- machine-bus-telegram-rail
- own-fleet-peer-equality
- vps-anchor-node
- desktop-max-laptop-min
- insight-DR-DR26-07-07-HUB-08-brain-as-a-service-exposing-the-hub-s-rag-vault-se — сиблинг из того же батча DR (MACANTON-03, русская версия), тот же вопрос
- insight-DR-DR26-07-14-HUB-05-brain-as-a-service-exposing-the-vault-rag-index-to — почти дословный дубль-тема (RAG через HTTP/MCP для флота), более ранняя версия того же DR-вопроса
