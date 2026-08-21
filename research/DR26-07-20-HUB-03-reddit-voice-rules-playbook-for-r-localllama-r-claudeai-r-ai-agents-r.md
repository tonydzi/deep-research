---
dr_id: DR26-07-20-HUB-03
title: "Reddit voice/rules playbook for r/LocalLLaMA, r/ClaudeAI, r/AI_Agents, r/MachineLearning,"
date: 2026-07-20
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-20-HUB-03): Reddit voice/rules playbook for r/LocalLLaMA, r/ClaudeAI, r/AI_Agents, r/MachineLearning, r/SideProject

> Researched the unwritten rules, voice, karma gates and winning post patterns across five AI/engineering subreddits to plan a safe, native-sounding launch of a 'fleet of Claude agents + synced knowledge vault' build-log.

## Ключевые выводы
- r/LocalLLaMA: needs ≥10 site karma to comment, ≥5 sub-karma to post; bans undisclosed/primarily LLM-generated text (April 2026 mod rule); self-promo capped at ~1/10 ratio; voice is blunt, hardware/numbers-first (tokens/s, VRAM, quant level), anti-hype toward closed models.
- r/ClaudeAI: self-promotion allowed only if you explain how Claude helped, keep promo minimal, offer free access, avoid referral links; voice is enthusiastic-practical, workflow-focused (CLAUDE.md, Projects, agents), less hardware-obsessed than LocalLLaMA.
- r/AI_Agents: explicit rule limits all-self-promo accounts; best posts are failure-first/lessons-learned with real project counts (e.g. 'I built 30+ automations, most shouldn't have been'); tone is blunt honesty about what didn't work.
- r/MachineLearning: strictest — self-promotion confined to periodic AutoMod '[D] Self-Promotion Thread'; main feed only for [R]/[D]/[P] with substantial/novel/open-source contribution; academic, citation-heavy, low-hype tone; highest risk of a build-log looking promotional.
- r/SideProject: most lenient, self-promotion IS the point, provided it's a live/accessible demo with story, not a bare pitch; lowest karma gate for project posts; personal-story tolerance highest of all five.
- Cross-sub AI-slop detection markers that get posts/comments flagged as bot-written: heavy em-dashes, word 'delve', symmetrical bullet lists, 'It's not X, it's Y' framing, overly polished/formal grammar in casual threads, bolded section headers in comments.
- Winning title patterns differ by sub: LocalLLaMA = 'I built X that does Y on real Z' / benchmark-hardware framing; ClaudeAI = 'Claude Code + X workflow that…'; AI_Agents = 'I built X. Lessons:…'; MachineLearning = '[P]/[D]/[R] Title'; SideProject = 'I built X after Y months'.
- Recommended posting sequence (author's own synthesis, 'emerging confidence'): value-first technical posts on SideProject + AI_Agents + ClaudeAI first, LocalLLaMA later with hardware/open-weight framing, MachineLearning only via the sticky self-promo thread; space posts 7-14+ days apart to avoid looking cross-posted.
- New/young accounts get hit hardest by AutoMod — recommended 2-4 weeks of genuine technical comments before posting to build sub-karma and voice familiarity; instant removal triggers are undisclosed LLM text or pure product/landing-page drops.

## Рекомендации / решения
- Do NOT post the same 'fleet of Claude agents + synced vault' story identically across subs — adapt angle per community: hardware/local-alternative framing for LocalLLaMA, Claude-specific config/CLAUDE.md framing for r/ClaudeAI, architecture+failure-lessons framing for r/AI_Agents, demo+personal-story framing for r/SideProject, and only a rigorous/open-source [P] framing (or the sticky thread) for r/MachineLearning.
- Warm up each subreddit with 2-4 weeks of genuine technical comments before posting, to clear karma gates and avoid AutoMod/shadowban risk.
- Space posts across the 5 subs by at least 7-14 days to avoid the appearance of cross-posting/spam.
- Strip all corporate/marketing language and disclose any LLM assistance; lead every post with concrete numbers/screenshots/config before any link.
- Do not attempt an aggressive simultaneous post to all subs including main r/MachineLearning feed — flagged as highest-risk option (account nuke on a young profile).

## Сущности
- **Люди:** u/rm-rf-rm
- **Компании:** Anthropic
- **Продукты/инструменты:** Claude, Claude Code, CLAUDE.md, Reddit AutoMod, llama.cpp, exllama, Open LLM Leaderboard

## Открытые вопросы
- Exact wording of r/ClaudeAI's Rule 7 on referral links is only single-source/low-authority confirmed, not verified from primary sidebar text.
- r/AI_Agents self-promotion rule text was retrieved incomplete/truncated in crawls.
- r/SideProject has no formal numbered rule set in some crawls — norms inferred from secondary/marketing sources, not an official rules page.
- Whether the recommended posting sequence and spacing actually holds up in practice (author's own confidence label was 'emerging', not established).

## Источник
- DR-ID `DR26-07-20-HUB-03` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-20-ZB-01-reddit-voice-grok.md`

## Связано
- [[everything-becomes-content]]
- [[content-cascade-auto-execute-never-ask]]
- [[no-public-content-without-natasha-ok]]
- [[dr-fanout]]
- [[reddit-native-voice-playbook]]
