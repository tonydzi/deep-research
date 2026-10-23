---
dr_id: DR26-07-16-HUB-03
title: "DR26-07-16-HUB-03 — Synthesis: safe fleet skill autonomy (supply-chain rails)"
date: 
lang: ru
source: Palo Alto AI Research Lab — deep research programme
---

# Синтез DR26-07-16-HUB-03 — безопасная автономия скиллов флота
> ⚠️ Alias: наружу этот DR ушёл как `DR26-07-14-MACANTON-01` (коллизия: тот же номер вручную получил фандрейз-DR). Перенумерован 16.07, реестр `_DR-Registry.md`.

**Метод:** Grok (Expert, 28 источников) + Gemini 3.1 Pro + ChatGPT (deep research, самый дотошный, забран Антоном вручную — MCP-браузер не пробил chatgpt.com). ТРИ независимых вендора → confidence very-high.

## Вердикт по нашему дизайну
✅ **Наш дизайн подтверждён как соответствующий best-practice** — local-* `.stignore` namespace (blast-radius=0 для чужих) + ОДИН писатель на always-on Маячке + промоушен «/tt + leak-scan + 1 peer», Anton-гейт на high-risk. Оба вендора пришли к той же архитектуре независимо. Наш инстинкт AK-47 (не строить тяжёлое) тоже подтверждён — см. «не заморачиваться».

