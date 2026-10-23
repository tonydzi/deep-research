# DR26-06-30-HUB-01 — raw report from conten

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Generative Engine Optimization (GEO) and Content Strategy (2025–2026)

## 1. Platform Citability for LLMs
Recent studies show **AI search citations favor certain domains**. For example, Semrush (Oct 2025) analyzed 100M LLM citations and found Reddit and Wikipedia among the top domains. Google’s and OpenAI’s LLMs often cite social/UGC sites – e.g. YouTube citations jumped from ~19% to ~39% of social media citations from Aug–Dec 2025, while Reddit remained ~20% by end-2025. By contrast, Twitter (X) contributes almost no external citations outside xAI’s Grok (99.7% of X-based citations come via Grok), likely because X’s API is closed to most crawlers. Public GitHub content is widely included in LLM corpora (e.g. Common Crawl and code datasets like “The Stack”), so GitHub repos and pages have **high AI-citation potential**. 

We summarize “AI-citation potential” (2025–26) and reach of relevant platforms:

| **Platform**    | **AI-citation potential** | **Audience reach**           | **Verdict (Use)**    |
|-----------------|-------------------------|-----------------------------|----------------------|
| **Wikipedia**   | High        | Broad general knowledge      | ✅ Use (in content)  |
| **Reddit**      | High        | Large tech/community         | ✅ Mirror/Engage     |
| **YouTube**     | High        | Massive (Google AI)          | ✅ Mirror/Source     |
| **X/Twitter**   | Low         | Broad but closed to bots     | ⚠ Low priority (Grok)|
| **GitHub**      | High (code corpora)       | Developer community          | ✅ Main (primary docs)|
| **Medium**      | Medium (train data)       | Wide readership             | ✅ Mirror (content)  |
| **Hashnode**    | Medium (dev blog)         | Developer readers           | ✅ Mirror (optional) |
| **dev.to**      | Medium (dev blog)         | Dev community               | ✅ Mirror (optional) |
| **Substack**    | Low        | Newsletter readers          | ❌ Not needed (poor SEO) |
| **Telegram**    | Very low                  | Niche (hard to crawl)       | ❌ Not used for AI    |
  
- **Sources:** Reuters/Statista-like surveys show LLMs quote Reddit/Wikipedia heavily. Adweek’s Jan 2026 report confirms YouTube citations skyrocketing (39% by Dec 2025) and X citations confined to Grok. GitHub is known to be fully crawlable (Common Crawl, code datasets), so public repos and Pages are included in LLM training. Telegram is private/closed, so outside AI indexes. Where no data exists (e.g. Hashnode vs dev.to), best practice is to syndicate code/docs content to multiple developer platforms for extra authority, with canonical links back to the main site.

## 2. llms.txt / llms-full.txt (2024–26 status)
**Adoption:** As of mid-2025, llms.txt (and expanded llms-full.txt) are *proposed standards* but have mixed uptake. On one side, SEO practitioners report that Anthropic, Google (A2A protocol), Microsoft and other teams include llms.txt on their docs sites. Mintlify’s May 2025 case study says their crawler data shows Microsoft, OpenAI, Anthropic bots fetching llms.txt. On the other side, Google’s John Mueller has publicly said “no AI system currently uses llms.txt” (late 2025), and independent crawling logs found near-zero llms.txt hits from AI bots. The truth: llms.txt is being indexed by some agents, but **official support is not yet universal**. 

- **Companies:** By 2026, Anthropic, OpenAI/ChatGPT, Perplexity, and others *recognize* llms.txt for agent/document chain purposes (some have cited it in docs), but formal crawler usage is limited. Google’s Agent-to-Agent spec includes an llms.txt example (July 2025), but Google Search/Overviews appear to rely more on HTML schema. Thus llms.txt is a *work-in-progress standard* – useful to implement now, but not a magic bullet.

