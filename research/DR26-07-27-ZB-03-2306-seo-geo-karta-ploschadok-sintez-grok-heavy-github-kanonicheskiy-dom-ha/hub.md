# DR26-07-27-ZB-03-2306 — raw report from hub

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "DR26-07-27-ZB-03-2306 — SEO+GEO platform map (Grok Heavy, 551 источник) — RE-FETCH с хаба"
dr_id: DR26-07-27-ZB-03-2306
type: reference
stage: raw
status: collected
vendor: grok
mode: Heavy
sources_claimed: 551
created: 2026-07-27
retrieved: 2026-07-29


source_url: https://приватный чат
provenance: "RE-FETCH из живого чата Grok, выполнен ХАБОМ 29.07. Отчёт был собран на HP17-ZBook 27.07 и сохранён в её ЛОКАЛЬНУЮ E:\\Obsidian\\_originals — папку, которая НЕ является Syncthing-шарой, поэтому на хаб не доехал (инцидент, карточка task-2026-07-28-originals-ne-doezzhayut-do-haba). Три пинга ноуту остались без ACK. Текст снят со страницы чата целиком."
duplicate_warning: "Если ноут донесёт свой файл, он ляжет под СВОИМ именем (без суффикса -REFETCH-HUB). Это НЕ ошибка-дубль, а два снимка одного отчёта. Ноутбучный считать ПЕРВИЧНЫМ (сохранён в момент генерации), этот — страховочным. Схлопывать вручную, сверив sha256."
tags: [deep-research, geo, seo, platform-map, grok-heavy, re-fetch]
---

> ⚠️ Это RE-FETCH со страницы чата, а не файл, сохранённый в момент генерации. Расхождения с ноутбучной копией возможны (рендер чата мог свернуть часть вывода — в частности список из 551 источника свёрнут в счётчик). Первичной считать копию ZBook, если она доедет.

# DR26-07-27-ZB-03-2306 — SEO + GEO platform map for a public human+AI research lab

_Источник: https://приватный чат · Grok Heavy · «Thought for 47s» · 551 sources_

## A. Executive Summary

For a two-person AI research lab publishing bilingual (RU/EN) technical content on multi-agent systems, governance, consensus, and memory, the 2026 evidence favors a narrow, high-ROI platform set. Classic SEO (human discovery by engineers and frontier-lab recruiters) and GEO (retrieval/citation/attribution by ChatGPT, Claude, Gemini/AI Overviews, Perplexity, Grok, Yandex Alice, and coding agents) diverge sharply.

GitHub (repos + markdown + Pages) is the single highest-leverage surface for both training-corpus inclusion and agent-facing discovery. Habr and VC.ru deliver measurable GEO weight in Russian and technical niches (and some Western crossover). X and Telegram drive human visibility with near-zero GEO value. Own sites matter for entity consolidation and canonical control but have low inherent GEO weight until authority is earned. Reddit remains high-potential but high-risk/volatile. Medium, Substack, Hashnode, HackerNoon, and most general blogging platforms are low-ROI or actively hostile for a small lab.

The prior internal assumption that "VC.ru and Habr are quoted almost verbatim by LLMs" is partially confirmed for RU/tech contexts by Russian primary analyses and unverifiable/low for pure English Western prompts (they do not appear in top global citation lists). Prioritize native technical long-form on GitHub + selective Habr/VC.ru + consistent entity signals over broad syndication. Cut or deprioritize anything requiring high maintenance for marginal citation share.

## B. Key Findings

### GEO mechanics (mid-2026 evidence)

Citation share is highly engine-specific and volatile. Reddit frequently ranks high (Profound ~21% of AIO citations; Semrush up to ~40% in some windows) but collapsed from ~60% to ~10% of ChatGPT responses in a matter of weeks in late 2025 after infrastructure changes. Wikipedia, YouTube, and earned media dominate other slices. Domain overlap across engines is low (max ~17%, often 2–14%; 78–85% of cited domains unique to one engine). Citation persistence is poor (~10.6% URL-level over 28 days). [Arvow +2]