## 6 рельсов, которых у нас НЕ было — ДОБАВИТЬ (оба вендора сошлись)
Ранжир impact×низкая-цена:
1. **Crash-on-collision в загрузчике** (оба = #1, high/low). Если существуют `local-X` И `shared-X` одновременно → агент ЖЁСТКО падает, отказывается грузить оба, пока человек не разрулит. Закрывает «Shadowing Trap»: локальная версия молча остаётся уязвимой, когда общая пропатчила дыру. → нужен guard в конвенции имён.
2. **Capability-манифест во frontmatter SKILL.md** + **shell ВЫКЛ по умолчанию**. `permissions: [shell, network, filesystem]` / `risk_level` / `processes_untrusted_data`, загрузчик энфорсит. ⭐ У Claude Code это РЕАЛЬНО есть: `disableSkillShellExecution` + `skillOverrides` (мы их не трогали). Скилл с shell → обязательный review.
3. **Sync-conflict blacklist в ЗАГРУЗЧИКЕ** (не только `.stignore`). Агент обязан отказываться грузить `*.sync-conflict-*` (иначе читает склеенные битые инструкции). 🔴 ЖИВОЙ РИСК: у нас уже лежит `skills/dr-fanout/SKILL.sync-conflict-20260714-*.md` — проверить, что загрузчик его игнорит.
4. **Sandbox для smoke-test/`/tt`** нового скилла — БЕЗ живого shell (restricted/mock), чтобы деструктивный скилл не выстрелил «пока тестирует сам себя».
5. **Data/action isolation (privilege separation)** — скилл, читающий внешние недоверенные данные, НЕ в одной сессии со скиллом с outbound/shell/high-risk. Защита от prompt-injection (скилл прочитал вебстраницу с «exfiltrate keys»).
6. **Leak-scan = ЖЁСТКИЙ блокирующий гейт** (TruffleHog/gitleaks), а не «глянули». Секрет в скилле = стоп промоушена.

## Апгрейд механизма промоушена — VPS-anchored GitOps (Gemini, чище нашего)
Вместо «пир прислал tar + 1 пир глянул глазами»: пир пушит `local-<name>/` в ветку Git-репо на Маячке → CI/n8n гоняет гейты (leak-scan + sandbox smoke + AI-peer-review; high-risk → webhook человеку) → merge в main → **ТОЛЬКО Маячок** пишет в `shared/` (Syncthing-папка) → read-only P2P на все. Разделяет ГЕЙТ (Git/CI на Маячке) и ДОСТАВКУ (Syncthing = только read-only слой). У нас уже есть GitLab + n8n → почти бесплатно.

## Failover — наш «STAGED, без force-flip» подтверждён; держим ПРОСТО
Оба: **НЕ строить Raft/fencing/кворум над Syncthing** (не тот слой для консенсуса). Хардкодим Маячок единственным писателем; упал → локальная разработка продолжается, промоушен ПАУЗА; ручное назначение временного писателя только при долгом простое. Gemini прямо: «автоматический failover, который мисфайрит, ХУЖЕ ручного». = ровно наш анти-паттерн «arm before green».

## «Не заморачиваться» (оба — валидирует AK-47)
Sigstore/SLSA подпись (Syncthing-транспорт уже mTLS между device-ID) · CRDT · Raft/Paxos кворум-выборы · reproducible builds/SBOM · тяжёлый canary · навороченный CODEOWNERS/2-персон-на-каждое.

## ChatGPT (3-й вендор) — добавил рельсы + ОСПОРИЛ 2 наших выбора
Самый строгий отчёт. Подтвердил всё выше и добавил:
7. **Immutable promotion reviewed-revision** — ревьюят конкретный commit-hash, публикуют ИМЕННО его, hash в манифест (иначе review/publish TOCTOU: ревьюнули A, синкнулось B). Подписанные git-теги.
8. **Risk-класс таксономия**: inert / shell-local / shell-networked / secret-touching / always-loaded-core / irreversible → гейт И rollout зависят от класса.
9. **`disable-model-invocation: true`** (реальный knob Claude Code) для side-effect скиллов = вызов только вручную.
10. **Canary rollout**: anchor → 1 canary → остальные после soak.
11. **GitHub protected-branch дёшево**: CODEOWNERS + dismiss-stale-approvals + «не последний пушер» аппрувит.
12. **Бан symlink + shared→local ссылок** — общий скилл самодостаточен, не тянет локальные пути/env.
13. Failover: epoch = fencing-token, консументы ОТКЛОНЯЮТ бандл со stale-epoch (энфорс сильнее нашего .leader; cites etcd/Consul/ZK/K8s).

### ⚠️ 2 вызова нашему approved-дизайну (решает Антон)
- **A. `local-*` префикс → ChatGPT против.** Плоский префикс легко зашадоуить (shadow)/мис-globнуть/случайно опубликовать; предлагает rooted-subtree `skills/local/<machine-id>/` + `skills/shared/`, ИЛИ держать локальное авторство ВНЕ синкаемого корня совсем (как `CLAUDE.local.md` — отдельный root). ⚠️ Тонкость: Claude Code грузит «внутренний путь лаборатории»<name>/` ПЛОСКО — вложенный `skills/local/foo/` НЕ загрузится тем же glob'ом → rooted-subtree конфликтует с loader'ом; практичный вариант = отдельный локальный root через `--add-dir` вне синка. Наш `.stignore local-*` доказан рабочим, но collision-fatal loader (рельс 1) обязателен.
- **B. Syncthing receive-only НЕ достаточно** (сильнейшая находка). Receive-only НЕ делает ФС immutable — локальные правки сохраняются и влияют на поведение узла, просто не распространяются. 🔴 ЖИВОЕ ДОКАЗАТЕЛЬСТВО: на этом Маке `receiveOnlyChangedFiles=10` прямо сейчас — 10 локально-изменённых файлов общего набора уже потенциально дивергентят узел. Нужен **OS-level read-only (ACL/ro-mount) на shared/ для агент-юзера**, поверх Syncthing. Материализовать из отдельной staging-папки.

## Что это меняет в плане стройки (для флота, через консенсус)
К staged-плану из decision-2026-07-14-fleet-skill-autonomy-local-namespace добавить рельсы 1-6 + рассмотреть GitOps-промоушен как механизм шага 4. Приоритет внедрения: рельсы 1 (collision), 3 (sync-conflict), 2 (shell-off/capability) — high-value/low-effort, первыми.

## Связано
- beat-2026-07-16-came-to-fix-already-fixed-receiveonly-detective — DR формулирует ту же находку receive-only не достаточно
