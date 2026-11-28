---
dr_id: DR26-11-28-HUB-02-0204
title: "Google Antigravity 2.0 Orchestrator (Carbon/Neon/Argon + Hydrogen Executor), внутреннее меню, 5 примитивов оркестрации и маршрутизация флота"
date: 2026-11-28
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-11-28-HUB-02-0204): Google Antigravity 2.0 Orchestrator — устройство, тесты и правила для нашего флота

> Как устроен Оркестратор в Google Antigravity 2.0 (`Orchestrator (Carbon / Neon / Argon)`), что находится внутри его подменю настроек, как реально работают 5 примитивов оркестрации (`Orchestrator`, `invoke_subagent`, `/swarm`, `/run-workflow`, `/boost`) и как правильно настроить их для нашего флота из 6 машин и папки `E:\!CLAUDE-PaloAltoPC-June26`.

Связи: alpha-protocol-recall-plus-dr · dr-max-thinking-and-wide-fanout · mission-get-noticed-hired-by-llm-company · retro-2026-11-28-antigravity-orchestrator-deep-research

---

## 1. Вердикт одной строкой (Decision Memo)

**Оркестратор Antigravity 2.0 (`gemini-pro-orchestrator*`) — это двусторонняя связка тяжелого Планировщика (`Gemini 4 Carbon / Argon / Neon` или `Claude Opus 5.5`) и скоростного Исполнителя (`Gemini 4 Hydrogen` ~165–170 tok/s или `Claude Sonnet 5.5`) внутри одного окна контекста с 91% попаданием в кэш промпта и экономией ~24% квоты.** В нашем стеке:
1. **Интерактивный кофаундер Майкрофт (лимит 110 слов):** `Gemini 4 Argon (High)` или `Orchestrator (Argon)` — лучшая дисциплина формата; `Orchestrator (Carbon)` — для глубоких архитектурных рефакторингов на 3–8 файлов.
2. **Фоновый CLI (`antigravity.py` / `llm_ask.py`):** починен критический дефект `best_model()`, который жадно выбирал `gemini-4-neon-xhigh` (17/26 обрывов `unparsable` за 21 минуту и латентность 125–168 с); теперь `best_model()` ранжирует `argon > carbon/pro > orchestrator > hydrogen > neon` и `high > medium > low > xhigh`, выбирая **`gemini-4-argon-high`** (`_test_antigravity.py` 14/14 зелёный).
3. **Рой (`/swarm`) в `E:\!CLAUDE-PaloAltoPC-June26` (5.9 ГБ без `.git`):** запускать строго в режиме `workspace: "inherit"` с непересекающимся делением файлов в `brief`, обязательным `workspace_write_lease.py acquire` и 1 независимым `verifier` на 2–4 авторов; при падении общего пула Gemini (<20%) переключать ждущих воркеров через `swarm{action:"model", model:"claude-sonnet-5-5-medium"}` в отдельный 100%-й пул Claude внутри Antigravity.
4. **Python Workflows (`/run-workflow`) на Windows-хабе:** починен корень зависания `run_workflow` — создан нативный жёсткий линк «внутренний путь лаборатории» рядом с `python.exe` и `python312.dll` (вместо батника `python3.CMD`).

---

## 2. Анатомия Оркестратора из бинарников `agy.exe` (v1.5.18) и `language_server.exe`

### 2.1. Матрица ролей `orchestrator-model-roles`
Извлечённый из бинарника `agy.exe` встроенный JSON-конфиг `"orchestrator-model-roles"` жёстко делит модели на два пула:
- **Допустимые Планировщики (`orchestrators`):** `gemini-4-neon`, `gemini-4-carbon`, `gemini-4-argon`, `claude-opus-5-5`.
- **Допустимые Исполнители (`executors`):** `gemini-4-hydrogen`, `claude-sonnet-5-5`.

В меню выбора моделей Antigravity выставлены три готовых пресета:
- `gemini-pro-orchestrator` -> **`Orchestrator (Carbon)`** (`Planner: Gemini 4 Carbon` + `Executor: Gemini 4 Hydrogen`)
- `gemini-pro-orchestrator-neon` -> **`Orchestrator (Neon)`** (`Planner: Gemini 4 Neon` + `Executor: Gemini 4 Hydrogen`)
- `gemini-pro-orchestrator-argon` -> **`Orchestrator (Argon)`** (`Planner: Gemini 4 Argon` + `Executor: Gemini 4 Hydrogen`)

