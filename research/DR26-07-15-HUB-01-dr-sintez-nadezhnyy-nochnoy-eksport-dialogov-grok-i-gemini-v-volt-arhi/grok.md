# DR26-07-15-HUB-01 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: "Grok Build CLI 0.2.106 (grok-4.5, --effort high), SuperGrok Heavy"
dr_id: DR26-07-15-HUB-01
topic: "Надёжный ночной экспорт диалогов Grok и Gemini в Obsidian-волт"
date_collected: 2026-07-21



type: dr-report-original
verbatim: true
---

# DR26-07-15-HUB-01 — Reliable nightly export of Grok and Gemini chat histories into a local Obsidian vault (2026)

**Document type:** Implementation dossier (personal-data pipeline)  
**Audience:** Technical team operating an always-on Windows hub + macOS peers  
**Research window:** prioritised 2025–2026; sources retrieved 2026-07-21  
**Status:** Actionable architecture recommendation with confidence tiers  
**Scope:** (a) grok.com / SuperGrok (X SSO) · (b) gemini.google.com (Google account)

---

## 0. Executive summary

| Vendor | Best *stable* rail today | Best *nightly incremental* rail | Official API access to *consumer* chat history? | Verdict |
| --- | --- | --- | --- | --- |
| **Grok (xAI / grok.com)** | In-product **Settings → Data Controls / Export Data** → email/ZIP with conversation JSON (commonly `prod-grok-backend.json` in community parsers) | **Not first-class.** No public documented list/content REST API equivalent to ChatGPT’s backend harvest. Practical nightly = re-export + delta-by-conversation-id, or DOM/extension scrape | **No** — xAI Inference API is for *new* completions; not a dump of grok.com history | **Primary: official export + local converter.** Secondary: browser-session reverse-engineered calls (fragile). Tertiary: UI automation / userscripts |
| **Gemini (Google)** | **Google Takeout → My Activity → Gemini Apps** (JSON/HTML) | **Possible** via reverse-engineered web wrappers (`list_chats` / `read_chat`) or UI automation; Takeout itself is **batch, multi-hour, not nightly-native** | **No** — Gemini API / AI Studio is a *separate product surface*; does not list gemini.google.com threads | **Primary: scheduled Takeout (every 2 months) + optional weekly manual Takeout for denser cadence.** Secondary: reverse-engineered cookie API for true nightly. Tertiary: Playwright + exporter extensions |

**Standing opinion (confidence: high):** Neither vendor offers a ChatGPT-class “bearer + documented backend conversation API” for consumer history. Architect for **official bulk as truth**, **optional reverse-engineered/UI as incremental overlay**, and **silent-breakage detectors** as first-class code—not afterthoughts.

---

## 1. GROK — all viable rails (2026)

### 1.1 Official data export (Settings / Data Controls)

**What it is.** xAI’s consumer FAQ (dated **2025-05-12**) states users can access, download, and delete data via the Grok mobile app or **Grok.com → Settings / Data Controls**, with an additional privacy-portal path for formal data-subject requests.  
Source: https://x.ai/legal/faq (May 12, 2025). [established]

Exact FAQ wording:

> “You are in control to access, download, and delete your data. Please go to your Grok mobile app or Grok.com (Settings/Data Controls) to delete or download your data. … you can submit a request at https://x.ai/privacy-portal/.”

Privacy portal URL in FAQ: https://x.ai/privacy-portal/ (older community text also referenced Relyance/xai-privacy URLs; treat portal URL as living). [established for FAQ text]

**Community procedure (second-hand, not official docs).** Multiple 2026 guides describe:

1. Sign in at https://grok.com  
2. Profile → **Settings** → **Data**  
3. **Export Data** / download account data  
4. Email delivery of a download link (minutes to hours)  
5. Single JSON or ZIP containing conversation payload  

Sources (third-party / marketing-adjacent — tag accordingly):

- https://ai-chat-importer.com/blog/how-to-export-grok-conversations (Apr 12, 2026) — claims email JSON with `conversations` array, MongoDB-style timestamps. **[single-source schema detail] [low-authority]**  
- https://chatexport.guide/guides/grok/ (Jan 3, 2026) — claims https://accounts.x.ai/data download portal. **[single-source for this URL] [low-authority]**  
- https://github.com/Owlock/easy-grok-chat-exporter — states export ZIP contains **`prod-grok-backend.json`** as the conversation corpus (thinking traces included). **[emerging] [low-authority for filename, but tool exists and is concrete]**

**Disagreement to preserve:**

| Claim | Source side | Counter-source |
| --- | --- | --- |
| “Dedicated bulk Grok export exists (Settings → Export Data → JSON)” | AI Chat Importer (2026-04), Owlock exporter, several YouTube 2026 tutorials | LLMnesia (2026-04-19): “As of 2026, there is no single button to download all your Grok conversations at once”; Grok sits as “Partial (X archive)” vs ChatGPT bulk — https://www.llmnesia.com/blog/how-to-export-grok-conversation-history |
| Export is **one JSON file** | AI Chat Importer | Owlock: **ZIP** with `prod-grok-backend.json` inside |
| Export lives at **accounts.x.ai/data** | ChatExport.guide | Official FAQ points to **Grok.com Settings/Data Controls** + privacy portal — not accounts.x.ai |

**Implementation note (opinion, confidence: medium):** Treat “Settings → Export” as **real enough to automate in a logged-in browser** (Playwright click-path), but **do not hardcode a single file layout**. Detector must accept: bare JSON, ZIP-of-JSON, `prod-grok-backend.json`, or multi-file archives. Re-probe after every successful export.