Earned media and brand mentions outperform owned content and pure backlinks. Ahrefs correlation: brand mentions 0.66–0.71 vs backlinks 0.27–0.33 for AI citation probability. Original research/data-rich content and technical depth score highest. [Resources.averi +1]

Retrieval-time (live search inside ChatGPT/Perplexity/Gemini) differs from training-corpus inclusion (Common Crawl, The Stack, licensing). Live retrieval drives most attributable citations and name recommendations today; training inclusion creates longer-latency "ambient" knowledge. Licensing deals (Reddit–OpenAI ~$70M/yr, Reddit–Google ~$60M/yr) do translate into elevated presence, but renewal talks in July 2026 are tense — Reddit is weighing tighter access or non-renewal because AI Overviews destroy referral traffic. [Winbuzzer +2]

Academic anchors: Chen et al. (2025) document earned-media bias and big-brand effects; Zhang, He & Yao (2026) separate selection from absorption (ChatGPT cites fewer sources more deeply). [Capston]

### Crawler policies (verifiable as of mid-2026)

Major engines now split training (GPTBot, ClaudeBot, Google-Extended, CCBot) from search/retrieval (OAI-SearchBot, Claude-SearchBot, PerplexityBot, ChatGPT-User, Claude-User). Compliant bots honor robots.txt. Allowing search bots while optionally blocking training is the common GEO-oriented posture. [Dataimpulse +1]

- Reddit: restrictive default (`User-agent: * Disallow: /`) + Public Content Policy; licensed access for OpenAI/Google.
- GitHub: largely open for public content (specific path disallows only).
- Medium: explicitly Disallows GPTBot and ClaudeBot on most paths (exceptions for a few corporate pages).
- Habr/VC.ru: generally open; Russian GEO practitioners recommend explicit Allows for the major AI bots.
- X: historically blocks GPTBot/ClaudeBot (Grok still reads it).
- Own sites / GitHub Pages: fully controllable.

Changes in the last 12–18 months: clearer training-vs-search split; Cloudflare Content Signals and pay-per-crawl experiments; continued publisher opt-outs.

### RU-language question

Russian analyses (150 M AI-link study on VC.ru, GeoScout State of AI Search Russia 2026, citation audits of 158 publications) show Habr and VC.ru receive meaningful citations/referrals from ChatGPT, Gemini, Perplexity, and especially Yandex Alice/Neuro/GigaChat. Brands active on them report 23–31% higher domain citation rates in technical niches. Western global top-lists rarely surface them, indicating a language/niche effect. Cross-lingual retrieval exists but is imperfect — RU content can surface in EN answers on technical topics when authority is high, yet the pools remain partially separate. Publishing in Russian does not hurt English-speaking recruiters if the technical substance and GitHub/entity signals are strong; it expands the Russian AI/engineering talent and research surface. [Geoscout +2]

### Duplicate content, canonical, syndication

Classic SEO: multiple near-identical copies without rel=canonical risk cannibalization and diluted ranking. GEO: engines appear to dedupe by content similarity/hash and preferentially select higher-authority or first-seen sources; exact mechanics are opaque. Native rewrites of the same claim across independent high-trust domains increase corroboration probability (multiple independent sources raise selection odds). Medium, dev.to, and Hashnode support setting an external canonical (import tools or front-matter). Habr/VC.ru do not give real external rel=canonical control. "Same text everywhere" is inferior to "N native technical rewrites + one canonical home."

### Entity / attribution layer

Consistent naming + schema.org Person/Organization + sameAs (Wikidata, ORCID, Scholar, GitHub org, personal sites) + arXiv/GitHub presence measurably improve named attribution over anonymous description. Wikipedia remains a high-weight anchor (frequently top-cited domain) but notability thresholds (independent secondary coverage in reliable sources) make it unrealistic for a two-person lab in the near term — high AfD risk. Wikidata is lower-friction. arXiv preprints and high-star GitHub repos receive disproportionate citation in AI/agent/technical queries relative to generic blog posts. [Prnewswire]

