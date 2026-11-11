---
dr_id: DR26-11-03-HUB-01-0950
title: "ДР 0950: карта рынка второго мозга и общей памяти, сравнение с Zep AI"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# ДР 0950: рынок второго мозга и общей памяти vs Zep AI

Заказ Антона из чата 001 (сессия-мать H21133). Потребитель: собеседование Антона с CEO Zep AI 04.11.2026 21:00 Лиссабон (роль Head of Forward Deployed Engineering) и решение, где строить наш продукт (Obsidian + SQLite graph RAG + 8 машин + Claude/Codex/Grok).

Ноги (оригиналы дословно в `_originals/deep-research/DR26-11-03-HUB-01-0950-second-brain-market-vs-zep-<нога>.md`, все прошли `dr_leg_gate --strict --prompt`):

| Нога | Режим | Источников | Чат |
|---|---|---|---|
| claude.ai | Research, Fable 5.1 Extra | 121 | https://приватный чат |
| ChatGPT | Deep research, Extra High (a2@) | 228 | https://приватный чат |
| Grok | Expert + skill deep-research (Heavy недоступен по тарифу) | 105 | https://приватный чат |
| GLM | GLM-5.3-Flash (GLM-5.3 отбит «peak hours») | 231 | https://приватный чат |
| Mistral | Vibe Work, deep-research, Think | 649 (ссылки со страницы) | https://chat.mistral.ai/work/25f164aa-99d8-4c00-a88a-a9f7282bc009 |
| Gemini | a@ Pro High, Deep research | 184 | https://приватный чат |

Оговорка по качеству: в ногах claude.ai, ChatGPT и Grok ссылки стоят списком в конце, а не у каждой фразы (гейт: «в тексте 0-1 из N»). Привязка цитат к утверждениям не проверена; GLM шёл на Flash. Несущие факты ниже сверены между ногами, а часть из них мной напрямую на страницах Zep (curl 03.11.2026): помечено «✅ первоисточник».

## Где ноги сошлись (сильный сигнал)

1. **Простая «долговременная память агента» коммодитизируется.** Mem0, Hindsight, Supermemory, Cognee, AWS AgentCore, Vertex Memory Bank, Cloudflare Agent Memory, встроенная память OpenAI/Anthropic/Google/Microsoft (все 5 ног).
2. **Отличие Zep одно и общее для всех ног: битемпоральный граф** (valid_at / invalid_at, противоречия помечаются, а не стираются) плюс provenance, BYOK/BYOC, SOC 2 Type II и HIPAA BAA. ✅ первоисточник: на getzep.com/pricing есть «SOC 2 Type II» и «HIPAA BAA», на главной «Context Lake of Context Graphs» на собственной графовой СУБД Konig (в демо 1,4 млн активных графов). ChatGPT-нога честно отмечает, что отдельные признаки есть и у Hindsight, Cognee Enterprise, Google Memory Bank: уникальна именно комбинация, а не каждый пункт.
3. **Все бенчмарки self-reported.** LoCoMo/LongMemEval у Zep (94,7%/90,2%), Mem0 (92,5%/94,4%), Hindsight (до 94,6%), MemMachine (91,69%) измерены разными harness и моделями, независимой сверки нет; идёт публичная война Zep и Mem0 (блог Zep «Lies, Damn Lies, & Statistics»: у Zep 75,14% против 68,44% у Mem0 Graph в исправленной конфигурации). Это самое уязвимое место Zep для вопросов на интервью (GLM, claude.ai).
4. **Ниша «общая память нескольких ИИ-вендоров у одного человека» занята OSS и ранними стартапами, зрелого SaaS нет**: OpenMemory MCP (Mem0), basic-memory, mcp-memory-service, Cognee MCP, Supermemory (все 5 ног). Наш стек почти повторяет basic-memory (Markdown + SQLite + MCP).
5. **Нет продукта, где личный слой сотрудника принадлежит сотруднику и уходит с ним при увольнении, и нет федерации личных мозгов между организациями** (все 5 ног). Об увольнении у вендоров описано только удаление (RTBF) или хранение по политике тенанта.
6. **Корпоративный сегмент строит граф компании, а не поиск по документам:** Glean, Microsoft Work IQ, Atlassian Teamwork Graph; все трое в 2026 открыли графы агентам через MCP/API.
7. **Устройства-«второй мозг» консолидировались в бигтех:** Limitless куплен Meta (дек. 2025, Rewind закрыт), Bee куплена Amazon, независимо остался Plaud (все 5 ног).