**Incremental?** **No** by design—full snapshot each request. Incremental is **local**: key by conversation id, skip unchanged hashes.  
**Cadence feasibility:** weekly automatic export is plausible for personal scale; nightly re-export may be unnecessary and could hit soft rate limits (undocumented). **[speculative] rate limits: [no source found]**

**Delay / email:** “few minutes” (AI Chat Importer) vs “few minutes to few hours” (Owlock). Link expiry: **[no source found]** for official TTL; AI Chat Importer implies re-request if missing after 30 minutes. **[low-authority]**

**Format fields (community sample only):** conversation title, create_time (`$date/$numberLong`), responses with `sender` human/assistant, message text. Images/attachments often **absent** from export claims. **[single-source schema]**

---

### 1.2 X platform data archive

**Path:** https://x.com/settings/download_your_data → “Download an archive of your data”.  
Source describing Grok-related hope: https://www.llmnesia.com/blog/how-to-export-grok-conversation-history (Apr 19, 2026) — “Grok conversation data **may** be included depending on account configuration”.  

**X Help (Grok-on-X surface):** https://help.x.com/en/using-x/about-grok — documents **delete conversation history** under Privacy & Safety → Data sharing → Grok; does **not** document a dedicated Grok chat export in the help snippet retrieved. [established for delete path]

**Reliability for grok.com SuperGrok history:** **Weak / uncertain.** grok.com and X Grok can maintain **separate** histories. Community/Grok bot statement (2026-07-15): “Standalone Grok (grok.com + app) and X's Grok keep separate chat histories—no direct transfer.”  
X post: https://x.com/grok/status/2077189863640576013 (2026-07-15). **[emerging] X-only for separation claim; consistent with product split.**

**Opinion (confidence: high):** Do **not** rely on X archive as primary rail for grok.com SuperGrok Heavy. Use as optional forensic backup only.

---

### 1.3 Per-chat manual / semi-manual

| Method | How | Incremental | Fragility | Sources |
| --- | --- | --- | --- | --- |
| Copy-paste / Print-to-PDF | Browser | Per chat | Low technical fragility, high labor | LLMnesia 2026-04-19 |
| Share public link | grok.com Share → `grok.com/share-links` revoke | Per chat | Public exposure risk | xAI FAQ 2025-05-12 |
| Prompt “Export as PDF” | In-chat generation | Per content | Not a history dump | Grokipedia (Grok-fact-checked page, ~2026); **[low-authority wiki]** |
| Bookmarklet / console JS | User-generated scripts | Per open chat | DOM drift | Reddit r/grok thread https://www.reddit.com/r/grok/comments/1jl6k2x/… |

---

### 1.4 Reverse-engineered / web-app backend JSON

**Public documented endpoints:** **None found** in official xAI developer docs for “list my grok.com conversations” or “get conversation by id from consumer store”.  
xAI docs cover **Inference** (Chat Completions / Responses API): https://docs.x.ai/ — create new generations; Responses may store **API** interactions for ~30 days for continuation—not consumer UI history.  
Sources: https://docs.x.ai/developers/model-capabilities/text/generate-text ; https://docs.x.ai/developers/model-capabilities/legacy/chat-completions (retrieved 2026-07). [established]

**What reverse engineering would look like (engineering strategy, not a published API):**

1. Logged-in Chrome on grok.com.  
2. DevTools → Network filter `Fetch/XHR` while opening https://grok.com/history and individual chats.  
3. Capture request URLs, method, auth headers/cookies, response JSON schema.  
4. Replicate with `curl`/Python using harvested session cookies or bearer tokens (pattern analogous to ChatGPT bearer harvest).  
5. Map fields → local SQLite + markdown.

**Auth mechanics (observed product constraints, not full RE dump):**

- Login via **X SSO** for many accounts; also email/Apple/Google paths exist on accounts.x.ai surfaces.  
- Session = browser cookies on `grok.com` / `.x.ai` domains; exact cookie names and TTL **[no primary source found in this research pass]** — treat as “session cookie jar, re-login on 401”.  
- X SSO **does** complicate headless harvest: 2FA, device trust, cookie binding to browser profile, possible bot detection on high-frequency API replay. **[opinion, medium]**  
- SuperGrok subscription is **not** the same credential as `api.x.ai` API keys.

**Breakage modes for RE rail:** schema drift on every UI deploy; auth cookie rotation; WAF/bot challenges; dual history (X vs grok.com); rate limiting undocumented.

**ToS risk:** using private web endpoints at personal scale is common in second-brain tooling (ChatGPT pattern) but is **not** a blessed public API; enforcement evidence for personal read-of-own-data is sparse. **[speculative enforcement]** Prefer official export for ToS posture.

**Finding on published RE projects:** community exporters overwhelmingly use **DOM scrape of open chat** or **official export JSON parse**, not a maintained open-source “Grok backend client” comparable to HanaokaYuzu’s Gemini wrapper. **[emerging]**

---

### 1.5 xAI / Grok **API** vs consumer history

| Capability | Consumer grok.com | xAI API (`api.x.ai`) |
| --- | --- | --- |
| Chat with models | Yes | Yes (paid/API key) |
| Persist multi-turn in product UI | Yes (server-side history) | Client-managed or Responses store ~30 days for **API** turns |
| List historical grok.com threads | In UI / export | **No** |
| Retrieve past SuperGrok chats by conversation id | Via product | **No documented endpoint** |

**Conclusion [established]:** API is **strictly separate** from consumer chat archive. Do not buy API quota expecting vault import of past grok.com sessions.

---

### 1.6 Third-party bulk tools (extensions / userscripts)