### 2.2. Что находится внутри подменю Оркестратора («менюшка внутри»)
В GUI (`Settings -> Models -> Orchestrator Settings` или выпадающее подменю над полем ввода чата) и в CLI (`/model` с навигацией `Tab` / `Shift+Tab` и стрелками `←` / `→`) настраиваются 4 параметра, сериализуемые в `~/.gemini/config/config.json` (`jetbox_state_pb.UserSettings`):
1. **Модель Оркестратора (`userSettings.orchestratorModel`, protobuf-поле 63):** на хабе сейчас `MODEL_PLACEHOLDER_M427` (профиль `M417`).
2. **Модель Исполнителя (`userSettings.orchestratorExecutorModel`, protobuf-поле 64 / `plan-execute-config.executionModel`):** по умолчанию `MODEL_PLACEHOLDER_M669` -> `M432` (`Gemini 4 Hydrogen`).
3. **Уровень размышлений (`Thinking Level / Effort: low | medium | high | xhigh | max`):** отдельно для планирования и исполнения.
4. **Политики ревью и автоисполнения (`artifactReviewMode` и `autoExecutionPolicy`):**
   - На хабе в `config.json`: `"artifactReviewMode": "ARTIFACT_REVIEW_MODE_TURBO"`, `"autoExecutionPolicy": "CASCADE_COMMANDS_AUTO_EXECUTION_EAGER"` — Планировщик передаёт план Исполнителю сразу без паузы на кнопку `Proceed`.

### 2.3. Двусторонняя передача хода и Checkpointer
- Планировщик исследует файлы в режиме чтения и строит план (`update_steps`), после чего передаёт управление Исполнителю (`Gemini 4 Hydrogen`, ~165–170 tok/s).
- При падении теста или неожиданном выводе команды управление **возвращается Планировщику** для пересмотра плана в том же контексте.
- Конфиг чекпойнтера (`CASCADE_USE_EXPERIMENT_CHECKPOINTER`): `"strategy": "CHECKPOINT_STRATEGY_SAME_MODEL"`, `"use_last_planner_model": true`, `"token_threshold": "50000"`, `"max_token_limit": "256000"` (`512000` на профиле `M667`), `"max_output_tokens": "32768"`.

---

## 3. Результаты живых испытаний всех примитивов на хабе (`хаб`)

| Тест | Что запустили | Фактический замер (улика) | Вывод |
|---|---|---|---|
| **Тест 1: `/swarm` + `Orchestrator (Carbon)`** | `swarm{action:"add", name:"probe-orch", model:"gemini-pro-orchestrator"}` (сессия `c01b5a12-1127-4b48-90ee-c0993af43175`) | Шаг 3: `in=36614, cache=0, out=841` (`read_board`, `swarm status`). Шаг 6: `in=3339, cache=34565` (**91% cache hit!**), `out=1996`, пост `#2 @coordinator [done] PONG (#1)` и уход в `swarm_wait` (0 токенов в ожидании). Воркер видит 17 тулов (без `invoke_subagent`). | Оркестратор работает внутри воркера роя, держит 91% кэша между шагами и чисто паркуется через `swarm_wait`. |
| **Тест 2: `invoke_subagent` (`self`)** | `invoke_subagent{TypeName:"self", Role:"Orchestrator Subagent Probe"}` (сессия `45b8a03f-28cc-4213-a80c-a04df6590d69`) | Отработал за **25 с** (`01:48:56Z` -> `01:49:21Z`), подтвердил все 21 инструмент (`swarm`, `invoke_subagent`, `run_workflow`, `edit_file`, `run_command`) и доступ к `E:\!CLAUDE-PaloAltoPC-June26`. | Идеально для изолированных проверок и сохранения чистоты главного контекста. |
| **Тест 3: CLI `antigravity.py` + `gemini-pro-orchestrator`** | Headless-вызов `antigravity.py` с `--model gemini-pro-orchestrator` (`antigravity_usage.jsonl:26509`) | `outcome="ok", secs=123.1, in_tokens=11628, out_tokens=84, thinking_tokens=81, turns=1` (сессия `f1d26451-557a-47ba-8543-cf23522a7c39`). | В одиночном 1-turn вызове Оркестратор тратит ~120 с на инициализацию пайплайна, поэтому для 1-turn CLI выгоднее `gemini-4-argon-high` или `gemini-4-neon-medium`, а `gemini-pro-orchestrator` нужен только для многошагового `antigravity.py agent`. |
| **Тест 4: `run_workflow` на Windows** | `run_workflow(Script=..., WaitMsBeforeAsync=5000)` (`task-91`, `task-145`) | Зависал в фоне, так как Go-раннер Antigravity зовёт `python3`, а на хабе `python3` вёл на батник «внутренний путь лаборатории». | Создан нативный `os.link` «внутренний путь лаборатории» -> `python.exe`. |
| **Тест 5: Аудит квот 27 моделей (`swarm models all:true`)** | Живой опрос `swarm{action:"models", all:true}` | Все 23 модели Gemini (`3.8-flash`, `4-argon`, `4-carbon`, `4-hydrogen`, `4-neon`, `pro-orchestrator*`) делят **один общий пул квоты (17%)**, тогда как `claude-opus-5-5-*`, `claude-sonnet-5-5-*` и `gpt-oss-120b-medium` в той же подписке Antigravity имеют **отдельный пул 100%**! | При просадке пула Gemini ниже 20% переключаем воркеров `/swarm` и Исполнителя Оркестратора на `claude-sonnet-5-5-medium`. |

---

## 4. Синтез панели 6 внешних моделей (`orouter_review.py panel`) и правила для нашего стека