## Где ноги разошлись (и что я решил)

| Вопрос | Позиции | Вердикт |
|---|---|---|
| «Zep Emerging $13k/год» из нашего брифа | Grok: «ранее ~$13k»; claude.ai: тарифа нет; Mistral: программа есть, суммы нет; GLM: цен на странице нет | ✅ первоисточник: на странице есть «Zep Enterprise at an emerging company price», суммы нет. **Цифру $13k не использовать.** Flex $125 и Flex Plus $375 в месяц подтверждены (ChatGPT ошибочно писал, что страница недоступна). |
| ARR Glean | Grok «~$200M+»; claude.ai и Mistral $300M (пресс-релиз Glean, май 2026); GLM не проверил | **$300M ARR** (два независимых пути к одному пресс-релизу). Оценка $7,2B относится к раунду F, июнь 2025. |
| Cognee деньги | Grok и GLM: $1,5M; ChatGPT, claude.ai, Mistral: seed $7,5M, 19.02.2026 | **Seed 7,5M** (eu-startups). Валюта спорна: eu-startups пишет €, ссылка на сайте Cognee зашита как «dollars». Писать «7,5 млн» и оговорку. |
| Есть ли уже продукт «два слоя одного графа: личный + компания» | claude.ai: да, Zep Memory MCP (19.08.2026), Glean, Work IQ; Grok, GLM, Mistral, ChatGPT: «Glean ближе всех, полноценного нет» | Развилка по определению. **Два слоя в одном хранилище есть** (✅ первоисточник Zep: блог «The Memory MCP Server gives each user one agent memory across all of them, governed by policy», help.getzep.com «Graph Directory», блог ABAC от 09.07.2026; дату GA 19.08 подтвердила только нога claude.ai). **Нет** владения сотрудником и выноса при увольнении. Все ноги согласны именно в этом втором. |
| Звёзды | Hindsight ~20k (Grok) против ~58k (ChatGPT); Graphiti 30-32k у всех; Mem0 62-68k | Не использовать звёзды как довод без свежей сверки на GitHub. |
| Деньги Supermemory | claude.ai ~$3M; Mistral $29M | **Не решено**, взять «от $3M до $29M, сверить». |
| Поглощения: Moveworks→ServiceNow $2,85B, Sana→Workday $1,1B, Onyx→Zoom (сент. 2026) | Только в ноге Mistral со ссылками на TechCrunch и пресс-релиз Workday | Правдоподобно, независимо второй ногой не подтверждено; на интервью называть осторожно. |

## Поправка 03.11.2026: Gemini-нога пришла (6/6)

- Подтверждает пункты 1-5 из «сошлись»: коммодитизация простой памяти, отсутствие продукта с двумя слоями и выносом при увольнении («при увольнении учётная запись деактивируется, экспорта нет»), Glean >$300M ARR, ниша мультивендорной памяти занята OSS.
- **Повторила устаревшее из нашего брифа без проверки**: «Emerging $13k/год» и «Cognee $1,5M». Против неё три ноги и страницы Zep, поэтому её вес по этим двум фактам нулевой (пример слабого места ног, которые принимают заказ за истину).
- Новая деталь, подтверждена только этой ногой: исходный локальный OpenMemory MCP объявлен устаревшим и переведён в архив, логика влита в единый self-hosted сервер Mem0. Перед тем как называть это на интервью, сверить на GitHub mem0ai.
- Также только у неё: Graphiti «24k+ звёзд» (у остальных 30-32k): звёзды как довод не брать.

## Три вывода, которые меняют действия