| Tool | Approach | Notes | Link |
| --- | --- | --- | --- |
| AI Toolbox (formerly Grok Toolbox) | Chrome extension Manage Chats → multi-export MD/JSON/PDF | Commercial; claims bulk where native lacks it | https://www.ai-toolbox.co/ai-toolbox-grok-features/grok-bulk-export-delete-2026 (2026-07-08) **[low-authority marketing]** |
| LLMnesia | Local index while browsing | Not full offline dump without visiting chats | llmnesia.com **[low-authority]** |
| YourAIScroll | Extension export | Multi-platform | youraiscroll.com blog 2025-03-07 |
| enhanced-grok-export | Userscript multi-format + auto-scroll | https://github.com/iikoshteruu/enhanced-grok-export |
| easy-grok-chat-exporter | **Parses official export** `prod-grok-backend.json` → MD/txt/jsonl + thinking | https://github.com/Owlock/easy-grok-chat-exporter |
| revivalstack/ai-chat-exporter | Tampermonkey multi-LLM incl. Grok | https://github.com/revivalstack/ai-chat-exporter |
| BlackOrchardLabs/de-dobe | Userscript multi-platform | https://github.com/BlackOrchardLabs/de-dobe (2025-11) |
| DREwX-code/omnichat-exporter | GreasyFork multi-platform | https://github.com/DREwX-code/omnichat-exporter |

---

### 1.7 Grok rails comparison table

| Method | Effort | Fragility | Incremental? | ToS risk (own data, tiny scale) | Best use |
| --- | --- | --- | --- | --- | --- |
| Settings/Data Controls export | Low–Med | Low–Med (UI path drift; format drift) | Local delta only | **Lowest** among automated options [established intent of export] | **Primary bulk truth** |
| Privacy portal DSR | Med | Low frequency, slow | No | Lowest | Legal/forensic |
| X data archive | Med | Uncertain Grok coverage | No | Low | Secondary forensic |
| RE backend JSON | High | **High** (undocumented) | Yes if endpoints exist | Medium–High grey zone | Nightly if RE succeeds |
| Userscript/extension on open chat | Low | High (DOM) | Semi (visited only) | Grey; extension permissions risk | Spot backups |
| Playwright full UI crawl | High | High | Yes | Grey | Last-resort ladder |
| xAI API | Med | Low for *generation* | N/A for history | Low (paid ToS) | **Do not use for history** |

---

## 2. GEMINI — all viable rails (2026)

### 2.1 Google Takeout (official bulk)

**Correct product bucket (critical):**

1. https://takeout.google.com  
2. **Deselect all**  
3. Select **My Activity** (not the top-level “Gemini” product alone)  
4. **All activity data included** → Deselect all → check **Gemini Apps** only  
5. Prefer **JSON** under Multiple formats  
6. Next step → Create export  

Sources:

- University of Michigan KB: https://teamdynamix.umich.edu/TDClient/30/Portal/KB/Article/15439/Google-Export-Gemini-Chat-History — HTML chat log via My Activity → Gemini Apps. [established procedure]  
- Practical guide: https://univik.com/help/export-gemini-chats.html (verified Mar 2026 claim on page) — warns **“Gemini” product = Gems config, not chats**. **[emerging] [single-source for Gems-vs-Activity distinction detail]**  
- Converter that expects `Takeout/My Activity/Gemini Apps/MyActivity.json`: https://github.com/Coryrichter94/gemini-to-obsidian  

**Disagreement — completeness of Takeout:**

| Side | Claim | Source |
| --- | --- | --- |
| Takeout is the bulk archive of Gemini activity | Official community answers + UMich + multiple guides | Google Gemini Community threads; UMich KB |
| Takeout is **truncated / incomplete** vs live UI | “stripped-down prompt–response pairs with truncated content” | https://github.com/Liyue2341/gemini-exporter README **[single-source strong claim] [low-authority for completeness metric]** |
| Users report success with readable `MonActivité.html` / MyActivity | Reddit r/GoogleGeminiAI export thread | https://www.reddit.com/r/GoogleGeminiAI/comments/1l8qh84/… |

**Opinion (confidence: medium):** Use Takeout as **canonical bulk**, but run a **canary**: after export, sample N conversation ids that still exist in the live UI and assert length(export) ≥ length(UI) or flag underfill. If underfill is systematic, promote reverse-engineered or extension rail for full text.

**Scheduling every 2 months:** Google Takeout frequency option **“Export every 2 months for 1 year”** (6 automated exports).  
Sources: https://www.lifewire.com/what-is-google-takeout-4173795 (article updated; option documented for years); https://www.makeuseof.com/how-to-see-what-google-knows-about-you/ (2025-08-19); Univik Gemini guide FAQ. [established feature of Takeout UI]

**Automation of Takeout itself:** no official public REST to *create* Takeout jobs without browser/Google account UI. Practical automation: Gmail polling for “Your Google data is ready” + download link (pattern already used in takeout-pull style pipelines). Link expiry commonly cited as **~7 days** (Univik). **[emerging]**

**Format:** ZIP → `Takeout/My Activity/Gemini Apps/` → `MyActivity.json` and/or HTML; attachments may be co-exported. Exact JSON schema is **activity-log oriented**, not always one-file-per-conversation like ChatGPT. [established path; schema details vary by account/history]

**Incremental?** **No** at source. Local pipeline: parse → key by conversation/activity id → upsert.

**Nightly?** **Poor fit** alone (hours of processing, email latency). Pair with denser rail below.

---

### 2.2 My Activity (myactivity.google.com) as incremental rail

**Surface:** https://myactivity.google.com/product/gemini (also filter “Gemini Apps”).  
Users view prompts/history; community asks for downloadable list of all prompts (2026-02 support thread): https://support.google.com/gemini/thread/410548499/…  

