# DR26-07-04-HUB-06 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "API Integration Setup Guide"
type: ai-conversation
source: chatgpt

authored_by: hybrid
conversation_id: 6a48c934-06c8-83eb-8514-8ef0fe228877
model: "gpt-5-5-pro"
gizmo_id: g-p-683da1839c048191a38b1e61c56d4fc3
url: "https://приватный чат"
date_recorded: 2026-07-04
date_added: 2026-07-07
valid_as_of: 2026-07-04
archived: false
volatility: slow
interpretation_confidence: high
intent: ""
language: ru
tags: [chatgpt, ai-conversation]
value_score: 0.0
msg_count: 3
related_concepts: []
revisit_if: ""
---

# API Integration Setup Guide

> ChatGPT · 2026-07-04 · `gpt-5-5-pro` · 3 сообщений

## Диалог

**Anton:**

DR26-07-04-HUB-06



ROLE: You are a senior API-integrations engineer. Give me EXACT, CURRENT (July 2026)

step-by-step setup flows for two APIs, and precisely classify each step as

"automatable by a logged-in browser agent acting for the account owner" vs

"strictly requires the human owner in person" (2FA, payment, consent screens).

Cite official docs where possible; flag anything that changed in 2025-2026.



CONTEXT: A solo founder wants a small assistant to (1) auto-post ONE tweet/day (a

short teaser WITH a link) to his X account, and (2) READ his own recent Facebook

posts to detect ones lacking a teaser. He is logged into both accounts on his machine.

He will NOT post to Facebook (read-only). Budget-sensitive; wants the cheapest legal path.



ANSWER THESE, each as an ordered checklist:



A) X (Twitter) API v2 — posting one tweet/day with a link:

   1. Exact steps to create a developer account at developer.x.com in July 2026.

   2. Is a PAYMENT METHOD required BEFORE you can (a) create an app, (b) generate

      API Key/Secret + Access Token/Secret, or (c) only before the first POST?

   3. Exact steps to generate the 4 OAuth 1.0a credentials WITH read+write permission.

   4. Current pay-per-use pricing for POST /2/tweets with a link; any monthly minimum;

      any monthly free write allowance still existing.

   5. Which steps strictly need the human owner (email/phone verify, card + 3-D Secure)?

   6. Any 2026 gotchas (verification, rate caps, app-suspension triggers for automation).



B) Facebook Graph API — reading the owner's OWN recent posts (personal profile):

   1. In July 2026, does GET /me/posts (or /me/feed) still return a PERSONAL profile's

      own authored posts? Or has Meta deprecated personal-profile reading so it only

      works for Pages? If dead, what is the official supported alternative to read

      one's own personal-profile posts programmatically (if any)?

   2. Exact steps to create a Meta app + get a User access token with `user_posts`

      (or the current equivalent permission) as the app OWNER in development mode.

   3. Does the app owner reading their OWN posts require App Review, or does dev-mode

      + owner token bypass review? Confirm for July 2026.

   4. Long-lived user token: how to exchange, exact lifetime, refresh procedure.

   5. Which steps strictly need the human owner (login, 2FA, consent dialog)?

   6. Any 2026 deprecations affecting this (API version, permission renames, removals).



OUTPUT: two checklists (A, B), each step tagged [AGENT-CAN-DO] or [OWNER-ONLY],

plus a final verdict: "For each platform, what % of setup can a browser agent

complete without the owner, and what is the single owner-only blocker?"

Include a short "what changed in 2025-2026" note per platform.@Deep research

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"prompt":"Research current (July 2026) setup flows for X API v2 posting and Facebook Graph API reading personal profile posts, including pricing, permissions, owner-only vs browser-agent steps, official documentation, and 2025-2026 changes. Produce detailed report."}}
```

---

**ChatGPT:** · `gpt-5-5-instant`

```
{"path":"/Deep Research App/start","args":{"user_query":"Research current July 2026 X API v2 setup for posting tweets and Meta Graph API setup for reading a user's own Facebook posts, including pricing, permissions, owner-only steps, browser-agent automatable steps, and 2025-2026 changes with official documentation."}}
```
