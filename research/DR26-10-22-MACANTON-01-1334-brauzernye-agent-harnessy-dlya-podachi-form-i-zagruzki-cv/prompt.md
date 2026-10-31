# DR26-10-22-MACANTON-01-1334 — the exact prompt sent to every model

# DR26-10-22-MACANTON-01-1334 — Агент-харнессы, которые жмут кнопки за Антона в браузере (формы, загрузка CV, Submit)

Deep research, October 2026. Use web search and cite every claim with a URL and date. Maximum reasoning.

Who asks: a CTO/COO in Palo Alto who must fill and submit many online forms himself (job applications, fellowship forms, Airtable / Google Forms / Ashby / Greenhouse portals, file uploads such as a CV PDF, dropdowns, multi-page wizards) and cannot spend his own time clicking. Today his main agent (Claude Code + Claude-in-Chrome) filled the form fields but was blocked from uploading a local CV file by its own auto-mode safety classifier, and it will not click Submit or write essays for him.

Question: which AGENT HARNESSES / computer-use / browser-agent products available right now (October 2026) can reliably operate a real logged-in browser on his behalf — fill forms, upload local files, select dropdowns, press Submit after a one-time approval — with the fewest false refusals and the least babysitting?

Cover at least: OpenAI ChatGPT Agent / Operator / Atlas browser; Anthropic Claude for Chrome and Claude Code computer use (incl. how to configure permissions so file_upload and submit are pre-approved); Google Gemini agent mode / Project Mariner / Chrome Gemini; Perplexity Comet; Microsoft Copilot Actions / Edge; Manus; Genspark; Fellou; Opera Neon; Browser Use (cloud and open source); Skyvern; Stagehand/Browserbase; Agent-E, OpenHands browsing; Amazon Nova Act; any 2026 newcomer that matters.

For each one, a table row with:
1. Can it control the user's own logged-in browser profile (cookies, Google SSO) or only a cloud sandbox?
2. Local file upload from the user's disk: yes/no, how.
3. Submit / irreversible action policy: always asks, can be pre-approved per site, or autonomous. Exact setting name.
4. CAPTCHA behaviour (it is acceptable that it stops for CAPTCHA).
5. Measured reliability: WebArena, OSWorld, Online-Mind2Web, BrowseComp-agent or real user reports with numbers and dates.
6. Price and plan needed; availability in the US.
7. Known failure modes ("refuses", "hangs", "loses form state on navigation").
8. Can it be driven from another agent (API/MCP/CLI) so an orchestrator can hand it a task?

Then: (a) top-3 recommendation for "fill and submit job/fellowship applications from a profile + CV, human approves once", with a concrete setup recipe for each; (b) how to unblock Claude Code's auto-mode classifier for file_upload on specific domains (settings.json permissions allow-list, permission modes) with official doc links; (c) honest limits: several programs (Successif, Generator, Pivotal) forbid AI-written answers and run AI-text detectors (Pangram) — which harnesses support a "voice-dictation → verbatim paste" workflow so the human's own spoken words go into fields without AI rewriting.

Output: comparison table, top-3 with recipes, unblock recipe, sources list. No marketing language.

## Материалы заказа (MOC)

- Синтез четырёх отчётов