Панель из 6 независимых семей (`z-ai/glm-5.3`, `deepseek/deepseek-v4-pro`, `mistralai/mistral-large-2512`, `openai/gpt-5.6-terra`, `google/gemini-3.1-pro-preview`, `meta/muse-wonder`) за 52 секунды проверила наши находки и выделила 5 правил эксплуатации в нашем контексте:

1. **Матрица выбора из 5 механизмов Antigravity:**
   - **Обычная задача в 1 окне (2–8 связанных файлов):** `Orchestrator (Argon)` или `Orchestrator (Carbon)` — двусторонний handoff между Планировщиком и `Hydrogen` + 91% попадания в кэш промпта.
   - **Шумный поиск по коду/докам или браузер (`research` / `browser`):** `invoke_subagent` — изолирует десятки вызовов чтения/кликов от главного окна.
   - **Параллельная разработка 3+ независимых модулей с жёсткой приёмкой:** `/swarm` — 2–4 автора (`gemini-4-hydrogen-high` или `claude-sonnet-5-5-medium`) + 1 состязательный `verifier` (`gemini-4-argon-high` или `claude-opus-5-5-high`) через общую доску `sidecar://swarm/swarm_board`.
   - **Однотипная конвейерная обработка списка файлов со строгой JSON-схемой:** `/run-workflow` (`parallel()`, `pipeline()`, `agent(..., output_schema=...)`).
   - **Один тяжёлый алгоритмический баг, гонка или формальное доказательство:** `/boost` (встроенный пайплайн `critic_orchestrator` + `empirical_challenger` + `lemma_proof_verifier`).
2. **Защита от локальности кэша (находка `glm-5.3` + `muse-wonder`):**
   - 91% кэш-хит (`34 565 cache_read_tokens`) работает только внутри **одной продолжающейся траектории на прибитой модели**. Не дёргать `swarm{action:"model"}` на каждом шаге — менять модель воркера только при реальном исчерпании пула квоты.
3. **Защита от шторма (Stampede) в 100%-й пул Claude внутри Antigravity (находка `deepseek-v4-pro` + `gpt-5.6-terra` + `muse-wonder`):**
   - Поскольку узлы флота под одним Google OAuth делят квоту подписки Antigravity, нельзя переводить массовые ночные фоновые циклы всех 6 машин на `claude-sonnet-5-5` внутри `agy`. Фоновые текстовые рутины идут через лестницу `llm_ask.py` (`grok`, `glm`, `omniroute`, `openrouter`), а 100%-й пул `claude-sonnet-5-5` / `claude-opus-5-5` внутри Antigravity держим как оперативный резерв для интерактивного Оркестратора, авторов `/swarm` и независимых верификаторов.
4. **Безопасность `/swarm` в не-гитовой папке `E:\!CLAUDE-PaloAltoPC-June26` (5.9 ГБ, 6/6 моделей):**
   - В корне `E:\!CLAUDE-PaloAltoPC-June26` нет `.git`, поэтому режимы `workspace: "share"` и `"branch"` не могут опереться на `git worktree`, а `git diff` недоступен. В этой папке воркеры `/swarm` запускаются только с `workspace: "inherit"`, явным списком непересекающихся файлов в `brief` и захватом `workspace_write_lease.py acquire <путь>`. Режим `workspace: "share"` применять только внутри вложенных git-репозиториев (`_oss/*`, `public-repos/*`).

---

## 5. Проверено / Не проверено / Что опровергло бы

- **Проверено:**
  1. Бинарники `agy.exe` (`v1.5.18`) и `language_server.exe` (конфиги `orchestrator-model-roles`, `plan-execute-config`, `UserSettings` поля 63/64, `CHECKPOINT_STRATEGY_SAME_MODEL`).
  2. Живые траектории `Orchestrator (Carbon)` в `/swarm` (`c01b5a12-1127-4b48-90ee-c0993af43175`, 91% cache hit), `invoke_subagent` (`45b8a03f-28cc-4213-a80c-a04df6590d69`, 25 с) и CLI `antigravity.py` (`f1d26451-557a-47ba-8543-cf23522a7c39`).
  3. Лог `antigravity_usage.jsonl` (26 вызовов `gemini-4-neon-xhigh`, 17 падений `unparsable`, 125–168 с) и починка `antigravity.py` + `_test_antigravity.py` (14/14 тестов зелено, `best-model` возвращает `gemini-4-argon-high`).
- **Не проверено:**
  - Точная формула внутреннего скоринга в закрытом серверном бэкенде Google, по которой `Orchestrator (Carbon)` решает на промежуточном шаге вернуть управление с `Hydrogen` на `Carbon` без явной ошибки инструмента.
- **Опровергло бы вывод:**
  - Если в следующем релизе Antigravity `agy models` изменит состав `orchestrator-model-roles` или объединит пулы квот Gemini и Claude внутри подписки в один общий счётчик (проверяется одной командой `swarm{action:"models", all:true}`).

Machine: хаб (Хаб) · Agent: Antigravity · Operator: Anton · 2026-11-28
