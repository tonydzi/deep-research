# DR26-07-16-MACANTON-01-1533 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: grok.com (SuperGrok, Expert mode, 74 sources, thought 2m36s)
dr_id: DR26-07-16-MACANTON-01-1533
topic: "Anthropic OSS contribution map после PR #784"
date: 2026-07-16


chat_url: https://приватный чат
---
# Grok Deep Research verbatim (report section; prompt echo omitted — canonical prompt in _drafts ledger)

Anthropic's public GitHub ecosystem (anthropics org + modelcontextprotocol org) is highly active as of mid-July 2026 and explicitly welcomes community contributions, with mechanisms that reward visible, high-quality work. Your position is strong: a live PR (#784) in claude-cookbooks on multi-agent consensus & verification (distilled from real 24/7 production fleet experience) plus proprietary assets in consensus engines, adversarial verification, human tier-gating, evals/trace tooling, 60+ Claude Code skills, multi-machine sync, and scaled Telegram/WhatsApp MCP usage give you differentiated, production-validated material that matches current demand signals.

Research draws from GitHub org/repo pages, CONTRIBUTING/AI_POLICY/GOVERNANCE files, recent PR/issue activity (2026), official Anthropic/MCP blogs (April-July 2026), the July 2026 Claude for Open Source program expansion, practitioner X posts (June-July 2026), and ecosystem mapping articles.

## 1. CONTRIBUTION SURFACES

### anthropics/claude-cookbooks
- Accepts external PRs: Yes — highly active (~169 open / 299 closed PRs). Multiple external contributors (May-June 2026 PRs: dynamic context, persistent project context, RAG fixes, tool-use, multi-agent orchestration).
- Median review/merge: days-low weeks for focused work.
- Merged vs closed: quality focused notebooks (capabilities, multi-agent, context mgmt, deprecation fixes) merge; duplicate/low-effort/untested close.
- Reviewers: Claude AI Review bot + human maintainers. CI: notebook execution, ruff, structure validation, link checks.
- Quirks: CONTRIBUTING.md (Python 3.11+, uv, pre-commit, conventional commits, one focused change), no CLA.

### anthropics/skills
- Accepts external PRs: Yes (March 2026 CONTRIBUTING.md addition; June 2026 issue про external skill contributions — critical-qa adversarial review skill). Приоритет: bug fixes > docs > improvements > new skills. Для новых скиллов: issue-first + template.
- Активная поддержка (multiagent skill update July 1, 2026). SKILL.md <500 строк идеал, self-contained folders, Principle of Least Surprise.

### anthropics/claude-code
- Yes — сотни PR; external: security fixes, docs/plugin-dev, examples (June 2026). Core строже, docs/examples/plugins открытее. Плагины: official vs community пути.

### SDKs (python/typescript/go) + claude-agent-sdk-python
- Стандартные open-source процессы, bug fixes/features/docs/examples принимаются; активность ниже профилем чем cookbooks/claude-code.

### modelcontextprotocol/* (spec+docs, registry, servers)
- Очень активен (тысячи коммитов, сотни контрибьюторов, частые мержи July 2026). Community-driven через SEPs и Working Groups.
- Reviewers: Core/Lead Maintainers (David Soria Parra, Justin Spahr-Summers) + community maintainers; WG (e.g. Skills Over MCP).
- Quirks: формальный governance (post-Dec 2025), SEPs для significant changes, registry принимает community server submissions.

### Прочее: anthropics/courses (educational), claude-plugins-official/community, claude-code-action. Quickstarts largely archived/moved.

## 2. WHAT GETS PEOPLE NOTICED
- Прямых задокументированных кейсов «hired after one PR» у Anthropic нет [no source found for direct cases]. Механизмы:
- **Claude for Open Source program (expanded July 7, 2026)**: бесплатные 6 мес Claude Max 20x (~$1,200) для мейнтейнеров high-impact репо (5k+ stars), core contributors, ИЛИ 100+ merged PRs в чужие репо за 12 мес (или репо с 20+ external contributors, OpenSSF critical packages, discretionary impact). Apply с GitHub proof.
- Production-validated reusable artifacts: пример Everything Claude Code / ECC (Affaan Mustafa) — hackathon winner, massive stars. Skills/MCP servers с реальным adoption попадают в Anthropic blogs/changelogs/social amplification.
- Сигналы соурсинга: OSS-программа = прямое заявление, что трекают demonstrated builder impact по экосистеме; cross-repo consistency и GitHub profile activity видимы.
- Контрарное: некоторые OSS (Zig и др.) банят AI-код; экосистема Anthropic — противоположная позиция (AI assistance ok с human oversight + disclosure).
- Прямых confer-invites/blog-features за одиночные мелкие PR не задокументировано; impact компаундится через reuse + program eligibility.