**Rails:**

| Approach | Feasibility | Notes |
| --- | --- | --- |
| Manual browse | High | Not scalable |
| HTML scrape of My Activity pages | Medium | Pagination, infinite scroll, auth cookies |
| Takeout of My Activity (above) | High | Batch |
| Google “Download your data” from Activity controls | Same family as Takeout | |

**Auth:** Google session cookies (`SID`, `HSID`, `__Secure-1PSID`, etc.). Scraping My Activity HTML is **brittle** and bot-sensitive. Prefer Takeout over live HTML scrape for bulk. **[opinion, high]**

---

### 2.3 Per-response / per-chat product export

Official help: **Export responses from Gemini Apps** — Share & export → Docs / Gmail / Colab / Replit — **per response**, not full multi-turn history dump.  
https://support.google.com/gemini/answer/14184041 (retrieved 2026-07). [established]

Share chat link: separate help center path (related: share chats answer 13743730 referenced by Google). Public links are not a vault strategy.

---

### 2.4 Reverse-engineered web backend (gemini.google.com)

**Best-known maintained wrapper:** [HanaokaYuzu/Gemini-API](https://github.com/HanaokaYuzu/Gemini-API) (`pip install gemini_webapi`), **3.3k+ stars**, AGPL-3.0, active releases (v2.0.0 noted Apr 6, 2026).

**Auth:** cookies `__Secure-1PSID` + `__Secure-1PSIDTS` from browser Network tab after login to gemini.google.com; optional `browser-cookie3`; **background auto-refresh of PSIDTS**.  
Source: project README (retrieved 2026-07-21). [emerging as de-facto community standard]

**History-relevant methods (per README):**

- `GeminiClient.list_chats()` — list recent chats (id + title)  
- `GeminiClient.read_chat(cid)` — full turns newest→oldest  
- `delete_chat`, chat metadata resume  

This is the **closest analogue to ChatGPT backend harvest** for Gemini consumer UI.

**Breakage modes:** Google endpoint/protobuf/RPC changes (frequent historically for Bard/Gemini RE); cookie invalidation; account risk if aggressive; CAPTCHA; regional feature flags. Package history shows continuous maintenance precisely because of drift. [emerging]

**ToS:** reverse-engineering the web app is **against spirit of Google ToS** for automated access; personal export of own chats is a common grey-area use. Enforcement for tiny personal pipelines is rarely publicized. **[speculative]** Prefer Takeout for compliance narrative; use RE for *own* data only, rate-limit hard, no redistribution of client traffic.

**Older forks:** dsdanielpark/Gemini-API archived/less curated; points users to HanaokaYuzu. https://github.com/dsdanielpark/Gemini-API  

---

### 2.5 Official Gemini API / AI Studio / Vertex

**Relation to gemini.google.com history:** **None for retrieval of consumer chats.**  
API is generateContent / chat sessions **you manage** (or Interactions API state on Google’s API side)—not a mirror of the consumer app store.  
Sources: https://ai.google.dev/api ; multi-turn docs on Firebase AI Logic / AI Studio — history is client- or session-managed for **API** calls. [established]

**Do not** expect API keys to unlock vault import of past gemini.google.com threads.

---

### 2.6 UI automation / extensions

| Project | Approach | Link |
| --- | --- | --- |
| Liyue2341/gemini-exporter | Playwright CDP + AI Chat Exporter extension; scroll sidebar; one JSON per conversation id; skip existing | https://github.com/Liyue2341/gemini-exporter |
| Coryrichter94/gemini-to-obsidian | Takeout JSON/HTML → Obsidian MD + frontmatter | https://github.com/Coryrichter94/gemini-to-obsidian |
| sho7650/obsidian-AI-exporter | Chrome ext → Obsidian Local REST API (Gemini among others) | https://github.com/sho7650/obsidian-AI-exporter |
| Chrome “Export Gemini Chats to Obsidian” | Bulk MD | Chrome Web Store id `nidakehgcgmenmkfpacineilblijncjh` |
| Simple Exporter for Gemini | HTML/JSON/MD one-click | Chrome Web Store |

---

### 2.7 Gemini rails comparison table

| Method | Effort | Fragility | Incremental? | ToS risk (own data) | Best use |
| --- | --- | --- | --- | --- | --- |
| Takeout My Activity → Gemini Apps | Low | Low (Google-owned) | Local delta | **Lowest** | **Primary bulk / 2-month schedule** |
| Weekly ad-hoc Takeout + Gmail pull | Med | Low | Local delta | Lowest | Dense bulk without RE |
| My Activity HTML scrape | High | High | Yes | Medium grey | Avoid if RE or Takeout works |
| gemini_webapi list/read | Med | **High** (RE) | **Yes — best nightly** | Medium grey | **Primary nightly overlay** |
| Playwright + exporter ext | High | High | Yes (skip files) | Medium grey | Fallback when RE breaks |
| Per-response Docs export | Low | Low | Manual | Lowest | One-off important chats |
| Official Gemini API | Med | Low | N/A for consumer history | Low | **Not for history export** |

---

## 3. Reliability, anti-fragility, ToS

### 3.1 How often rails break (community evidence 2025–2026)

| Rail | Observed failure modes | Frequency signal | Sources |
| --- | --- | --- | --- |
| Grok official export | UI path renames; users disagree whether bulk export “exists”; email delay; format ZIP vs JSON | Product maturity friction; not catastrophic outage reports | LLMnesia vs AI Chat Importer disagreement; Owlock docs |
| Grok history sync bugs | Missing recent messages; single-chat rollback | Ongoing user reports | X: @WolfPoli17 2026-07-21 chat history rolled back — https://x.com/WolfPoli17/status/2079435799179972796 **[anecdotal]**; Grokipedia summary of user reports **[low-authority]** |
| Grok X vs standalone split | Wrong surface exported | Product design | @grok 2026-07-15 separate histories |
| Gemini Takeout | Wrong product selected (empty/Gems); incomplete vs UI (disputed); multi-hour latency; 7-day link expiry | High *user error* rate; medium process latency | Univik; Liyue2341; community threads |
| Gemini RE wrappers | Constant endpoint drift | **High** maintenance load | Star history + continuous releases of HanaokaYuzu |
| Browser extensions | DOM class renames; extension store policy; malware risk in unvetted tools | High for unmaintained scripts | General pattern; prefer open-source with recent commits |
| Cookie/session | Expiry, Google “unusual activity”, X SSO 2FA | Weekly–monthly ops load | RE READMEs (PSIDTS refresh) |

### 3.2 Account / bot-detection risk

- **Tiny personal scale, own account, human-like intervals:** practical risk is usually soft blocks / forced re-login rather than permanent ban. **[opinion, medium] [no hard enforcement stats found]**  
- **Parallel headless farms, high RPS, multi-account:** elevated ban risk—out of scope and not recommended.  
- **Extensions with broad permissions:** supply-chain risk to vault secrets; prefer offline converters of official exports.

### 3.3 ToS-safe ranking for *own* data (personal scale)

**Safer → greyer:**

1. Official product export buttons / Takeout / privacy portal  
2. Manual copy of own chats  
3. Local parsing of files Google/xAI emailed *to you*  
4. Browser automation of *your* session for *your* data (ChatGPT-style)  
5. Reverse-engineered private APIs at scale / redistribution / multi-tenant SaaS on stolen cookies  

**Documented enforcement:** public case law specifically for “personal Gemini/Grok history backup bot” **[no source found]**. Platforms document rights to access/delete data (xAI FAQ; Google Takeout). Scraping prohibitions in general ToS exist but are rarely detailed for this use case in the sources retrieved.

---

## 4. Recommended architecture

### 4.1 Design principles (shared)

1. **Idempotent upsert** keyed by stable `conversation_id` (+ content hash).  
2. **Raw archive first** (`_originals/` JSON/ZIP), then markdown notes.  
3. **Frontmatter** for vault graph: `source`, `conversation_id`, `exported_at`, `source_updated_at`, `model`, `url`, `hash`.  
4. **Canaries:** known conversation ids; minimum message counts; “zero new chats for 7 days while usage continues” alert.  
5. **Fallback ladder** encoded as state machine, not tribal knowledge.  
6. **Run on always-on hub**; Mac peers only for interactive re-auth if needed.  
7. Prefer **subscription/included** rails (export, Takeout) over paid APIs for history.

### 4.2 Grok pipeline (recommended)

```
[A Official Export] ──weekly──► raw ZIP/JSON
        │
        ▼
  normalize_schema() ──► conversations[] with id, title, messages[], thinking?
        │
        ▼
  delta vs state.db (conversation_id + content_hash)
        │
        ├── new/changed ──► write markdown + update index
        └── unchanged ──► skip
        │
[B Optional nightly] RE/session harvest OR Playwright history crawl
        │ only if A is stale > N days OR canary fail
[C Last resort] Userscript batch under human supervision
```

#### Bootstrap steps (Grok)

1. **Human once:** Log into grok.com on hub Chrome profile dedicated to “export bot” (X SSO complete, 2FA stored in credential store).  
2. **Manual export:** Settings → Data Controls / Export Data; download archive; note file layout.  
3. **Parser:** Start from Owlock `easy-grok-chat-exporter` logic for `prod-grok-backend.json`; fork to emit Obsidian frontmatter + vault paths.  
4. **State DB:** SQLite `grok_conversations(id PRIMARY KEY, title, created_at, updated_at, content_hash, md_path, last_export_batch)`.  
5. **Scheduler (nightly 00:30–04:30 Lisbon):**  
   - Prefer **weekly** full export job (Sun) + **nightly** “if raw newer than last import, reprocess”.  
   - Optional: Playwright navigates Settings → Export, waits for email (IMAP on Gmail a@), downloads.  
6. **Token/cookie strategy:**  
   - Export rail: no long-lived API token; session cookies only for browser automation of export button.  
   - Refresh: detect login wall → alert + optional peer OTP relay; do not infinite-loop login.  
7. **Canaries:**  
   - `export_bytes > 0`  
   - `parsed_conversations >= previous_count * 0.95` (warn on shrink)  
   - `new_or_updated_last_14d > 0` if telemetry shows user used Grok  
   - Hash of latest conversation id list changes when UI history length changes  

#### Markdown shape (Grok)

```markdown
---
title: "…"
source: grok
conversation_id: "…"
url: "https://grok.com/c/…"   # if known
created: 2026-07-01T12:00:00Z
updated: 2026-07-15T09:00:00Z
exported_at: 2026-07-21T02:00:00Z
content_hash: "sha256:…"
tags: [ai/grok/export]
---

# Title

## User
…

## Grok
…

<details><summary>thinking</summary>
… optional …
</details>
```

#### Breakage playbook (Grok)

| Symptom | Check | Action |
| --- | --- | --- |
| No email / empty download | Login, spam, retry export | Fall to manual; open incident |
| Schema parse fail | Diff JSON keys vs last good | Pin parser version; quarantine raw |
| Conversation count cliff | User deleted vs silent partial export | Compare to UI screenshot/history length; re-export |
| X SSO expired | Cookie jar | Human re-login on hub profile |
| RE endpoints 401/403 | Auth drift | Disable RE rail; rely on official export |
| Dual history confusion | X vs grok.com | Document which profile is vault-canonical (recommend **grok.com SuperGrok**) |

---

### 4.3 Gemini pipeline (recommended)

```
[A Takeout Gemini Apps] ── every 2 months scheduled + optional weekly manual ──► raw ZIP
        │
        ▼
  parse MyActivity.json / HTML ──► notes (reuse gemini-to-obsidian patterns)
        │
[B Nightly incremental] gemini_webapi list_chats + read_chat
        │ cookie auto-refresh; rate limit ≤1 chat/s; backoff
        │
        ▼
  upsert by cid; prefer fuller body if B text length >> A
        │
[C Fallback] Playwright + AI Chat Exporter / Liyue2341-style
[D Nuclear] Manual Share→Docs for critical threads only
```

#### Bootstrap steps (Gemini)

1. **Takeout schedule:** Create export My Activity → Gemini Apps → JSON → **Export every 2 months for 1 year**.  
2. **Gmail automation:** Watch for Takeout-ready mail → download ZIP to hub → verify size → parse. (Reuse takeout-pull patterns.)  
3. **Converter:** Adapt https://github.com/Coryrichter94/gemini-to-obsidian for vault conventions (frontmatter, `_originals`, reindex).  
4. **Nightly RE (optional but recommended for “ChatGPT parity”):**  
   - Harvest cookies from dedicated Chrome profile once.  
   - Persist cookie path volume.  
   - `list_chats` → for each id not in DB or with newer mtime → `read_chat` → markdown.  
5. **Rate limits:** **[no official public limits found]** — engineer conservative: ≤30–60 chats/hour first week; exponential backoff on 429/empty. **[opinion, medium]**  
6. **Canaries:**  
   - Takeout age < 70 days  
   - Nightly job: `list_chats` non-empty  
   - Random sample of 3 cids: message count vs last run  
   - Alert if RE fails 3 consecutive nights → escalate to Playwright fallback  

#### Markdown shape (Gemini)

Same pattern as Grok with `source: gemini`, `url: https://gemini.google.com/app/…`, tags `ai/gemini/export`.

#### Breakage playbook (Gemini)

| Symptom | Check | Action |
| --- | --- | --- |
| Empty Takeout | Selected “Gemini” not “My Activity → Gemini Apps” | Re-run correct product filter |
| Truncated text vs UI | Compare canary cid lengths | Prefer RE/UI rail for body; keep Takeout for inventory |
| Cookie death | `init()` fails | Fresh private-window cookie harvest; close window after copy |
| RE package breaks on Google deploy | Pin version; watch GitHub issues | Temporary Playwright; watch HanaokaYuzu releases |
| Link expired | 7-day window | Recreate Takeout |
| Workspace vs personal | Side panel chats not persisted | Document non-coverage |

---

### 4.4 Fallback ladder (per vendor)

**Grok:**  
1. Official Settings export (automated browser + email pull)  
2. Privacy portal DSR (slow)  
3. Reverse-engineered session API (if discovered & stable)  
4. Playwright open each history item + DOM extract  
5. Human + userscript  

**Gemini:**  
1. Takeout My Activity → Gemini Apps  
2. `gemini_webapi` list/read  
3. Playwright + extension exporter  
4. Per-chat Docs export for VIP threads  
5. Human  

---

### 4.5 Silent breakage detection (shared implementation sketch)

```python
# Pseudocode — counters written to metrics table + dashboard HTML
metrics = {
  "run_id": ...,
  "vendor": "grok|gemini",
  "rail": "export|takeout|re|ui",
  "raw_bytes": ...,
  "conversations_seen": ...,
  "conversations_written": ...,
  "conversations_skipped_unchanged": ...,
  "parse_errors": ...,
  "auth_ok": True/False,
  "canary_pass": True/False,
}
# Alert if:
# - auth_ok False
# - conversations_seen == 0
# - conversations_written == 0 for 7d while usage_proxy True
# - parse_errors > threshold
# - canary_pass False
```

Emit to existing System Health / `_Dashboards` pattern; exit non-zero on hard fail so night robots surface yellow/red.

---

## 5. Prior art (open source & notable tools, 2025–2026)

### 5.1 Grok-focused

| Name | Approach | Maintenance signal | Reuse vs avoid |
| --- | --- | --- | --- |
| [Owlock/easy-grok-chat-exporter](https://github.com/Owlock/easy-grok-chat-exporter) | Parse official `prod-grok-backend.json` → MD/txt/jsonl + thinking | Small repo; concrete format knowledge | **Reuse parser ideas**; verify on your export |
| [iikoshteruu/enhanced-grok-export](https://github.com/iikoshteruu/enhanced-grok-export) | Userscript multi-format, auto-scroll | Community | Reuse for one-off; not nightly server |
| [revivalstack/ai-chat-exporter](https://github.com/revivalstack/ai-chat-exporter) | Tampermonkey multi-LLM | Active multi-platform | Optional manual |
| [BlackOrchardLabs/de-dobe](https://github.com/BlackOrchardLabs/de-dobe) | Userscript | 2025-11+ | Optional |
| [DREwX-code/omnichat-exporter](https://github.com/DREwX-code/omnichat-exporter) | GreasyFork multi | 2026 | Optional |
| AI Toolbox bulk export | Closed extension | Commercial | **Avoid as sole dependency** (vendor lock, opaque) |
| LLMnesia | Local browse index | Commercial-ish | Not full archive engine |

### 5.2 Gemini-focused

| Name | Approach | Maintenance signal | Reuse vs avoid |
| --- | --- | --- | --- |
| [HanaokaYuzu/Gemini-API](https://github.com/HanaokaYuzu/Gemini-API) | RE async Python; list/read chats; cookie refresh | **Strong** (3.3k★, frequent releases) | **Reuse for nightly** if ToS risk accepted; pin version; AGPL compliance |
| [Coryrichter94/gemini-to-obsidian](https://github.com/Coryrichter94/gemini-to-obsidian) | Takeout → Obsidian MD | Small; Sep 2025+ | **Reuse conversion** for Takeout rail |
| [Liyue2341/gemini-exporter](https://github.com/Liyue2341/gemini-exporter) | Playwright + extension | Small | **Fallback ladder** |
| [sho7650/obsidian-AI-exporter](https://github.com/sho7650/obsidian-AI-exporter) | Extension → Obsidian REST | Multi-AI | Desktop interactive, not hub headless |
| dsdanielpark/Gemini-API | Older RE | Less curated | Prefer HanaokaYuzu |

### 5.3 Cross-cutting

- Multi-exporter userscripts (yyy-OPS Universal AI Chat Exporter, etc.) — good for manual; weak for unattended hub.  
- Chrome Web Store “Export Gemini Chats to Obsidian” — convenient, trust/extension risk.

---

## 6. X / practitioner signal (last ~30 days, retrieved 2026-07-21)

**Method note:** X keyword/semantic search mixed product chatter with unrelated “export” hits. Below are **representative** posts relevant to chat history/export. Separate X-only vs web-corroborated.

### 6.1 Relevant posts

1. **@grok (verified product account)** — 2026-07-15 — Standalone Grok vs X Grok **separate chat histories**, no direct transfer.  
   https://x.com/grok/status/2077189863640576013  
   **Corroboration:** aligns with dual-product architecture; treat as **high value** for pipeline scoping.

2. **@WolfPoli17** (anonymous/unverified) — 2026-07-21 — Single chat history rolled back to weeks-old messages; other chats normal.  
   https://x.com/WolfPoli17/status/2079435799179972796  
   **X-only anecdote** — supports “don’t trust server history without local backup”.

3. **@lalopenguin** — 2026-07-21 — Advice: export data from Grok, Gemini, ChatGPT, Claude settings → ZIP → load into local app.  
   https://x.com/lalopenguin/status/2079367810011234577  
   **X-only; low-authority** — directionally consistent with official export rails.

4. **@MarioNawfal** — 2026-05-06 (slightly outside 30d but high engagement) — Grok history UX (view/share/delete/ghost).  
   https://x.com/MarioNawfal/status/2051957645716676974  
   **Product marketing**, not export API.

5. **@GeminiApp (verified)** — 2026-03-26 — Chat history **import into Gemini** from other AI ZIP exports (competitive switcher).  
   https://x.com/GeminiApp/status/2037247068025315591  
   **Interesting reverse:** Google wants imports *in*; still weak on first-class *out* beyond Takeout/share.

6. **@0xSero** — 2026-03-14 — “Own a copy” via Takeout then RAG.  
   https://x.com/0xSero/status/2032738207100490001  
   **Web-corroborated pattern** (Takeout).

7. **@BrooksWhaleX** — 2026-07-20 — How to delete Gemini history via My Activity (privacy, not backup).  
   https://x.com/BrooksWhaleX/status/2079327411704140063  

**Finding rest only on X:** dual Grok history separation (item 1 is product-account but still X). Rollback bugs (item 2).  
**Corroborated independently on web:** Takeout for Gemini; official xAI download language in FAQ; lack of ChatGPT-class exporters in mature FOSS for Grok RE API.

### 6.2 Contrarian / dissenting takes

- **“Don’t build scrapers—use platform export + local importer apps”** (lalopenguin; AI Chat Importer blog; LLMnesia). Valid if export cadence ≤ weekly is acceptable.  
- **“Takeout is enough / Takeout is trash”** — both exist (UMich/Univik vs Liyue2341). **Do not average them—instrument canaries.**  
- **“Just use the API”** — **reject for history retrieval** (both vendors). API ≠ consumer archive.  
- **“Browser extension is the product”** — fine for interactive; **poor** for always-on hub reliability and security.

---

## 7. Concrete recommendation for *this* fleet

| Priority | Grok | Gemini |
| --- | --- | --- |
| P0 (ship first) | Automate official export → parse → Obsidian MD + state.db | Automate Takeout pull (2-month schedule + Gmail) → gemini-to-obsidian-style converter |
| P1 (nightly parity) | Optional Playwright “export if last_export_age > 6d” | `gemini_webapi` list/read incremental |
| P2 | RE backend if Network-tab yields stable endpoints | Playwright exporter fallback |
| Explicit non-goals | xAI API for history; X archive as sole source | Gemini API key for history; scraping all Google products |

**Opinion (confidence: high):** Matching ChatGPT’s *reliability* is achievable; matching ChatGPT’s *elegance of one bearer+REST rail* is **not** currently available for either vendor without reverse engineering. Budget engineering for **multi-rail orchestration** and **detectors**.

---

## 8. Open questions / [no source found]

1. Official xAI rate limits on Data Controls export frequency.  
2. Authoritative JSON schema for Grok export (field list, attachment inclusion).  
3. Whether SuperGrok Heavy / voice / image assets appear in export.  
4. Stable private REST paths for grok.com conversation list (no public RE client found equivalent to gemini_webapi).  
5. Quantitative Takeout truncation rates vs live Gemini UI.  
6. Documented enforcement actions against personal chat exporters for either vendor.  
7. Exact cookie names/TTL for grok.com session after X SSO.

---

## 9. Full source list

### Official / primary

1. xAI Consumer FAQs — https://x.ai/legal/faq — May 12, 2025  
2. xAI privacy portal (as linked from FAQ) — https://x.ai/privacy-portal/  
3. X Help — About Grok — https://help.x.com/en/using-x/about-grok  
4. Google Support — Export responses from Gemini Apps — https://support.google.com/gemini/answer/14184041  
5. Google Takeout — https://takeout.google.com  
6. Google My Activity — https://myactivity.google.com  
7. xAI developer docs (Chat Completions / Responses) — https://docs.x.ai/developers/model-capabilities/text/generate-text ; https://docs.x.ai/developers/model-capabilities/legacy/chat-completions ; https://docs.x.ai/developers/rest-api-reference/inference/chat  
8. Google AI Gemini API reference — https://ai.google.dev/api  

### Guides / institutional

9. U-M TeamDynamix — Export Gemini Chat History — https://teamdynamix.umich.edu/TDClient/30/Portal/KB/Article/15439/Google-Export-Gemini-Chat-History  
10. Univik — How to Export All Google Gemini Chats — https://univik.com/help/export-gemini-chats.html — Mar 2026 verification claim on page  
11. Lifewire — Google Takeout (frequency option) — https://www.lifewire.com/what-is-google-takeout-4173795  
12. MakeUseOf — Google Takeout walkthrough — https://www.makeuseof.com/how-to-see-what-google-knows-about-you/ — Aug 19, 2025  
13. LLMnesia — Export Grok Chat History — https://www.llmnesia.com/blog/how-to-export-grok-conversation-history — Apr 19, 2026  
14. AI Chat Importer — How to Export Grok Conversations — https://ai-chat-importer.com/blog/how-to-export-grok-conversations — Apr 12, 2026  
15. ChatExport.guide — Grok — https://chatexport.guide/guides/grok/ — Jan 3, 2026  
16. Grokipedia — Exporting Grok chats — https://grokipedia.com/page/Exporting_Grok_chats  

### Open source

17. Owlock/easy-grok-chat-exporter — https://github.com/Owlock/easy-grok-chat-exporter  
18. HanaokaYuzu/Gemini-API — https://github.com/HanaokaYuzu/Gemini-API  
19. Coryrichter94/gemini-to-obsidian — https://github.com/Coryrichter94/gemini-to-obsidian  
20. Liyue2341/gemini-exporter — https://github.com/Liyue2341/gemini-exporter  
21. revivalstack/ai-chat-exporter — https://github.com/revivalstack/ai-chat-exporter  
22. iikoshteruu/enhanced-grok-export — https://github.com/iikoshteruu/enhanced-grok-export  
23. BlackOrchardLabs/de-dobe — https://github.com/BlackOrchardLabs/de-dobe  
24. sho7650/obsidian-AI-exporter — https://github.com/sho7650/obsidian-AI-exporter  
25. dsdanielpark/Gemini-API — https://github.com/dsdanielpark/Gemini-API  
26. DREwX-code/omnichat-exporter — https://github.com/DREwX-code/omnichat-exporter  

### Community / support threads

27. Reddit r/GoogleGeminiAI export discussion — https://www.reddit.com/r/GoogleGeminiAI/comments/1l8qh84/is_there_a_way_to_export_a_whole_gemini_chat_to/  
28. Reddit r/grok bookmarklet export — https://www.reddit.com/r/grok/comments/1jl6k2x/is_there_a_way_to_export_a_chat_create_a_new_chat/  
29. Google Gemini Community — download entire history — https://support.google.com/gemini/thread/410548499/how-do-i-download-my-entire-conversation-history-with-gemini  
30. Google Gemini Community — export entire conversation — https://support.google.com/gemini/thread/418243882/how-to-export-entire-gemini-conversation  

### Commercial / marketing (low-authority for architecture claims)

31. AI Toolbox Grok bulk export page — https://www.ai-toolbox.co/ai-toolbox-grok-features/grok-bulk-export-delete-2026 — Jul 8, 2026  
32. YourAIScroll Grok history blog — https://www.youraiscroll.com/blog/solve-grok-chat-history-problems — Mar 7, 2025  

### X posts cited

33. https://x.com/grok/status/2077189863640576013 — 2026-07-15  
34. https://x.com/WolfPoli17/status/2079435799179972796 — 2026-07-21  
35. https://x.com/lalopenguin/status/2079367810011234577 — 2026-07-21  
36. https://x.com/MarioNawfal/status/2051957645716676974 — 2026-05-06  
37. https://x.com/GeminiApp/status/2037247068025315591 — 2026-03-26  
38. https://x.com/0xSero/status/2032738207100490001 — 2026-03-14  
39. https://x.com/BrooksWhaleX/status/2079327411704140063 — 2026-07-20  

---

## 10. Confidence board (key findings)

| Finding | Tier |
| --- | --- |
| xAI FAQ documents download via Settings/Data Controls + privacy portal | **[established]** |
| xAI API does not expose consumer grok.com chat archive | **[established]** |
| Gemini consumer history bulk path is Takeout My Activity → Gemini Apps | **[established]** |
| Takeout can schedule every 2 months for 1 year | **[established]** |
| Official Gemini API does not list gemini.google.com threads | **[established]** |
| Grok vs X Grok histories can be separate | **[emerging]** |
| Exact Grok export file layout (`prod-grok-backend.json`) | **[emerging]** / **[single-source]** community |
| Takeout systematically truncates full chat text | **[speculative]** disputed |
| Safe personal-scale RE without enforcement | **[speculative]** |
| Stable public RE client for Grok history like gemini_webapi | **[fringe]** — not found |

---

*End of dossier DR26-07-15-HUB-01. Research pass completed 2026-07-21. Do not invent additional endpoints; re-harvest Network tab before coding RE rails.*
