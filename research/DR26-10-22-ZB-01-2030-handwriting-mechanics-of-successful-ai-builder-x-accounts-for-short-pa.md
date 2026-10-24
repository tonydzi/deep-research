---
dr_id: DR26-10-22-ZB-01-2030
title: "Handwriting mechanics of successful AI-builder X accounts for short pain posts"
date: 2026-10-22
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-10-22-ZB-01-2030): Handwriting mechanics of successful AI-builder X accounts for short pain posts

> What writing mechanics and which accounts make short failure/pain posts perform on X in the AI-builder niche (2025-2026), with a few-shot corpus and a system-prompt recipe for daily lab posts.

> ⚠️ Поправка dr-judge 23.10: строка ниже «выжила только Gemini (2 из 6)» неверна по охвату: на диске ДВЕ ноги, grok (36 КБ, 94 ссылки x.com, ~34 из 40 постов достал) и gemini. Заметка написана по gemini, числа grok не учтены. Механика по grok: 29% постов начинаются со строчной, 29% кончаются точкой, 0 хэштегов, 79% от первого лица (у gemini 42,5% строчных, 72,5% без точки). Корпус gemini без URL = недоказуемый; брать корпус grok. Потребители `/x-post`, `/ai-slop` ссылаются на этот ДР: при следующей правке пересобрать образец по grok. Связь: MOC-Deep-Research-2026-09. Бэкап: `.prebak`.

## Ключевые выводы
- Only the Gemini leg of the DR survived (2 of 6 legs on disk). Its 40-post corpus has serious provenance problems. Most posts are tagged search snippet or page retrieved with no URLs. Several look fabricated or misattributed: the levelsio post dated 2026-10-21 about PHP, the Karpathy Medium-sourced quote, and the Danny Postma 2023 Stripe 'jack smith' posts cited via a non-primary site. The engagement numbers cannot be verified. Treat the corpus as unverified candidates, not ground truth.
- Gemini's claimed mechanics over N=40: mean length about 207 chars; 42.5% start lowercase; 72.5% omit the final period; 15% use '...'; mean 0.8 line breaks (65% single block); sentences 1/2/3 = 15/55/30%; first person 92.5%; hard numbers in 85%; question endings 7.5%; emoji 5%; hashtags 0%. These frequencies are computed on a corpus that is not verified, so they are plausible but not proven.
- Openings in the claimed corpus: in media res or action verb 45% ('Spent 4 hours...', 'Pushed an update...'), direct object or system 27.5%, casual connector 27.5%. Endings: flat technical consequence 47.5%, self-deprecating punchline 35%, unresolved action 17.5%. No engagement-question closers.
- What the report says separates a performing pain post from a flat one: granular specificity (named flags, tools, error types, dollar sums, row counts, durations), real stakes (money lost, users locked out), admission of own mistake, temporal proximity (just happened), and no retrospective moral. The report says 2024-2026 X ranking weights replies and bookmarks over likes, but cites no study or primary source for this.
- Anti-patterns that read as corporate or AI in 2026: 'Key takeaways' blocks and 'lessons learned' closers, symmetrical balanced sentences, abstract adjectives (seamless, crucial, testament, pivotal, groundbreaking) instead of named artifacts, engagement-bait questions, polished capitalization and punctuation, and hashtags.
- The account list has 18 handles in the 10k-500k tier, among them levelsio, karpathy, simonw, t3dotgg, dannypostmaa, marc_louvion, nikitabier, swyx, aarondfrancis, asmartbear, raunofreiberg, mckaywrigley, mitchellh, shl, rauchg. Follower ranges are given without sources. Some handles are doubtful: 'antontroshin' and 'abacaj' are listed as separate entries with unclear identity, and karpathy and rauchg exceed the 500k ceiling in reality.
- The RU-speaking comparison is thin and unsupported. It names Anton Troshin, Abacaj, Denis Shiryaev and Boris Tane without showing any of their posts or metrics, and its claims (deadpan, higher technical density, shorter openings) are not backed by data. Task 3's RU comparison is effectively unanswered.
- The report delivered a 10-line system-prompt recipe: first person, open mid-thought or with an active verb, 2-3 asymmetric sentences, exact technical entities, no moral, no hashtags or emoji, no question closer, drop the final period about 75% of the time, lowercase start for terminal or code friction, ban promo words, output post text only.

## Рекомендации / решения
- Do not use the Gemini corpus as few-shot examples until each post is opened on x.com and checked verbatim, with its date and metrics. Drop any post that cannot be found.
- Re-run or complete the remaining 4 legs (chatgpt, grok, claudeai, glm, mistral) and require URLs for every post. Reject any leg that has no URLs or no retrievable posts, per the quorum rule of 4 of 6.
- Use the qualitative rules (specific entity plus number, first person, no moral, no hashtags, no closing question) as a working draft for /x-post and /ai-slop, but mark the numeric frequencies as unverified.
- Do not hard-code '75% omit the final period' or '42.5% lowercase' into the prompt as fixed rates. Use them as soft tendencies, and check against a verified sample first.
- Measure mechanics ourselves from a verified sample of real posts, using a script to compute lowercase share, period share, length and number counts, rather than trusting LLM-reported tallies.
- Test the 10-line recipe on real voice notes in shadow mode (draft only, no posting) before it feeds the daily X flow, per the shadow-first rule.

## Сущности
- **Люди:** Pieter Levels, Andrej Karpathy, Simon Willison, Theo Browne, Danny Postma, Marc Lou, Nikita Bier, Shawn Wang (swyx), Aaron Francis, Jason Cohen, Rauno Freiberg, Mckay Wrigley, Thorsten Ball, Mitchell Hashimoto, Sahil Lavingia, Guillermo Rauch, Anton Troshin, Anton Abacaj, Denis Shiryaev, Boris Tane
- **Компании:** Vercel, Gumroad, Stripe, PayPal, Anthropic, xAI, Postmark, WP Engine
- **Продукты/инструменты:** X (Twitter), Claude Code, SQLite, vLLM, Datasette, TrustMRR, HeadshotPro, Ghostty, Zig, Ollama, Next.js, NotebookLM

## Открытые вопросы
- Which of the 40 posts are real, and what are their actual dates and metrics? None is verified with a URL.
- Is there any creator-reported or published data (2024-2026) that supports the claimed ranking weights and the pain-post mechanics? The report cites none.
- What do the results from the other 4 vendor legs (chatgpt, grok, claudeai, glm, mistral) say, and do they agree on frequencies and on the account list?
- Which RU-speaking tech X authors have measurable success in this format, and what do their real posts look like?
- Do the follower ranges and the identities of the doubtful handles (antontroshin, abacaj) hold up when checked?
- Does the 'no moral, no closer' pattern also hold on posts with high bookmark counts, or only on high-like posts?

## Источник
- DR-ID `DR26-10-22-ZB-01-2030` · реестр _DR-Registry
- оригинал: «внутренний архив лаборатории»
- оригинал: «внутренний путь лаборатории»

## Связано
- build-in-public X posting
- platform voice profile for X
- AI slop detection and humanization
- few-shot corpus for style imitation
- Deep Research quorum and leg verification
- Shadow-first rollout for content flows
- pain and failure posts as content

## 🤔 Факт-линт (dr_fact_lint)
<!-- авто-секция dr_fact_lint.py: перезаписывается при каждом прогоне, при разборе вырезается -->
Проверено 2026-10-23 через gh: 0 ссылок · ✅ 0 · ⚠️ 0 · ❓ 0 · ⚪ 0 · 🤷 0.