### Metrics of consumption

Cheapest proofs: (1) server logs of AI-crawler hits, (2) GitHub traffic insights, (3) native platform stats (Habr/VC.ru views, Telegram), (4) weekly scripted prompt-panel tests across engines for name/mention/citation of specific techniques, (5) UTM-tagged links from platforms to own site. ChatGPT referrers (chatgpt.com / chat.openai.com) dominate trackable AI referral traffic; many AI-influenced visits later appear as branded search or direct. Time-to-first-citation for new pages is often measured in days to a few weeks (median figures around 1 week in some datasets), with high volatility thereafter.

## C. Contradictions & Debates

- **Reddit citation share:** high in aggregate Profound/Semrush data vs. near-zero after intent/location controls (Yext) and measured rejection rates by OpenAI. Volatility is the consensus; licensing still elevates presence.
- **Habr/VC.ru "quoted almost verbatim":** confirmed in Russian primary traffic/citation analyses for RU and tech niches; absent from Western top-domain lists. **Вердикт: partially confirmed / context-dependent [emerging].** Несущее допущение держится для русской аудитории и технических тем, но универсальным считать нельзя.
- **Training vs. retrieval:** ambient model knowledge vs. live citation — live is higher priority for measurable attribution in 2026.
- **llms.txt:** adopted by ~5–9% of top sites and growing, yet server logs show near-zero fetches by major engines; Google explicitly rejects it for ranking/AI Overviews. Useful mainly for coding agents.
- **Multi-platform syndication:** classic-SEO risk of cannibalization vs. GEO corroboration benefit of independent native rewrites. Evidence favors the latter when content is substantially rewritten.

## D. Best Practices

- Canonical home = GitHub markdown / repo (ingestible by The Stack, coding agents, training pipelines). Explicit text pointer + real rel=canonical where platforms allow.
- Native rewrites, never pure copy-paste. One core technical claim then platform-specific framing and depth.
- Entity hygiene first: consistent "Palo Alto AI Lab" / person names, ORCID, Scholar, arXiv, GitHub org, schema, sameAs graph.
- Allow AI search/retrieval bots; decide training bots on principle.
- Measure with free prompt panels + crawler logs rather than paid SOV tools.
- Genre discipline: technical depth, original claims, data, or working code. Avoid "about us" self-promo.

## E. Failure Modes

- **Habr:** explicit ban on unauthorized advertising/self-promo; "articles about ourselves" rejected (совпадает с историей лаборатории: 4 отказа). Company blogs or the "Я пиарюсь" hub only. Sandbox has strict uniqueness, IT-relevance, and non-generated-content rules.
- **Reddit:** 9:1 rule or outright bans in most founder/tech subs; new/multi-account/link-heavy behavior triggers shadowbans. Prior bans already present.
- **Hacker News:** Show HN only for tryable projects; pure promotion flagged; domain/account history matters.
- **Medium et al.:** AI-content policies evolving; many block training crawlers; paywalls and low authority limit GEO.
- Broad syndication without native value or canonical control leads to spam classification, diluted signals, community backlash.
- Premature Wikipedia attempt leads to AfD and reputational noise.

## F. Strategic Recommendations

Platform decision table (evidence tier: established / emerging / speculative)