1. **Для собеседования в Zep.** Zep из «памяти чат-бота» стал «Context Lake»: Memory MCP Server с одним графом пользователя для Claude, ChatGPT, Cursor, общие графы компании через ABAC (default-deny), v4 Context API (14.10.2026), BYOC. Для роли Head of FDE главная история: внедрение это не «подключить SDK», а онтология (типы сущностей и рёбер), разбиение на пользовательские и общие графы, ABAC и SSO, оценка на данных клиента, а не на LoCoMo. Вопросы CEO: (а) чем объясняется скачок LoCoMo с 75% до 94,7% после спора с Mem0; (б) деньги: публично раскрыто ~$2,3M против $24M у Mem0, раунда 2026 года не найдено; (в) что будет, когда AgentCore/Vertex/Cloudflare съедят нижний сегмент. Идея в подарок: экспорт личного графа сотрудника при увольнении по политике работодателя и федерация между проектами: по всем ногам ниша пуста, а у Zep разделение user graph / shared graph уже есть.
2. **Для нашего продукта.** Не позиционироваться как «одна память на все ИИ-инструменты»: ниша занята. Уникальность строить на трёх вещах, которых в найденных продуктах нет вместе: битемпоральные факты в локальном SQLite, provenance «какой агент, вендор и машина записали факт» с разрешением конфликтов между Claude, Codex и Grok, синхронизация 8 машин без облака. Graphiti (Apache-2.0) брать как эталон схемы, а не изобретать темпоральную модель. Эксперимент на неделю: сравнить Graphiti self-host и наш SQLite-граф на темпоральной инвалидации и многоагентной записи.
3. **Корректировка наших старых цифр** (брифа 02.10): убрать «Emerging $13k»; Mem0 это 62-68k звёзд, а не 41k; Cognee $7,5M, не $1,5M; Glean $300M ARR, Zep Community Edition не поддерживается (open source только Graphiti), Letta сместила код в letta-code.

## Чего в данных нет
Независимых головы-в-голову бенчмарков; выручки Zep и числа клиентов; цен Enterprise у Glean/Writer/Hebbia/Zep; ниша «второй мозг семьи» специально не искалась (слабая улика пустоты); регуляторика (GDPR, право на перенос личной памяти) не исследована.

## Что дальше (внедрение)
- Секция «Конкуренты Zep» уже в interview-prep-2026-11-04-zep-ai (дописана по первой ноге, после синтеза обновить таблицей расхождений выше).
- Gemini-нога пришла, поправка выше. Все 6 ног прошли гейт.
- Подкастный/контентный выход (§9.1 пост через 2-3 дня): `dr_post_draft.py --id DR26-11-03-HUB-01-0950`.

Machine: хаб (Хаб) · Agent: Claude · Operator: Anton · 2026-11-03


## Check it yourself: prompt, sources, raw reports

- Prompt: [DR26-11-03-HUB-01-0950-dr-0950-karta-rynka-vtorogo-mozga-i-obschey-pamyati-sravnenie-s-zep-ai/prompt.md](DR26-11-03-HUB-01-0950-dr-0950-karta-rynka-vtorogo-mozga-i-obschey-pamyati-sravnenie-s-zep-ai/prompt.md)
- All sources (1085 links): [DR26-11-03-HUB-01-0950-dr-0950-karta-rynka-vtorogo-mozga-i-obschey-pamyati-sravnenie-s-zep-ai/sources.md](DR26-11-03-HUB-01-0950-dr-0950-karta-rynka-vtorogo-mozga-i-obschey-pamyati-sravnenie-s-zep-ai/sources.md)

## Raw model reports

6 reports delivered, synthesis used 5 of 6. Note: 1 report(s) arrived after the synthesis was written; they are attached raw and were not reconciled. [DR26-11-03-HUB-01-0950-dr-0950-karta-rynka-vtorogo-mozga-i-obschey-pamyati-sravnenie-s-zep-ai/](DR26-11-03-HUB-01-0950-dr-0950-karta-rynka-vtorogo-mozga-i-obschey-pamyati-sravnenie-s-zep-ai/README.md)
