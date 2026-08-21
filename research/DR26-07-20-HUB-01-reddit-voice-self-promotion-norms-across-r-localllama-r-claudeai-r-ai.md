---
dr_id: DR26-07-20-HUB-01
title: "Reddit voice & self-promotion norms across r/LocalLLaMA, r/ClaudeAI, r/AI_Agents, r/Machin"
date: 2026-07-20
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-20-HUB-01): Reddit voice & self-promotion norms across r/LocalLLaMA, r/ClaudeAI, r/AI_Agents, r/MachineLearning, r/SideProject

> Deep-research mapping of each subreddit's written rules, unwritten culture, winning-post anatomy, and a safe founder playbook for disclosing AI/agent projects without triggering anti-astroturfing/anti-slop backlash.

## Ключевые выводы
- Core conclusion: the durable 'ghost style' on Reddit is not concealment but transparent founder-authorship edited into the local community's preferred evidence format — 'I built this, here's the technically useful part, here's what failed' — never 'I stumbled across this' when you own it.
- r/LocalLLaMA: strictest technical bar; formal rule bans primarily-LLM-generated copy/code, requires 1-in-10 self-promotion ratio and affiliation disclosure; April 2026 mod update added account gates (10 karma to comment, 5 subreddit-earned comment karma to post) and began auto-removing slop with accepted false-positive rate.
- r/ClaudeAI is the most showcase-friendly and the best natural fit for a 'built with Claude' project post; requires 50+ karma for feed showcases (else megathread), minimal promotion, free-to-try product, and explicit description of how Claude helped; vote manipulation is an explicit instant-permaban trigger.
- r/AI_Agents values production failures, reliability math and honest 'this didn't need an agent' takes over hype; rules route product links to comments/weekly Project Display thread (not the main post body) and enforce a 1-in-10 promotional ratio; no public account-age/karma threshold was found.
- r/MachineLearning is the strictest main feed of the five: paid-product promotion is banned unless it's genuinely a research/engineering contribution (baselines, ablations, code/data); marketing-styled [P] posts risk permanent bans and purge of prior contributions; self-promotion belongs in the dedicated thread.
- r/SideProject has the highest formal tolerance for self-promotion (no numeric ratio rule found, just sitewide Reddiquette) but the lowest tolerance for ad-shaped writing; personal story, embarrassing mistakes, and visual proof (video/GIF) outperform landing-page copy and unexplained MRR screenshots.
- Across all five communities, 'AI slop' detection is now common culture (not just formal rule) — signals include unearned polish, symmetrical bullet structure, generic corporate vocabulary, missing hardware/data/config specifics, and inflated/unverifiable claims; but single stylistic tics (e.g., one em dash) are not reliable tells — communities are split on how much surface style matters versus whether operational details survive questioning.
- Winning-post title formulas are highly consistent per subreddit (e.g., LocalLLaMA: '[Tool]: [specific local capability]' or 'I built [constrained system] on [hardware]'; ClaudeAI: 'I built [system] with Claude Code that [outcome]. Just open-sourced it.'; AI_Agents: 'Stop building [hyped thing]' or 'Spent $X on [tool]. Everything worked in dev, nothing in production.'; MachineLearning: '[D] Got burned by [paper/company]'; SideProject: 'My silly [project] got its first paying user.').
- For the author's own 4-machine/multi-agent Claude fleet project, the report recommends different post framings per subreddit: LocalLLaMA only if there's a genuine local-model component; ClaudeAI is the best fit as a 'Built with Claude' failure/architecture story; AI_Agents wants reliability/state/retry metrics not a product link; MachineLearning requires converting it into an actual empirical study with baselines/ablations; SideProject wants the personal story and a demo video, not architecture depth.

## Рекомендации / решения
- Disclose authorship/affiliation in paragraph one of any post ('I built this' / 'I'm one of the maintainers') — never frame your own product as something you 'discovered.'
- Match evidence format to subreddit: exact hardware/config/benchmarks for LocalLLaMA; visible Claude-specific workflow detail (CLAUDE.md, skills, MCP, model choice) for ClaudeAI; reliability/state/cost metrics for AI_Agents; baselines/ablations/code+data for MachineLearning; personal story + video/GIF for SideProject.
- Keep self-promotion under the stated 1-in-10 ratio where a formal rule exists (LocalLLaMA, AI_Agents); route product/blog links to comments or dedicated self-promo/weekly threads rather than the main post body (AI_Agents, MachineLearning, LocalLLaMA style).
- Always include at least one visible limitation or failure before the project link; respond to 'this is an ad' accusations by acknowledging and adding raw data, not arguing or deleting/reposting.
- For the 4-machine Claude fleet project specifically: lead with ClaudeAI as the primary venue (best fit), post to LocalLLaMA only if a genuine local-model/open component exists, and only post to MachineLearning after converting the build-log into a real empirical study with fixed evaluation suite and baselines.
- Avoid formal 'open source' claims when core production features are gated/closed; avoid corporate/marketing vocabulary ('revolutionary,' 'seamless,' 'enterprise-grade') without hard data, especially on LocalLLaMA and MachineLearning.
- Do not coordinate votes, seed flattering questions from other accounts, or use referral/booking links in the main post body — these are explicit ban triggers (esp. ClaudeAI, AI_Agents).

## Сущности
- **Люди:** —
- **Компании:** Anthropic, Apple
- **Продукты/инструменты:** r/LocalLLaMA, r/ClaudeAI, r/AI_Agents, r/MachineLearning, r/SideProject, Claude Code, Heretic, TurboQuant, VeridisQuo, GraphZero, PyTorch Geometric, llama.cpp, vLLM, MLX, exllamav2, GGUF

## Открытые вопросы
- No current public numeric account-age/karma threshold was found for r/AI_Agents or r/MachineLearning (marked [no source found] in report) — thresholds may exist but be undisclosed/algorithmic.
- r/SideProject's rules endpoint has no subreddit-specific written rules beyond sitewide policy, yet visible discretionary moderation occurs — exact enforcement logic is unclear.
- Author-reported metrics (revenue, download counts, benchmark numbers) in winning posts are self-reported and not independently verified by the researcher; treated as reception evidence only, not product-claim evidence.
- Report content beyond r/SideProject section 5.6 was truncated in the retrieved document — original may contain additional sections (e.g., cross-community synthesis, full playbook comparison table) not captured here.
- No consensus within communities (esp. ClaudeAI, SideProject) on how much surface AI-writing style (em dashes, 'delve,' symmetrical bullets) should be treated as a reliable authenticity signal versus false-positive risk.

## Источник
- DR-ID `DR26-07-20-HUB-01` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\deep-research\DR26-07-20-ZB-01-reddit-voice-chatgpt.md`

## Связано
- [[everything-becomes-content]]
- [[content-cascade-auto-execute-never-ask]]
- [[no-public-content-without-natasha-ok]]
- [[short-text-when-unreviewed]]
- [[cofounder-identity]]
- [[model-routing-fable-smart]]