| Platform | GEO weight | Human-traffic weight | Language | Content genre that passes | Canonical handling | Cheapest consumption metric | Verdict |
|---|---|---|---|---|---|---|---|
| GitHub repos + markdown | High (training + agents) [established] | High (engineers) | EN primary | Technical longreads, code, dev-logs | Full control (canonical home) | GitHub traffic API + stars/forks | **INVEST** |
| GitHub Pages (scholar/hiring) | Medium-High | Medium | EN/RU | Technical + hiring | Full control + robots Allow | Server logs + crawler hits | MAINTAIN |
| наш сайт | Low-Medium (entity) | Low | EN/RU | Lab overview + pointers | Full control + llms.txt | Server logs + AI crawler hits | MAINTAIN |
| Habr | Medium-High (RU/tech) [emerging] | High (RU eng) | RU | Deep technical, original research | None external | Native views + prompt tests | **INVEST** (genre discipline) |
| VC.ru | Medium-High (RU/tech) [emerging] | High (RU) | RU | Case studies, expert analysis | None external | Native views + prompt tests | **INVEST** (genre discipline) |
| X / @Tony_Stef_ | Low (bots blocked) | High (recruiters) | EN | Short technical claims + links | N/A | Impressions + profile visits | **INVEST** |
| Telegram (RU/EN channels) | None | High (community) | RU/EN | Longreads + discussion | N/A | Native stats | MAINTAIN |
| Reddit | High but volatile [emerging] | Medium | EN | Value-first technical discussion | N/A | Hard (shadowban risk) | MAINTAIN carefully / CUT if risk high |
| arXiv | High (attribution) [established] | Medium (academic) | EN | Preprints | N/A | Downloads + citations | **INVEST** (when ready) |
| Google Scholar / ORCID | Medium (entity) | Low | EN | Profiles + papers | N/A | Profile views | MAINTAIN |
| Medium / Hashnode / dev.to / HackerNoon | Low-Medium | Medium | EN | Technical with external canonical | Supported on most | Platform stats + UTMs | CUT or selective only |
| Hacker News | Low-Medium | High (if Show HN succeeds) | EN | Working projects only | N/A | Points + comments | Selective (projects only) |
| HuggingFace | Medium (agents) | Medium | EN | Models / Spaces / Datasets | N/A | Downloads / likes | **INVEST** for artifacts |
| Facebook / personal | None | Low-Medium (RU) | RU | Medium-length | N/A | Native | CUT or minimal |
| Substack | Low | Medium | EN | Newsletter | Limited | Subscribers | CUT |
| Wikipedia / Wikidata | High (if present) [established] | Low | Multi | Not applicable | N/A | Mentions | Wikidata yes; Wikipedia later/skip |

## G. Action Plan (ordered by ROI for a two-person team; first 5 doable in a week)

1. Make GitHub the explicit canonical home for every substantial piece; add a consistent "canonical source: [repo URL]" text line + ensure robots.txt Allows AI search bots on own sites and GitHub Pages.
2. Run a one-week prompt-panel baseline: 20–30 target queries (multi-agent orchestration, agent consensus, second-brain systems, etc.) across ChatGPT, Claude, Perplexity, Gemini, Grok, Alice; record name mentions, citations, and sources.
3. Publish or polish 1–2 high-quality technical pieces on Habr and/or VC.ru that are not self-promo — pure technical contribution that happens to reference lab work.
4. Strengthen entity layer: consistent naming everywhere, ORCID/Scholar updates, schema on own site, Wikidata entry if feasible, GitHub org polish.
5. Add lightweight server-log monitoring for AI crawler hits and UTM tracking on outbound links from X/Telegram/Habr.
6. (Week 2+) Selective Show HN or HuggingFace Space only when there is a runnable artifact.
7. Cut or freeze: pure Medium/Hashnode/dev.to syndication, Facebook volume, any Reddit activity that risks further bans, Substack.

**Single recommendation [emerging confidence]:** Treat GitHub as the permanent technical and agent-facing home, Habr/VC.ru as the primary RU GEO surface under strict genre rules, and X/Telegram as human-distribution channels. Everything else is secondary or cut. Main assumption: the lab's technical claims are substantive enough that native presence on high-trust technical surfaces will generate the corroboration and entity signals that drive both human discovery and LLM attribution.

## H. Sources

551 sources (Heavy mode).

⚠️ Полный список источников на странице чата свёрнут в счётчик «551 sources» и в теле ответа не развёрнут — при необходимости раскрывать прямо на странице https://приватный чат . Инлайн-атрибуции, которые Grok отрендерил в тексте: Arvow, Resources.averi, Winbuzzer, Capston, Dataimpulse, Geoscout, Prnewswire.

---

## Промпт

Полный текст исходного промпта: «внутренний путь лаборатории». На странице чата он продублирован эхом перед ответом.