## 3. GAPS WORTH FILLING (July 2026)
- Спрос на production-grade multi-agent паттерны: consensus, adversarial verification, human tier-gating, evals/observability/cost-tracing, memory management, context persistence. Cookbooks: свежие PR по persistent project context (#670 тема) и multi-agent orchestration; skills: multiagent guidance updated July 2026.
- MCP servers: базовые категории (fetch, filesystem, git) насыщены. Пробелы: scaled communications (Telegram/WhatsApp), multi-machine sync, evals/trace exposure, secure enterprise patterns. Выделяет: production-validated с реальным масштабом + security/auth/discovery best practices.
- Claimable today: cookbooks multi-agent eval/observability/verification рецепты; skills external contributions (adversarial/critical-qa, consensus/verification skills) issue-first; MCP server в underserved категориях; Skills Over MCP WG / SEPs; claude-code examples/plugins для production fleet.

## 4. MCP ECOSYSTEM STATUS (Mid-2026)
- Governance: **Agentic AI Foundation** (directed fund под Linux Foundation) с декабря 2025 (донейшн Anthropic); vendor-neutral, founding support Anthropic/Block/OpenAI/Google/Microsoft. Иерархия: Contributors → Maintainers → Core → Lead (David Soria Parra, Justin Spahr-Summers). SEPs + Working Groups (Skills Over MCP активна June 2026).
- Registry: community-driven, self-publish путь. Servers repo = reference implementations (steering group).
- Выделяет: решение реальной production-боли на масштабе (auth, discovery, semantics, security), качество + docs/tests, novel non-duplicative категории. Anthropic-инженеры активно вовлечены (core maintenance, WGs, blogs).

## 5. ETIQUETTE & ANTI-PATTERNS + AI DISCLOSURE
- Реджекты: AI-slop (untested/hallucinated/duplicate), scope creep, игнор ревью/CI, self-promotion без ценности, слабые docs/rationale/tests.
- **MCP org: AI_POLICY.md (moved/updated ~June 22, 2026) — ОБЯЗАТЕЛЬНЫЙ disclosure AI-использования в PR/issue** («This PR was written primarily by Claude Code» / «Consulted Claude; solution manually authored»). Human understanding + rationale + evidence (tests) required. Non-disclosure = rude, может вести к закрытию.
- claude-cookbooks: явного запрета нет; Claude AI Review бот сам используется. Дух: human oversight + quality.
- Для нашего кейса (architecture mine, code 80% Claude): дисклозить прозрачно: «Human-designed architecture and validation from production 24/7 multi-agent fleet; implementation assisted by Claude (per project AI guidelines); fully reviewed, tested, battle-tested». Production-опыт = дифференциатор против slop.

## 6. CADENCE STRATEGY
- OSS-программа явно ценит cross-ecosystem PR volume (100+ merged) и consistency на GitHub-профиле.
- Мейнтейнеры/DevRel трекают: activity graphs, cross-repo contributions, PR/issue quality, reuse signals (stars/forks/mentions).
- Рекомендация: cookbooks flagship (момент уже есть) → параллельно 3-5 skills → один standout MCP server.

## TOP-10 (ranked; visibility × merge prob × leverage)
1. Довести #784 + расширить в серию multi-agent consensus/verification рецептов (cookbooks). Effort Low, Merge High, Visibility Very High. FLAGSHIP.
2. Production evals + observability + cost-tracing notebook (cookbooks). Effort Medium, Merge High, Visibility High.
3. 3-5 distilled production skills (consensus engine, adversarial verifier, human tier-gate, multi-machine sync) в anthropics/skills, issue-first. Effort Medium, Merge High, Visibility High. Two-way door (начать с 1-2).
4. Production-grade MCP server: scaled communications (Telegram/WhatsApp fleet-scale) ИЛИ evals/trace exposure → registry. Effort Medium-High, Merge Medium-High, Visibility High. FLAGSHIP (или two-way если скоупнуть).
5. Memory management patterns recipe/skill pack (fleet sync/persistence lessons). Effort Medium, Merge High, Visibility Medium-High.
6. Claude Code examples/plugins: multi-agent fleet orchestration / human-in-loop gating. Effort Low-Medium, Merge Medium-High, Visibility Medium.
7. Мелкие high-signal фиксы + one focused improvement в cookbooks (пока #784 в ревью). Effort Very Low, Merge Very High. Two-way door.
8. Trace tooling / cost observability skill или MCP exposure. Effort Medium, Merge Medium, Visibility Medium-High.
9. Участие в MCP Skills Over MCP WG / SEP discussions с нашими consensus-паттернами. Effort Low, Merge Medium, Visibility Medium (прямой контакт с мейнтейнерами).
10. Cross-surface consistency PRs (ссылки cookbooks↔skills↔MCP README). Effort Low, Merge High. Two-way door.

## Decision
- A) Cookbooks-deep first; B) Balanced flagship across surfaces (anchor cookbooks + skills pack + один MCP server); C) Skills-first volume.
- **Рекомендация: B (balanced)** — сперва #784 до мержа, затем 2-3 skills + скоуп одного standout MCP server. Confidence [emerging]. Main assumption: quality+disclosed+production-validated в активных зонах компаундится быстрее, чем объём в одном репо. Monitor #784 review closely.

(74 sources; полный machine-код источников остался в чате Grok — URL в шапке)