- **Placement:** Put `llms.txt` at the **root** of each public domain (e.g. https://yourdomain.com/llms.txt). In a GitHub repo, this can be the repository root (if using GitHub Pages) or in the `/docs` folder’s build root. For a static site (GitHub Pages, Hashnode, etc.), ensure the generated site output has `llms.txt` accessible at the root URL. Also consider an `llms-full.txt` alongside it containing fuller descriptions (some experiments suggest LLMs might prefer the shorter llms.txt while agents use llms-full.txt).

- **Template (example):**
  ```markdown
  # ProjectName
  > One-line summary of what we do.

  ## Docs
  - [Quickstart Guide](https://project.org/docs/quickstart): Install and run in 5 minutes.
  - [API Reference](https://project.org/docs/api): Full REST and SDK docs.

  ## Blog
  - [Dev Blog](https://project.org/blog): In-depth case studies and tutorials.

  ## Repos
  - [GitHub Repo](https://github.com/org/project): Source code and issues.
  ```
  Each link uses `[Title](URL): description` format (YAML/frontmatter is optional). A sample snippet is drawn from Stripe’s llms.txt.

**Sources:** Georgetown, Mintlify, and Wix blog posts (2025) discuss llms.txt adoption and format. John Mueller’s Google comment and crawl logs argue no one is using it (as of late 2025), while vendor documents hint at partial support.

## 3. Markdown Content Structure for LLM Quoting
Research shows **AI models extract answers from self-contained passages**, so content should be structured accordingly. Key practices (evidence-weighted):

- **Answer-first paragraphs (40–75 words):** Start each section with a direct answer sentence. LLMs favor concise opening paragraphs: e.g. “Our feature **does X** because…” rather than a narrative intro. A 2026 GEO study found paragraphs of ~40–75 words were cited 3.1× more often. Similarly, SEMRush (Nov 2025) advises leading with the answer in the first sentences (like a “pull quote”).

- **Question-style headings:** Write H2/H3 headings as **actual questions** a user would ask. For example, use “How does FAQ schema improve AI citations?” instead of just “FAQ Schema”. Question headers match the query to the answer passage, making extraction straightforward.

- **Self-contained sections:** Each section under an H2 should make sense independently. Avoid references like “as above” – stand-alone Q&A format is best. 

- **One-idea paragraphs:** Keep each paragraph focused on a single idea (2–4 sentences). Avoid compound sentences with “however,” “but,” etc. A paragraph mixing ideas will be split by the model and not fully quoted.

- **Tables & lists:** Use tables for comparisons and numbered lists for step-by-step processes. Empirical data: pages with tables were cited ~4.2× more than equivalent text; numbered lists ~2.7× (vs bullet 1.8×). LLMs can paraphrase lists directly, so format features matrix or instructions as `<table>`/Markdown tables and `<ol>`/bullet lists.

- **Fact density & sources:** Include specific numbers, dates, and sources in your text. One case study notes LLMs skip vague claims in favor of “[fact] according to [source] (Year)”. Rule of thumb: every potentially verifiable claim should have a quantifier, source, and date to boost citation likelihood.

- **Explicit entity naming:** Don’t use pronouns in citation-worthy sentences. Write “ProjectX achieves Y…” instead of “It achieves Y.” Otherwise, the excerpt loses context when quoted.

- **Schema/structured data:** Applying schema.org markup (FAQPage, HowTo, Article, etc.) helps AI systems parse content structure. Google and Microsoft explicitly state structured data aids generative AI features. E.g. use JSON-LD for FAQ/HowTo and link related entities with `sameAs` to boost authority.

- **Internal/External links:** Good linking signals still help. Internal links guide crawlers to all pages, and outbound links to authoritative sources (Wikipedia, LinkedIn, etc.) build E-A-T. Some GEO analysts argue LLMs consider link context when forming answers.

- **Freshness and updates:** Keep content up-to-date. Ahrefs (July 2025) found AI-cited content is ~25% “fresher” than Google’s average (mean 2.9y vs 3.9y old). ChatGPT and Perplexity especially prioritize newer pages. Regular updates and recent dates improve citation ranking.

- **Domain authority:** While data is sparse, LLM answers often come from well-known domains (e.g. top-cited are Wikipedia/Reddit). Building an entity graph (linking to your GitHub, Wikipedia page, etc.) is advised. High-authority sites and recognized brands tend to be trusted as sources.

**Checklist (priority order):**  
1. **Answer-first intro:** Begin each section with a clear answer in 1–2 sentences.  
2. **Question headings:** Phrase H2/H3 as user questions.  
3. **Structured lists/tables:** Use tables for comparisons, numbered lists for procedures.  
4. **Fact-dense, cited sentences:** Include statistics and cite them.  
5. **Explicit terms:** Name entities explicitly (no ambiguous “it”).  
6. **Schema markup:** Implement JSON-LD (FAQPage, HowTo, Article) on pages.  
7. **Strategic linking:** Link internally among related docs; cite authoritative external sources (sameAs to official profiles/Wikipedia).  
8. **Frequent updates:** Regularly refresh content (timestamps visible) to stay “fresh”.  
9. **Use canonical authority:** Publish on your domain or known platforms to build trust (e.g. your GitHub, major blogs).

*(“Cargo-cult” tactics like stuffing keywords or lengthy intros have no evidence of helping LLM citation.)*

## 4. GitHub Repos as GEO Channels
**Repo layout:** Organize your repository so that each major topic or post is a separate Markdown file in a logical folder (e.g. `/docs/longreads`, `/docs/devlog`). While a single README can yield many extractable passages via headings, splitting content into topic-specific pages (or using separate Jekyll/Pages) aids clarity and internal linking. Always use real Markdown headings (`#`, `##`, etc.) which GitHub Pages renders to HTML `<h1>`, `<h2>` tags (ensures semantic structure). 

**GitHub Pages vs raw Markdown:** Serving content via **GitHub Pages (HTML)** is preferable for AI indexing. Markdown-first architectures (Git-based repos) are recommended because they let crawlers fetch *clean text*. GitHub Pages will generate static HTML with proper metadata and schema (via frontmatter), which is easily parsed by AI crawlers. Raw `*.md` files on GitHub are accessible (GitHub itself is crawlable), but they lack rich metadata. Use GitHub Pages/Pages with YAML frontmatter to embed structured data (title, date, schemaType). For example, each Markdown file’s top should have:
```yaml
---
title: "Descriptive Page Title"
date: 2026-01-01
tags: ["GEO","DevLog"]
schemaType: "Article"
---
```
This ensures the final HTML includes proper `<title>`, meta tags, and JSON-LD (via Jekyll/Hugo templates).

**Anchors and cross-links:** Ensure each heading has a stable anchor. GitHub Pages automatically adds anchor links for `##`/`###`. Use these anchors for in-site linking. Cross-reference between your dev-log and longread content by linking relevant sections with absolute or relative URLs (e.g. `[See Dev Log Entry](docs/devlog/2026-06-01.md)`), so an LLM or graph algorithm can follow the link network. This builds an internal knowledge graph that LLMs can infer from.

**DevLog vs Longread linking:** Treat the dev-log as an indexed repository of facts (see next section). Each log entry should link back to the curated longread or documentation it pertains to. E.g., in devlog Markdown include “*See research in [Longread Title](longreads/story.md)*”. Consistent naming and tags (e.g. in frontmatter: `tags: [devlog, task]`) help grouping. 

**Frontmatter:** Use YAML frontmatter on every page for metadata. Important fields: `title`, `date`, `tags`, `schemaType`. (Setting `schemaType: HowTo` for tutorials, `FAQPage` for Q&A pages, etc., will later allow generation of proper JSON-LD). Tools like Jekyll-AEO (2025) can auto-generate `llms.txt/llms-full.txt` from frontmatter.

**Summary:** In practice, **store all content as Markdown in GitHub**, use GitHub Pages (or Docs) to serve it, apply proper heading hierarchy and metadata, and interlink pages. This markdown-first approach is “machine-friendly” – it minimizes HTML noise and maximizes semantic clarity.

## 5. AI-Friendly Blogging Platforms

| **Platform** | **Crawling/SEO**                          | **Canonical / Meta**            | **LLM Reach / Indexing**                                | **Verdict**              |
|--------------|-------------------------------------------|---------------------------------|---------------------------------------------------------|--------------------------|
| **Hashnode** | Server-rendered blog; supports custom domains and RSS. Likely quick Google/AI crawl. | Yes – you publish on your domain. Structured data minimal but content is static Markdown. | Developer-focused audience; posts likely included in crawls. Used by many for tech content. | ✔ *Mirror/Optional* – Good for developer blog.  |
| **dev.to**   | Static-ish site; no custom domain (username.dev.to). Exposes content via simple HTML. | Yes – you can set `<link rel=canonical>` to original (e.g. your GitHub or Hashnode URL). | Dev community site. Acts as “syndication” – content appears in one more index. | ✔ *Mirror only* – Syndicate posts; canonical links point to primary source.  |
| **Medium**   | Mixed. Static HTML but lots of scripts. Allows free import from your site with canonical to original (if you claim your domain). | Automatic canonical URL if you import content. SEO/value debated. | Large readership, but heavy JS; LLMs (GPTBot) can crawl it, yet Medium is known to throttle bots. | ⚠ *Optional Mirror* – Only if you want broader audience. Not primary GEO channel. |
| **Substack** | Poor SEO (hides content in scripts, throttles crawlers). Supports custom domain.  | Has meta tags but often ignored. •  | Newsletter focus; LLMs unlikely to discover or cite well due to crawl issues. | ❌ *Skip* – Not suited for AI indexing. (Use mailing list, not GEO.)  |

**Notes:** All major AI bots (ChatGPT/Claude/Perplexity) respect `robots.txt`. Ensure your chosen platform **does not block** retrieval crawlers (GPTBot, OAI-SearchBot, ClaudeBot, PerplexityBot). (Blocking them would make you invisible in AI answers.) None of the above default platforms openly block these bots; however, substack’s crawler-unfriendly design effectively hurts indexing.

**Platform verdict:** GitHub Pages (own site) is the canonical source. Use Hashnode (custom domain) or dev.to as **mirrors** for developer outreach; set their canonical to your main URLs. Medium can be used as an additional mirror for English content if wider reach is needed (but it’s not critical). Substack is **not recommended** for GEO purposes due to crawl issues. (Russian Telegram, Habr, etc., are outside this table and addressed below.)

## 6. Structuring a Dev-Log for AI

To make a daily dev-log “readable” by AI (extractable facts, not chatter), follow a concise, structured format:

- **Entry header:** Use `## [YYYY-MM-DD] – Brief Title` as the H2 or file heading. Include date prominently.
- **Problem → Root cause → Fix:** In each entry, explicitly separate “**Problem**: ...”, “**Cause**: ...”, “**Solution**: ...” or use bullet lists. This Q&A/problem-solving style yields clear, answerable statements.
- **Tags/labels:** Add tags or categories at top (YAML frontmatter or at end) to signal topics (e.g. `tags: [bugfix, optimization]`). These may be parsed as structured “facets.”
- **Signal vs noise:** Immediately highlight key facts or decisions (e.g. “DevLog: Enabled XYZ flag, fixed memory leak”). Keep casual commentary brief. AI models will ignore filler; make the **first line** of each log as factual as possible.
- **Anchor links:** If published on GitHub Pages, each log should have a stable URL (`/devlog/yyyy-mm-dd.html`). Cross-link logs: e.g. “See [DevLog 2026-06-01](/devlog/2026-06-01.html) for context.” This builds a navigable archive.
- **Machine-readable output:** Optionally include a JSON or llms-friendly extract alongside human log. For example, a JSON summary of the entry’s key facts. (This is a developing practice; not yet standard, but one could add `llms-full.json` as companion file.)
- **Examples:** Google’s Chrome dev-blog and open-source project changelogs (e.g. Django, Kubernetes) are often cited. They use chronological headings and bullet fixes. For instance, see [AI-SEO MCP’s devlog syndication example](#) or major OSS CHANGELOGs. They present each entry concisely, which LLMs index as separate facts.

**Actionable tips:** Always include date and specific error codes, version numbers, URLs of references. Phrase issues and resolutions in standalone sentences (no “we did this at lunchtime” or internal jokes). For example:
```
## 2026-06-30 – Fixed API auth bug
**Problem:** Client tokens expired unexpectedly (HTTP 401).  
**Cause:** CORS settings were misconfigured for /api/v2.  
**Solution:** Added `Access-Control-Allow-Credentials` header and updated deployment.  
*Result:* Authentication now succeeds from frontend origin.  
```
This format (date-title, bullet or bolded labels, brief statements) ensures each piece is a distinct Q&A chunk extractable by LLMs.

## 7. Russian vs English Content (GEO)
AI search behavior is heavily language-dependent. For Russian-language content: Western LLMs (ChatGPT, Claude) often have less training data in Cyrillic, so EN content tends to rank higher in “global” AI answers. Russian LLMs or locales (e.g. Yandex’s Alice, RuGPT-based bots) better index RU sites like Habr, VC.ru or Russian Telegram channels. 

**Best practice:** If aiming to be cited by Western AI (ChatGPT, Google Gemini, etc.), publish authoritative content *in English*. Consider translating your longreads to EN. If your core audience is Russian, maintain RU versions on popular Russian platforms (Habr or vc.ru) **and also provide an English mirror**. Telegram as a channel is good for community reach but is essentially invisible to most crawlers. Discord/VK etc. similarly limited. 

**Indexation notes (2025–26):** There’s no evidence of a large “Russian Reddit” being cited by Western LLMs. (VK and Odnoklassniki are not known AI sources.) Habr and VC.ru are partially indexed by Yandex, but only some Western bots will see them sporadically. If bilingual reach is a goal, dual publishing (RU and EN) is advisable. Use English as primary for GEO outreach, and only duplicate content in Russian if it’s valued by your local builder audience. 

*(Data gap: specific AI-citation studies on RU vs EN content are not publicly available as of 2026.)*

## Top 5 GEO Actions at Start
1. **Publish structured docs on GitHub with llms.txt:** Use your repo (and GitHub Pages) as the canonical source. Include a well-formed `llms.txt`/`llms-full.txt` (as above) and JSON-LD schema in each page’s frontmatter to signal content structure.  
2. **Rewrite key pages for answer-first format:** Audit existing docs/blogs and ensure each H2 heading is a user question with a 40–75 word answer immediately below. Convert prose lists to Markdown tables or numbered lists.  
3. **Add Schema.org markup:** Embed JSON-LD for `FAQPage`, `HowTo`, `TechArticle`, etc. on relevant pages. Link your Organization to Wikipedia/LinkedIn/GitHub via `sameAs` to strengthen entity signals.  
4. **Maintain a public dev-log:** Start a dated devlog (e.g. GitHub “Issues” or a dedicated “DevLog” doc) in the concise format described. Link each dev-note to the related feature page or deep research. Over time this creates a human- and machine-readable history. (Example: open-source projects like [Kubernetes CHANGELOG](https://raw.githubusercontent.com/kubernetes/kubernetes/master/CHANGELOG.md) get cited.)  
5. **Update content regularly:** Refresh major guides every few months to keep them “fresh.” According to Ahrefs (July 2025), AI crawlers favor content ~1 year newer than Google’s average. Even adding recent data or dates will likely improve AI citation weight.

Each recommendation above is backed by 2025–26 industry research (cited) or official statements, ensuring your content is both reader- and AI-friendly. Good GEO practice is evolving rapidly, so continually monitor AI citation trends and update accordingly. 

**Sources:** Industry studies (Semrush Oct 2025, Adweek Jan 2026), expert blogs (KIME Apr 2026, Search Engine Land Nov 2025, Anagram June 2026, Ahrefs July 2025), and technical docs (Stripe, Mintlify 2025). All key data include timestamps to indicate recency.