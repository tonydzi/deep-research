---
dr_id: DR26-07-03-HUB-02
title: "Distribution channels for a free, MIT-licensed Claude Code / Anthropic agent-framework OSS"
date: 2026-07-03
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-03-HUB-02): Distribution channels for a free, MIT-licensed Claude Code / Anthropic agent-framework OSS project

> Maps which channels (GitHub, MCP registries, Reddit, Hacker News, newsletters, launch boards, communities) actually drive discovery and adoption for education-first Claude Code/Anthropic OSS tooling, and in what sequence to use them.

## Ключевые выводы
- Best strategy is a compact loop, not 'launch everywhere': GitHub-native optimization → MCP/Claude directories → one discussion-native channel → one aggregator post → one curator/newsletter pitch, repeated with evidence not hype.
- Breakout comparables (browser-use 101K stars, Goose 50K+, OpenHands 77K+, mcp-agent 8.4K, Browser Harness 15K, CodeBurn 1,201 stars/2 days) all followed the same sequence: sharp pain-first framing + GitHub packaging → HN discussion or GitHub Trending spike → sustained curator/ecosystem embedding.
- Registry/awesome-list inclusion (mcp.so, Glama, PulseMCP with 20,100+ servers, Smithery, awesome-claude-code, best-of-mcp-servers) is discoverability substrate, not a demand generator — indexing ≠ distribution; these mostly compound wins that already have social proof.
- r/ClaudeAI (2.2M members, 2.1M weekly visitors) is the tier-one Reddit channel — rules explicitly reward free, educational project showcases (karma>50 required); r/Anthropic bans self-promotion outright; r/LocalLLaMA/r/selfhosted/r/opensource allow only limited, disclosed, production-ready self-promo.
- Hacker News is the primary breakout lever for one strong launch per repo/version, but the post title must state the pain, not the architecture — e.g. 'Recall — local project memory for Claude Code' hit 136 points while abstractly-framed agent/orchestrator posts got only 4-7 points.
- Product Hunt's centrality has declined for this category — it's now mainly useful as a later-stage social-proof/SEO wrapper once organic traction, testimonials, or a case study already exist, not for a cold start.
- Curator channels with technical/skeptical audiences (Latent Space ~189K subscribers/10M readers-listeners in 2025, Ben's Bites 168K+ subscribers, TLDR 1.6M readers, The Rundown AI 2M+ readers) outperform generic PR or founder-facing boards for this niche.
- GitHub-native packaging (precise topics, custom social preview, pain-first README, dependency graph/traffic visibility) is non-optional baseline infrastructure that improves every downstream channel.
- llms.txt/llms-full.txt adoption is cheap and aligned with Anthropic's own documentation practice, but SEO practitioners debate its standalone value — authority, freshness, crawlability and doc structure matter more than the file alone.
- GitHub stars are inflated ('fake star economy') but still function as a lightweight routing/discovery signal for awesome-list maintainers, HN readers, and directory browsers — not proof of adoption.

## Рекомендации / решения
- Run a 30/60/90-day rollout: Month 1 = GitHub repo packaging + registry/awesome-list saturation (mcp.so, Glama, PulseMCP, awesome-claude-code, best-of-mcp-servers) + one educational r/ClaudeAI post + one dev.to/Hashnode explainer + llms.txt setup; Month 2 = one polished Show HN + Peerlist Launchpad/DevHunt + newsletter pitches (Ben's Bites/TLDR/Rundown/Latent Space); Month 3 = a public mini case-study + awesome-list refresh + Uneed/Product Hunt (only with existing social proof) + a community talk/Discord workshop.
- Keep one repo, one story, one dominant discussion channel per release wave — don't fragment the same launch across five channels simultaneously; let one channel surface objections before adapting for the next.
- Widen first-contact messaging one layer above the technical implementation (e.g. 'governance for AI coding agents', 'multi-machine coordination without state drift', 'local-first production patterns for Claude Code') while keeping 'Claude Code'/'Anthropic' as secondary keywords in topics/README/directory tags.
- Explicitly skip: r/Anthropic for launches, spray-and-pray Reddit posting, Lobsters before earning participation credibility, Microlaunch, and long launch-directory marathons before social proof exists.
- Treat Product Hunt and Uneed as later-stage legitimacy layers, used only once testimonials/stars/case studies exist — not as first-wave discovery.
- Judge 90-day success by discussion-thread breakouts, curator mentions, visible case studies, and durable directory placements — not raw star count.
- Keep owned Telegram as the primary Russian-language channel; treat any other RU venue as experimental/relationship-driven, not core to the first 90 days.

## Сущности
- **Люди:** —
- **Компании:** Anthropic, GitHub, Reddit, Product Hunt, Hacker News, Peerlist, DevHunt, Uneed, BetaList, Microlaunch, Smithery, Glama, PulseMCP, mcp.so, daily.dev, Lobsters, dev.to, Hashnode, TLDR, Ben's Bites, Latent Space, The Rundown AI, Indie Hackers, Discord
- **Продукты/инструменты:** Claude Code, MCP (Model Context Protocol), browser-use, Goose, OpenHands/OpenDevin, mcp-agent, Browser Harness, CodeBurn, awesome-claude-code, best-of-mcp-servers, awesome-ai-agents-2026, mcpservers.org, llms.txt/llms-full.txt

## Открытые вопросы
- No high-confidence public traffic/audience figures were found for Hacker News, Lobsters, dev.to, Hashnode, DevHunt, or Peerlist — mechanics and culture were reported instead of unfoundable metrics.
- No clean 2025-2026 map of Russian-language Claude Code/MCP communities outside owned Telegram was established; flagged as needing a separate targeted audit.
- Launch-directory conversion data remains largely anecdotal; recommendations against certain platforms (e.g. Microlaunch) are fit-based, not proof the platform never works.
- Whether Product Hunt still meaningfully drives user acquisition versus serving mainly as SEO/social-proof remains an active practitioner debate.
- Whether GitHub stars retain real signal value given the 'fake star economy' criticism, versus their continued use as a routing/discovery heuristic, is unresolved.

## Источник
- DR-ID `DR26-07-03-HUB-02` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- distribution-map
- open-source-go-to-market
- mcp-registries
- github-discoverability
- second-brain-northstar
- everything-becomes-content
- alpha-protocol-recall-plus-dr
- claude-code-ecosystem
