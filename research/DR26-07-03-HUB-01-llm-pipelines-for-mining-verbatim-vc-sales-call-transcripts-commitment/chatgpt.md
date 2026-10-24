# DR26-07-03-HUB-01 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "LLM Sales Call Mining"
type: ai-conversation
stage: raw
source: chatgpt


authored_by: hybrid
conversation_id: 6a48be2e-fd24-83ed-a04a-78ce3ce213de
model: "gpt-5-5-pro"
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
msg_count: 24
related_concepts: []
revisit_if: ""
---

# LLM Sales Call Mining

> ChatGPT · 2026-07-04 · `gpt-5-5-pro` · 24 сообщений

## Диалог

**Anton:**

DR26-07-03-HUB-01

Conduct a deep research investigation on the following topic.

CONTEXT (already known - dig deeper, don't repeat):
- We already extract ALL call data from Granola daily via its official API: verbatim transcripts (diarized only as microphone-vs-speaker, no named speakers), plus AI summaries we deliberately distrust. Corpus: ~253 calls / 1.2M words, growing ~2-6 calls/day. VC/startup deal-flow calls (RU+EN mixed), stored as markdown in an Obsidian vault + SQLite CRM.
- We have working infra: cheap-detector->LLM-judge alpha mining, lead CRM with promise/follow-up cards, nightly batch jobs, local GPU for embeddings/Whisper, model ladder (deterministic -> small model -> big model).
- The open question is NOT extraction. It is the DISTILLATION layer: how to reliably turn verbatim call transcripts into (a) structured commitments/agreements with owners and due dates, (b) lead/CRM facts, (c) cross-call patterns (how our calls actually go, what pitches land, which promises rot), (d) "alpha" - non-obvious signals worth money.

TOPIC:
Best practices (mid-2026) for LLM pipelines that mine large corpora of verbatim sales/VC call transcripts: per-call structured extraction, cross-call pattern mining, commitment tracking, and quality control against hallucinated attributions.

OBJECTIVE:
Find the best current practices, approaches, failures, trade-offs and frameworks for:
1. Per-call extraction: schemas that work for commitments/decisions/facts (who-what-when), prompt patterns for long diarized-but-unnamed transcripts, chunking vs full-context, RU/EN mixed speech, mapping "microphone vs speaker" audio channels to named participants using calendar metadata.
2. Cross-call analytics: methods for pattern mining over hundreds of calls (talk-ratio, objection taxonomies, winning pitch patterns, promise-completion rates) - both classic (embeddings+clustering, BERTopic-style) and LLM-map-reduce; what revenue-intelligence products (Gong, Chorus, Attention, Fathom teams) actually compute and which of those metrics are worth cloning in-house.
3. Commitment/promise tracking: systems that detect commitments in conversation and track them to completion; false-positive rates; how to ground each claim with a transcript quote to kill hallucinated attributions.
4. Failure modes: known pitfalls of transcript mining (ASR errors compounding into wrong facts, speaker misattribution, summary drift, over-extraction noise) and mitigations.
5. Cost/architecture: batch-nightly vs streaming, small-model-first ladders, caching, when fine-tuned/local models beat API calls for this class of task.

RESEARCH REQUIREMENTS:
1. Academic research - papers, meta analyses, benchmarks (meeting summarization/action-item extraction: AMI, ICSI, QMSum lineage and what superseded them)
2. Industry practice - revenue-intelligence vendors, startups, open source projects
3. Competitive landscape - what Gong/Attention/Fireflies/Fathom compute; open-source equivalents
4. Failure analysis - failed attempts, abandoned projects, lessons learned
5. Emerging trends - last 12 months
6. Implementation recommendations - actionable framework, roadmap, priorities for our stack (nightly batch, Sonnet-grunt/Opus-judge ladder, Obsidian+SQLite home)

OUTPUT FORMAT:
A. Executive Summary
B. Key Findings
C. Contradictions & Debates
D. Best Practices
E. Failure Modes
F. Strategic Recommendations
G. Action Plan
H. Sources

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"2025 2026 LLM meeting summarization action item extraction benchmark QMSum AMI ICSI transcript hallucination attribution"},{"q":"LLM action item extraction meeting transcripts hallucinated attribution grounded quotes benchmark 2025"},{"q":"meeting summarization benchmarks AMI ICSI QMSum MeetingBank 2025 large language models"},{"q":"sales call transcript LLM extraction commitments action items revenue intelligence metrics Gong Chorus 2025"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Gong conversation intelligence features talk ratio topics trackers deal warnings action items official"},{"q":"Gong trackers mentions talk ratio ask questions customer monologue official docs"},{"q":"ZoomInfo Chorus conversation intelligence features trackers talk ratio action items official"},{"q":"Attention AI sales call AI action items CRM updates sales methodology official features"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

I’ll treat this as a current-practice research memo for an already-built Granola ingestion system, triangulating academic benchmarks, vendor docs, OSS tooling, and failure-mode literature into concrete Obsidian+SQLite nightly-batch recommendations.

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Fathom Teams official AI meeting assistant action items CRM sync call coaching features"},{"q":"Fireflies.ai official meeting intelligence features action items topic trackers sentiment analytics"},{"q":"Avoma official conversation intelligence features talk ratio objection action items CRM"},{"q":"Otter.ai sales meeting action items CRM insights official 2026"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"site:aclanthology.org meeting summarization hallucination factuality MESA SCOPE FRAME 2025"},{"q":"FAME meeting summarization benchmark 2025 English German meeting transcripts LLM hallucination"},{"q":"FaithBench hallucination benchmark summarization 2025 LLM"},{"q":"meeting summarization evaluation metrics human correlation 2024 2025 Kirstein"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"AMI meeting corpus official 100 hours transcripts summaries action items"},{"q":"ICSI meeting corpus official 75 meetings transcripts annotations"},{"q":"MeetingBank benchmark dataset meeting summarization 1366 meetings city councils paper"},{"q":"QMSum benchmark query-based multi-domain meeting summarization 1808 query-summary pairs 232 meetings official"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"action item detection meeting transcript dataset LLM 2024 2025"},{"q":"meeting action item extraction dataset benchmark owner due date commitment detection"},{"q":"detect commitments promises in dialogue transcript action item extraction LLM grounded citation 2025"},{"q":"commitment tracking conversation transcripts action items owner due date extraction LLM"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"speaker diarization errors transcript mining meeting summarization ASR errors speaker attribution LLM 2025"},{"q":"LLM diarized transcripts speaker attribution errors meeting action items owner extraction"},{"q":"WhisperX pyannote speaker diarization word level timestamps multilingual code switching 2025"},{"q":"mapping diarized speaker labels to names using calendar metadata meeting transcripts"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"code switching multilingual transcripts LLM extraction benchmark Russian English ASR meeting transcription"},{"q":"code-switched speech recognition Russian English Whisper hallucination transcript processing 2025"},{"q":"multilingual meeting summarization code-switched transcripts LLM 2025"},{"q":"LLM structured extraction code mixed meeting transcripts action items 2025"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Anthropic Claude citations documents API structured outputs tool use JSON mode 2026 official"},{"q":"Anthropic Claude prompt engineering long context citations grounding official docs 2026"},{"q":"OpenAI structured outputs JSON schema strict function calling official docs 2026"},{"q":"Instructor BAML structured extraction LLM validation JSON schema 2025"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"LLM long context lost in the middle 2023 position bias paper"},{"q":"RAG evaluation grounded citation hallucination claim extraction LLM 2025 benchmark RAGTruth"},{"q":"large language models retrieval augmented generation citation faithfulness benchmark 2025 attribution"},{"q":"LLM extraction grounded citations source span verification framework 2025"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"conversation intelligence knowledge graph CRM call transcripts LLM 2025 open source"},{"q":"LLM knowledge graph extraction from transcripts citations source spans 2025 2026"},{"q":"grounded knowledge graph extraction LLM source spans 2026"},{"q":"entity resolution CRM facts from meeting transcripts LLM pipeline source citations"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn459144search1"},{"ref_id":"turn459144search9"},{"ref_id":"turn459144search29"},{"ref_id":"turn997357search1"},{"ref_id":"turn997357search9"},{"ref_id":"turn997357search0"},{"ref_id":"turn997357search7"},{"ref_id":"turn997357search13"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn459144search3"},{"ref_id":"turn459144search8"},{"ref_id":"turn997357search21"},{"ref_id":"turn997357search22"},{"ref_id":"turn459144search18"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn119885search3"},{"ref_id":"turn119885search10"},{"ref_id":"turn356529search3"},{"ref_id":"turn356529search9"},{"ref_id":"turn356529search23"},{"ref_id":"turn356529search2"},{"ref_id":"turn533982search24"},{"ref_id":"turn533982search4"},{"ref_id":"turn700857search0"},{"ref_id":"turn700857search5"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"https://arxiv.org/html/2303.16763"},{"ref_id":"turn276899view2","lineno":1}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn547891search16"},{"ref_id":"turn731364search5"},{"ref_id":"turn834215search1"},{"ref_id":"turn834215search0"},{"ref_id":"turn607490search6"},{"ref_id":"turn607490search26"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"BERTopic topic modeling official documentation c-TF-IDF embeddings clustering 2026"},{"q":"topic modeling call transcripts embeddings clustering BERTopic LLM labeling 2025"},{"q":"LLM map reduce clustering qualitative analysis transcripts 2025 topic modeling"},{"q":"large scale qualitative analysis interview transcripts LLM thematic analysis 2025 benchmark"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"2026 AI meeting agents real-time coaching action items trend sales calls last 12 months"},{"q":"2026 conversation intelligence agents CRM updates follow ups live coaching recent trends"},{"q":"2025 2026 LLM sales call analysis real-time agent CRM automation"},{"q":"2026 meeting AI agents workflow automation CRM action items product launch"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"search_query":[{"q":"Anthropic prompt caching official docs cost latency 2026"},{"q":"OpenAI prompt caching official docs 2026 cached tokens cost"},{"q":"vLLM offline inference batch processing structured outputs 2026 docs"},{"q":"llama.cpp grammar constrained decoding JSON schema structured outputs official"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

```
{"open":[{"ref_id":"turn416299search0"},{"ref_id":"turn416299search2"},{"ref_id":"turn393916search0"},{"ref_id":"turn393916search1"},{"ref_id":"turn393916search2"},{"ref_id":"turn393916search13"}],"response_length":"medium"}
```

---

**ChatGPT:** · `gpt-5-5-pro`

I’ve gathered the core evidence; public vendors converge on a narrow metrics set, while commitment attribution and false-positive control remain under-benchmarked despite heavy marketing.

---

**ChatGPT:** · `gpt-5-5-pro`

# A. Executive Summary

The right architecture is **not “meeting summarization.”** It is a **grounded claim extraction and evidence-tracking system** where every downstream object—commitment, CRM fact, objection, pattern, alpha hypothesis—points back to exact transcript evidence.

For your corpus and stack, the winning design is:

**Transcript → numbered evidence spans → candidate extraction → structured claim records → independent grounding judge → CRM/Obsidian materialization → cross-call analytics.**

The central rule should be brutal:

> **No transcript quote / turn ID / speaker-channel evidence = no fact.**

That single rule kills most hallucinated attribution, overconfident summaries, and “AI said they promised X” garbage.

Current academic meeting-summarization research still revolves around AMI, ICSI, QMSum, MeetingBank, and newer synthetic/multilingual datasets, but those benchmarks only partially match your use case. AMI is about 100 hours, much of it scenario-based design-team meetings, while QMSum has 232 meetings and 1,808 query-summary pairs—not deal-flow calls with mixed RU/EN, weak diarization, CRM enrichment, and monetary alpha extraction. citeturn550510view1turn550510view0 MeetingBank explicitly notes that older meeting corpora like ICSI and AMI are small, and that useful meeting summaries need to capture **issues, decisions, and actions** while filtering irrelevant discussion. citeturn937071view2

Industry revenue-intelligence vendors have converged on a common feature set: call summaries, action items, CRM auto-fill, talk/listen ratio, longest monologue, interactivity, topic/keyword trackers, objections, competitor mentions, coaching scorecards, follow-up drafts, call libraries, and “deal risk” views. Gong, Clari, Fireflies, Fathom, and Attention all expose variants of these. citeturn315750view0turn315750view1turn315750view2turn315750view5turn315750view8turn315750view9turn865599view1 The parts worth cloning are **not** generic summaries or sentiment dashboards; they are the grounded operational primitives: **next steps, explicit commitments, objections, competitor/product mentions, talk dynamics, pitch modules, promise decay, and CRM field updates with evidence.**

The strategic recommendation:

1. **Use full-call context for final adjudication, but not as the only extraction pass.** Run cheap high-recall candidate mining over chunks/windows, then run a stronger model over evidence windows or full call to validate, normalize, and dedupe.
2. **Store claims, not summaries.** A summary is a view. The database primitive should be a quote-grounded atomic claim.
3. **Separate “channel speaker” from “named person.”** Granola’s microphone-vs-speaker diarization is not enough to assign names unless calendar metadata, attendee count, self-intros, voice identity, or explicit transcript evidence supports it.
4. **Build your own commitment ontology.** Distinguish hard commitments from suggestions, intentions, agenda items, requests, dependencies, tentative offers, and superseded promises.
5. **Use LLMs as judges, not oracles.** Every extracted item gets checked by an independent grounding pass against its cited quote.
6. **Measure extraction quality like a retrieval system:** precision, recall, quote-support rate, owner accuracy, due-date accuracy, duplicate rate, unsupported-claim rate, and reviewer correction rate.
7. **Run nightly batch, not streaming.** Streaming only matters for real-time coaching; your value is after-call distillation, cross-call analytics, and alpha detection.

---

# B. Key Findings

## 1. Academic work is useful, but its default target is too weak for your use case

The academic lineage gives you useful building blocks:

| Area | What it contributes | Limitation for your use case |
|---|---:|---:|
| AMI / ICSI | Long meeting transcripts, speaker turns, dialogue acts, summarization tasks | Mostly research/meeting settings, not VC/sales, not enough named-owner commitment tracking |
| QMSum | Query-focused summarization over long meetings | Good for retrieval-style “answer this question from meeting,” weak for CRM event extraction |
| MeetingBank | Public-meeting summarization, long transcript-summary pairs | More civic-meeting than deal-flow; not commitment lifecycle tracking |
| Action-item extraction papers | Commitment definition, action-item ambiguity, class imbalance | Often small, domain-specific, not production sales/VC |
| Recent LLM meeting-summary papers | Hallucination/error taxonomies, fact extraction, refinement | Mostly summary quality, not event-sourced CRM facts |

QMSum is especially relevant because it frames meeting summarization as **query-based locate-then-summarize** rather than generic summarization. That is closer to your system: “find commitments,” “find objections,” “find investable alpha,” “find lead facts.” QMSum contains 1,808 query-summary pairs over 232 meetings and emphasizes that long meeting context must first be localized before summarization. citeturn550510view0

The newer Re-FRAME work points in the same direction: LLM meeting summaries remain error-prone because of hallucinations, omissions, and irrelevant content; its proposed method extracts and scores salient facts before organizing them thematically. citeturn550510view4 That is exactly the mental model you want: **facts first, prose later.**

The recent “What’s Wrong?” paper also matters because it treats meeting summary improvement as **mistake identification + refinement**, with error categories such as hallucination, omission, relevance, informativeness, concision, and coherence. It reports that even GPT-4 Turbo performed worse on hallucination and partial-omission detection than on easier error categories. citeturn371460view2 This supports a practical rule: do not trust a single LLM pass to both extract and self-verify.

## 2. Action-item extraction is deceptively hard

The action-item literature is directly relevant to commitment tracking. A 2024 action-item detection paper states that action items in meeting transcripts are crucial, but manually annotated datasets are scarce and small. citeturn371460view0 The code-mixed Urdu-English action-item work is useful despite the language mismatch because it formalizes ambiguity: general instructions, future discussion, or vague task talk should **not** automatically count as action items; explicit assignment, agreement, owner, and timeframe matter. citeturn937071view1

This maps cleanly to your VC/sales calls:

- “We should probably send them the deck” = weak intention, not a hard commitment.
- “I’ll send you the deck tomorrow” = commitment.
- “Can you send the SAFE docs?” = request, not completion or acceptance.
- “Yes, I’ll send them after the call” = accepted commitment.
- “Let’s discuss next week” = proposed next step unless both sides agree.
- “Actually, let’s push it to Friday” = superseding commitment.

The implication: your schema needs **commitment strength**, not a boolean `is_action_item`.

## 3. Speaker attribution is a first-class risk, not a cosmetic issue

Your Granola diarization is microphone-vs-speaker, not named speakers. That means most “Anton promised X” vs “Founder promised X” errors will come from **speaker mapping**, not extraction.

Modern diarization still has nontrivial failure modes. Pyannote’s benchmark documentation defines diarization error as a combination of speaker confusion, missed speech including overlap, and false alarms. citeturn558049view0 A 2025 diarization benchmark notes that diarization remains unsolved, with errors propagating into downstream analysis; primary errors include missed speech and speaker confusion, especially with higher speaker counts. citeturn558049view4

WhisperX and pyannote are strong open-source components for local ASR/alignment/diarization, but they do not magically solve named speaker identity. WhisperX supports word-level timestamps and pyannote-based diarization; its README also notes that some platforms can diarize via separate audio streams or platform speaker data to get actual speaker names. citeturn558049view1 Pyannote’s open model documentation says multichannel audio is downmixed to mono by default, which matters if you were hoping to recover channel identity from audio after the fact. citeturn558049view2

For your setup, the clean abstraction is:

```text
speaker_channel != person_id
```

Treat named attribution as probabilistic and evidence-backed.

## 4. Vendor metrics have commoditized; evidence-backed workflow is the moat

Gong exposes AI-generated summaries, CRM updates, follow-ups, dashboards, scorecards, and coaching recommendations. citeturn315750view0 Gong “trackers” identify words, phrases, and concepts across calls/emails, including pricing, discovery questions, objections, and product messaging; “smart trackers” operate on AI-recognized concepts, not only literal keywords. citeturn315750view1 Gong also reports talk/listen ratio, longest monologue, customer story length, and interactivity; its help docs describe a “highest converting” talk/listen ratio around 43:57 and recommend keeping longest monologues below roughly 2:30. citeturn315750view2

But Gong’s own 2025 analysis adds nuance: average calls were about 60% seller talk / 40% listening; closed-won calls had about 57% seller talk and lost calls about 62%, with consistency being more important than a magic talk-ratio target. citeturn315750view3 So: clone the metric, not the mythology.

Fireflies exposes speaker talk time, sentiment, topic trackers, AI filters, CRM auto-fill, and automatic tasks. citeturn315750view4turn315750view5turn315750view6 Fathom exposes notes, insights, action items, CRM sync, custom summaries, conversational assistant, deal views, coaching metrics, and AI scorecards. citeturn315750view7turn315750view8 Attention’s positioning is broader “AI Sales Agents” that send transcripts, key moments, top calls, coaching scorecards, and conversation-derived memory into other systems. citeturn315750view9 Clari’s conversation-intelligence page lists customer pain points, needs, objections, competitor sentiment, script/pitch effectiveness, rep performance, lead quality, talk/listen ratio, call duration, key moments, battle cards, scorecards, and deal central. citeturn865599view1

Public vendors are converging on the same primitives. Your advantage is not another dashboard. It is **private corpus + better ontology + evidence-grounded VC-specific alpha extraction.**

## 5. Long context helps, but does not remove the need for extraction architecture

Current frontier APIs support structured outputs, long context, batch processing, prompt caching, and schema-constrained JSON. Anthropic’s structured-output docs emphasize schema-constrained responses to prevent malformed JSON, missing fields, and inconsistent types. citeturn891053view0 OpenAI’s docs make the same distinction: JSON mode gives valid JSON, but Structured Outputs are designed for schema adherence. citeturn515745view5 Gemini’s long-context documentation describes 1M-token context windows and explicitly frames long context as reducing the need to drop or summarize prior content. citeturn515745view6

But long context is not a substitute for evidence design. It is a convenience for final adjudication and cross-reference, not a reason to run one giant “summarize this call” prompt.

For your corpus, use:

```text
cheap high-recall candidate detector
        ↓
strong model structured extractor
        ↓
independent grounding judge
        ↓
event-sourced claim DB
```

That is much more robust than:

```text
full transcript → one big summary → parse summary
```

---

# C. Contradictions & Debates

## Debate 1: Full-context extraction vs chunked extraction

**Full-context argument:**  
A whole-call pass can resolve late corrections, speaker references, context shifts, and whether a commitment was superseded.

**Chunking argument:**  
Chunking improves recall, reduces token cost, and avoids long-context attention failure. It also lets you run local/small models cheaply over every window.

**Best answer for your stack:**  
Use both.

- Run **chunk/window candidate detection** for high recall.
- Run **full-call or evidence-window adjudication** for final records.
- Store only claims that pass quote-grounding and schema validation.

Practical design:

```text
Window size: 60–120 transcript turns or ~1,500–3,000 tokens
Overlap: 10–20 turns
Candidate types: commitment, decision, lead_fact, objection, competitor, technical_signal, alpha_signal
Final judge context: candidate quote ± 20 turns + call metadata + optionally full transcript
```

## Debate 2: Generic summaries vs structured claim extraction

Generic summaries are useful for humans, but they are poor as machine inputs. Meeting-recap research explicitly notes that summaries alone are often not enough because meeting goals differ and fixed-length summaries can be too verbose or too concise. citeturn550510view3

For CRM, promise tracking, and alpha, a summary is lossy compression. Use summaries only as generated views over a claim table.

Bad primitive:

```json
{
  "summary": "The founder said they would send metrics next week."
}
```

Good primitive:

```json
{
  "claim_type": "commitment",
  "normalized_claim": "Founder committed to send updated metrics.",
  "speaker_channel": "speaker",
  "owner_resolution": {
    "person_id": null,
    "party": "external",
    "confidence": 0.72,
    "basis": ["calendar_has_single_external_attendee", "speaker_channel_external_prior"]
  },
  "due_date": null,
  "due_text": "next week",
  "evidence": {
    "call_id": "DR26-07-03-HUB-01",
    "turn_ids": ["t184", "t185"],
    "quote": "Yeah, I'll send you the updated metrics next week."
  },
  "support_status": "quote_supported",
  "confidence": 0.86
}
```

## Debate 3: LLM-as-extractor vs LLM-as-judge

A single extractor will hallucinate, over-normalize, or silently misattribute. A separate judge catches many errors because it receives a narrower task:

> “Does this exact quote support this exact claim, owner, date, and status?”

Recent hallucination work operationalizes groundedness through explicit citations and claim-level evidence checking; it also notes a subtle failure mode where a model cites a relevant source but the source does not actually support the specific claim. citeturn865599view2 That maps perfectly to transcript mining: a quoted span may be “near” the commitment but not actually prove it.

Use two independent passes:

1. **Extractor:** high recall, produces candidate claims with evidence.
2. **Judge:** high precision, rejects unsupported / ambiguous / overclaimed records.

## Debate 4: Sentiment analysis vs concrete behavioral metrics

Vendors love sentiment. It is easy to demo. It is weakly actionable and culturally fragile, especially in RU/EN mixed calls.

Prefer:

- explicit objections,
- response latency to objections,
- whether objection was answered,
- commitment accepted or dodged,
- stakeholder seniority,
- follow-up specificity,
- founder defensiveness markers,
- investor excitement markers,
- evidence of traction, margin, CAC, retention, fundraising pressure, or urgency.

Sentiment can be a weak feature, not a decision input.

## Debate 5: Fine-tuning vs prompt engineering

Fine-tuning becomes useful only after you have a labeled corpus of your own errors. Until then, it mostly compresses your current prompt mistakes.

Use fine-tuning/local models for:

- candidate detection,
- quote classification,
- objection taxonomy,
- duplicate detection,
- RU/EN routing,
- simple commitment/no-commitment classification.

Keep frontier APIs for:

- final commitment adjudication,
- messy owner/date normalization,
- alpha hypothesis generation,
- cross-call synthesis,
- difficult multilingual ambiguity.

---

# D. Best Practices

## 1. Canonicalize every transcript before extraction

Your transcript markdown should be transformed into a canonical machine format before any LLM sees it.

Minimum fields:

```json
{
  "call_id": "DR26-07-03-HUB-01",
  "call_date": "2026-07-03",
  "timezone": "Europe/Lisbon",
  "calendar_title": "...",
  "crm_lead_id": "...",
  "participants_from_calendar": [
    {
      "person_id": "p_anton",
      "name": "Anton",
      "email": "...",
      "org": "...",
      "side": "internal"
    },
    {
      "person_id": "p_unknown_external_1",
      "name": "...",
      "email": "...",
      "org": "...",
      "side": "external"
    }
  ],
  "transcript_turns": [
    {
      "turn_id": "t0001",
      "speaker_channel": "microphone",
      "start_time": null,
      "end_time": null,
      "text": "..."
    }
  ]
}
```

Use stable `turn_id`s even if timestamps are absent. The judge should always work against `turn_id`s and quotes.

## 2. Maintain a speaker-resolution table

Do not write named speaker directly into extracted claims unless resolved.

Suggested table:

```sql
CREATE TABLE speaker_resolution (
  call_id TEXT NOT NULL,
  speaker_channel TEXT NOT NULL,          -- microphone, speaker, speaker_1, speaker_2, etc.
  candidate_person_id TEXT,
  candidate_party TEXT CHECK(candidate_party IN ('internal','external','unknown')),
  confidence REAL NOT NULL,
  basis_json TEXT NOT NULL,               -- calendar prior, self-intro, email domain, known channel mapping
  conflict_flag INTEGER DEFAULT 0,
  created_at TEXT NOT NULL,
  PRIMARY KEY (call_id, speaker_channel, candidate_person_id)
);
```

Resolution rules:

| Situation | Allowed attribution |
|---|---|
| Microphone channel known to be you | `person_id = Anton`, high confidence |
| One external attendee on calendar and one non-mic speaker channel | external attendee, medium/high confidence |
| Multiple external attendees, one speaker channel | `external_unknown`, not a named person |
| Transcript self-intro: “I’m Sergey from X” | named person if matches calendar/CRM |
| “As Maria said earlier…” | weak evidence only; do not assign channel by itself |
| Founder says “we’ll send it” | owner party may be external; person owner unknown |
| Counterparty says “Anton will send it” | request/expectation, not Anton commitment unless Anton accepts |

## 3. Use a strict claim schema

Do not create one generic extraction schema. Use separate record types with shared evidence fields.

### Shared fields

```json
{
  "record_id": "uuid",
  "call_id": "DR26-07-03-HUB-01",
  "record_type": "commitment | decision | lead_fact | objection | alpha_signal | crm_update | question | risk",
  "normalized_text": "...",
  "verbatim_quote": "...",
  "turn_ids": ["t0123", "t0124"],
  "speaker_channel": "speaker",
  "speaker_party": "external",
  "speaker_person_id": null,
  "speaker_resolution_confidence": 0.71,
  "support_status": "supported | weakly_supported | unsupported | contradicted",
  "extractor_model": "small-or-sonnet",
  "judge_model": "opus-or-other",
  "prompt_version": "v0.3.1",
  "confidence": 0.0,
  "created_at": "..."
}
```

### Commitment-specific fields

```json
{
  "commitment_type": "send_info | intro | schedule | review | decide | invest | follow_up | other",
  "commitment_strength": "hard | soft | tentative | request_only | rejected | superseded",
  "owner_party": "internal | external | mutual | unknown",
  "owner_person_id": null,
  "recipient_party": "internal | external | mutual | unknown",
  "due_date": "2026-07-10",
  "due_date_confidence": 0.73,
  "due_text": "next Friday",
  "status": "open | completed | overdue | superseded | cancelled | unknown",
  "supersedes_record_id": null,
  "completion_evidence_record_id": null
}
```

### Lead/CRM fact fields

```json
{
  "entity_type": "company | person | fund | product | market | round | metric | competitor",
  "field_name": "ARR | revenue | burn | runway | valuation | round_size | CAC | retention | geography | intro_source",
  "field_value_raw": "...",
  "field_value_normalized": "...",
  "currency": "USD | EUR | RUB | unknown",
  "as_of_date": "2026-07-03",
  "temporality": "current | historical | forecast | target | hypothetical",
  "sensitivity": "normal | confidential | highly_sensitive"
}
```

### Alpha-signal fields

```json
{
  "alpha_type": "market_signal | founder_signal | fundraising_signal | customer_pull | technical_edge | hidden_risk | network_edge | timing_signal",
  "non_obviousness": 0.0,
  "economic_materiality": 0.0,
  "evidence_strength": 0.0,
  "actionability": 0.0,
  "time_decay": "fast | medium | slow",
  "hypothesis": "...",
  "counterevidence_needed": ["..."]
}
```

## 4. Prompt pattern for per-call extraction

Use this structure.

```text
SYSTEM:
You extract structured, quote-grounded records from verbatim call transcripts.
You must not use the AI summary.
You must not infer facts not supported by transcript quotes.
No quote = no record.
Do not assign named speaker identity unless supported by metadata or transcript evidence.
When ambiguous, use unknown / external_unknown / internal_unknown.

INPUT:
- call metadata
- calendar participants
- known CRM entities
- speaker-channel priors
- numbered transcript turns

TASK:
Extract candidate records of these types:
1. hard commitments
2. soft commitments / tentative next steps
3. decisions / agreements
4. lead facts
5. objections / concerns
6. competitor mentions
7. alpha signals

For each record:
- cite exact turn IDs
- include a verbatim quote
- normalize only what is supported
- mark ambiguity explicitly
- do not collapse multiple commitments into one
- detect whether a later turn supersedes an earlier one

NEGATIVE EXAMPLES:
- "we should" without acceptance is not a hard commitment
- "can you send" is a request, not accepted commitment
- "maybe next week" is tentative, not due date
- "I'll try" is soft commitment
- "we'll see" is not a commitment
```

Then run a judge:

```text
You are validating one extracted record against transcript evidence.

Return:
- supported / weakly_supported / unsupported / contradicted
- whether quote proves the normalized claim
- whether quote proves owner
- whether quote proves due date
- whether commitment strength is overstated
- whether record should be split, merged, or deleted

Reject if:
- quote does not contain the claim
- owner is inferred without evidence
- due date is normalized beyond evidence
- request is mislabeled as acceptance
- later transcript text supersedes it
```

## 5. Use structured outputs, but do not rely on schema alone

Structured outputs solve malformed JSON. They do not solve hallucinated content.

Anthropic’s structured-output docs emphasize strict schema conformance and type-safe outputs, while also noting schema complexity limits and recommending splitting or flattening complex schemas. citeturn891053view0 OpenAI’s docs similarly distinguish JSON validity from schema adherence and recommend Structured Outputs when possible. citeturn515745view5

For you:

- Use strict JSON schema for extractor output.
- Keep schemas flat enough to avoid model failure.
- Validate with Pydantic/Zod.
- Reject missing evidence fields.
- Run independent quote-grounding judge.
- Store `model`, `prompt_version`, `schema_version`, and validation status.

Important Anthropic-specific caveat: their structured-output docs note that structured outputs work with batch, but are incompatible with citations. citeturn891053view0 So for transcript mining, do **not** depend on model-native citation mode. Put `turn_ids` and `verbatim_quote` inside your JSON schema and validate them yourself.

## 6. Prefer evidence spans over summaries for cross-call analytics

For pattern mining, cluster and analyze **claims/spans**, not whole calls.

Good units:

- 5–15 turn objection windows
- commitment evidence spans
- founder-answer spans after key questions
- pitch-module spans
- pricing/valuation discussion spans
- technical-differentiation spans
- “why now” spans
- “fundraising pressure” spans
- “customer pull” spans

Bad units:

- whole transcript embedding,
- AI summary embedding,
- one paragraph call recap,
- entire call compressed into one vector.

Whole-call embeddings are too coarse. They are fine for rough retrieval, not pattern discovery.

## 7. Cross-call analytics architecture

Use three layers.

### Layer 1 — deterministic metrics

Compute without LLM:

| Metric | Use |
|---|---|
| Talk/listen ratio | Conversation balance; compare by stage/outcome |
| Longest monologue | Pitch discipline / founder rambling |
| Turn count / interactivity | Conversational rhythm |
| Question rate | Discovery intensity |
| Time to first concrete ask | Sales/investor process quality |
| Number of explicit next steps | Process discipline |
| Commitment specificity | Owner + due date rate |
| Promise completion / rot | Execution reliability |
| Repeated objections | Product/market friction |
| Mention frequency of competitors, funds, sectors | Market map |
| Numeric facts extracted per call | Data richness |

These map to common vendor metrics such as talk/listen ratio, call duration, key moments, objection/competitor filtering, trackers, monologues, and scorecards. citeturn315750view2turn315750view12turn865599view1

### Layer 2 — embedding/topic mining

Use BERTopic-style or custom embedding clustering over evidence spans. BERTopic uses transformer embeddings and c-TF-IDF to form interpretable topic clusters. citeturn898458search0turn898458search3

Recommended pipeline:

```text
evidence spans
  → multilingual embeddings
  → dimensionality reduction
  → HDBSCAN / clustering
  → cluster labels via LLM
  → merge/split review
  → compare by outcome/stage/person/company
```

For your RU+EN corpus:

- Keep original quote language.
- Use multilingual embeddings.
- Store both original quote and normalized English/Russian label.
- Cluster by semantic span, not translated summary.
- Do not delete outliers automatically; in alpha mining, outliers are often the interesting part.

### Layer 3 — LLM map-reduce, but with denominators

LLM reduce passes should produce hypotheses, not facts.

Bad output:

> “Founders who mention enterprise traction tend to convert better.”

Good output:

```json
{
  "hypothesis": "Calls containing specific enterprise customer names plus quantified deployment stage appear more likely to advance.",
  "supporting_claim_ids": ["c1", "c7", "c19"],
  "counterexample_claim_ids": ["c33", "c41"],
  "denominator": {
    "calls_with_signal": 18,
    "advanced_with_signal": 11,
    "calls_without_signal": 235,
    "advanced_without_signal": 42
  },
  "effect_size": "...",
  "confidence": "medium",
  "next_test": "Review calls tagged enterprise_customer_specificity but not advanced."
}
```

Every cross-call pattern needs:

- examples,
- counterexamples,
- denominator,
- outcome stratification,
- effect size,
- time period,
- prompt/model version,
- reviewer status.

Without denominators, LLM pattern mining becomes astrology with nicer prose.

---

# E. Failure Modes

## 1. ASR error → wrong fact

ASR errors can corrupt names, numbers, dates, currencies, and technical terms. Recent work on multimodal meeting data reports that ASR quality correlates with summary coherence, and that multimodal cues reduced character error rate substantially in their dataset. citeturn550510view6 Another paper directly states that ASR errors significantly degrade downstream summarization, and studies synthetic ASR-like errors to improve robustness. citeturn558049view3

Mitigations:

- Re-transcribe high-value calls locally with Whisper/WhisperX.
- Keep transcript quote as evidence, but mark `asr_risk`.
- Flag numeric facts for review.
- Require confirmation for money amounts, dates, names, and legal/investment terms.
- Use glossary correction for known people, funds, companies, sector terms.
- Store alternate hypotheses for uncertain names.

## 2. Speaker misattribution → false owner

This is probably your highest-risk commitment failure.

Example:

```text
speaker: "I'll send it tomorrow."
```

If `speaker` is only “external speaker,” you may know the party, but not the person. If multiple founders are on the call, named owner is unknown.

Mitigations:

- Separate `speaker_channel`, `party`, and `person_id`.
- Use `owner_unknown_external` when needed.
- Store basis for speaker resolution.
- Review all high-value external commitments with ambiguous owner.
- Let CRM cards say “External party committed…” instead of inventing a person.

## 3. Summary drift

If you feed AI summaries into extraction, errors become invisible. The model may convert “we might” into “we will,” drop qualifiers, or assign names not present in the transcript.

Mitigations:

- Never extract from summaries.
- Generate summaries only from validated claims.
- Require quote-grounded atomic facts.
- Store unsupported-claim rate per model/prompt.

## 4. Over-extraction noise

LLMs love to extract every polite phrase as an action item.

Common false positives:

| Transcript phrase | Correct label |
|---|---|
| “We should discuss that” | suggestion |
| “Maybe we can send something” | tentative idea |
| “Can you send the deck?” | request |
| “We’ll see” | noncommitment |
| “Let’s keep in touch” | vague follow-up |
| “I’ll try” | soft commitment |
| “We can probably do that” | weak offer |
| “I need to check internally” | dependency / blocker |

Mitigation:

- Explicit negative examples in prompt.
- `commitment_strength` field.
- Judge downgrades hard commitments to soft/tentative/request.
- Default to “not a commitment” unless accepted owner/action exists.

The code-mixed action-item paper’s annotation guidance is relevant here: not every future task discussion is an action item; explicit assignment/agreement matters. citeturn937071view1

## 5. Due-date hallucination

“Next week” becomes a precise date. “After the call” becomes today. “Friday” gets assigned to the wrong week.

Mitigation:

```json
{
  "due_text": "next Friday",
  "due_date": "2026-07-10",
  "due_date_confidence": 0.68,
  "reference_date": "2026-07-03",
  "timezone": "Europe/Lisbon",
  "normalization_basis": "call_date + phrase"
}
```

If the phrase is vague, keep `due_date = null` and preserve `due_text`.

## 6. Commitment supersession

A call often contains:

```text
"I'll send it tomorrow."
...
"Actually, let's wait until after your partner meeting."
```

Naive extraction creates one open task. Correct extraction creates a commitment that is later superseded.

Mitigation:

- Full-call final pass.
- `supersedes_record_id`.
- Later evidence can cancel, delay, or alter commitment.
- Event-sourced status changes instead of destructive updates.

## 7. Cross-call spurious correlations

With ~253 calls, you can find seductive but fake patterns. Example:

> “Founders who mention CAC early are better.”

Maybe true. Maybe those calls were later-stage. Maybe those founders were in SaaS. Maybe one partner asked CAC questions only in higher-quality calls.

Mitigation:

- Stratify by call stage, source, sector, geography, language, and date.
- Require counterexamples.
- Track denominator.
- Use holdout periods.
- Treat LLM-generated patterns as hypotheses until tested.

## 8. RU/EN code-mixing and translation drift

Mixed Russian/English calls create errors in:

- transliterated names,
- legal/financial terms,
- idioms,
- “we” ambiguity,
- softening language,
- culturally specific commitment phrasing.

Mitigation:

- Store original quote unchanged.
- Normalize separately.
- Do not translate evidence quote.
- Use multilingual embeddings.
- Add RU/EN-specific negative examples.
- Review borderline commitments in original language.

## 9. Hallucinated citations / fake grounding

Even when a model produces citations, the cited source may not support the exact claim. Recent hallucination work explicitly calls out this failure mode: a model may cite an appropriate source while fabricating details not substantiated by that source. citeturn865599view2

Transcript equivalent:

```json
{
  "claim": "Founder committed to send audited financials Friday.",
  "quote": "Yeah, we can send over some numbers."
}
```

The quote is related but does not prove “audited,” “financials,” or “Friday.”

Mitigation:

- Judge each field independently:
  - action supported?
  - owner supported?
  - due date supported?
  - object supported?
  - commitment strength supported?
- Store partial support, not binary support.
- Reject or downgrade over-specific claims.

---

# F. Strategic Recommendations

## 1. Build a “claim ledger,” not a note generator

SQLite core tables:

```sql
CREATE TABLE call (
  call_id TEXT PRIMARY KEY,
  call_date TEXT NOT NULL,
  title TEXT,
  source TEXT,
  language_mix TEXT,
  crm_lead_id TEXT,
  transcript_hash TEXT,
  metadata_json TEXT
);

CREATE TABLE transcript_turn (
  call_id TEXT NOT NULL,
  turn_id TEXT NOT NULL,
  speaker_channel TEXT NOT NULL,
  start_time REAL,
  end_time REAL,
  text TEXT NOT NULL,
  text_hash TEXT NOT NULL,
  PRIMARY KEY (call_id, turn_id)
);

CREATE TABLE extracted_record (
  record_id TEXT PRIMARY KEY,
  call_id TEXT NOT NULL,
  record_type TEXT NOT NULL,
  normalized_text TEXT NOT NULL,
  speaker_channel TEXT,
  speaker_party TEXT,
  speaker_person_id TEXT,
  confidence REAL NOT NULL,
  support_status TEXT NOT NULL,
  extractor_model TEXT NOT NULL,
  judge_model TEXT,
  schema_version TEXT NOT NULL,
  prompt_version TEXT NOT NULL,
  created_at TEXT NOT NULL
);

CREATE TABLE record_evidence (
  record_id TEXT NOT NULL,
  call_id TEXT NOT NULL,
  turn_id TEXT NOT NULL,
  quote TEXT NOT NULL,
  quote_hash TEXT NOT NULL,
  evidence_role TEXT NOT NULL, -- primary, context, contradiction, supersession, completion
  PRIMARY KEY (record_id, turn_id, evidence_role)
);

CREATE TABLE commitment (
  record_id TEXT PRIMARY KEY,
  commitment_type TEXT,
  commitment_strength TEXT,
  owner_party TEXT,
  owner_person_id TEXT,
  recipient_party TEXT,
  due_text TEXT,
  due_date TEXT,
  due_date_confidence REAL,
  status TEXT,
  supersedes_record_id TEXT,
  completion_record_id TEXT
);

CREATE TABLE crm_fact (
  record_id TEXT PRIMARY KEY,
  entity_type TEXT,
  field_name TEXT,
  field_value_raw TEXT,
  field_value_normalized TEXT,
  as_of_date TEXT,
  temporality TEXT,
  sensitivity TEXT
);

CREATE TABLE record_judgment (
  judgment_id TEXT PRIMARY KEY,
  record_id TEXT NOT NULL,
  judge_model TEXT NOT NULL,
  support_verdict TEXT NOT NULL,
  owner_supported INTEGER,
  due_date_supported INTEGER,
  quote_supports_claim INTEGER,
  overclaim_flags_json TEXT,
  rationale_short TEXT,
  created_at TEXT NOT NULL
);
```

This gives you:

- reproducibility,
- regression testing,
- prompt/model comparisons,
- human review,
- Obsidian rendering,
- CRM sync,
- cross-call analytics.

## 2. Use your model ladder explicitly

Recommended ladder:

| Stage | Model/tool | Goal | Precision/recall target |
|---|---|---:|---:|
| Deterministic preprocessing | Python/SQLite | line IDs, metadata, dates, obvious metrics | exact |
| Candidate detection | local/small model + regex | high-recall spans | recall > precision |
| Structured extraction | Sonnet-class model | normalized records | balanced |
| Grounding judge | Opus-class or different frontier model | reject unsupported records | precision > recall |
| Cross-call hypothesis generation | frontier model | generate patterns | creative but cited |
| Quant validation | Python/SQL | denominators/effect sizes | exact |
| Human review | Obsidian/CRM | high-value ambiguity | final authority |

Use batch APIs and prompt caching for nightly jobs. Anthropic’s batch docs state that Message Batches support prompt caching and that discounts stack, with cache-hit rates ranging from 30% to 98% in examples. citeturn515745view1 Anthropic’s pricing page states that Batch API usage gets a 50% discount on input and output tokens. citeturn515745view3 Prompt caching is especially relevant because your instructions, schemas, examples, and ontology are static across calls. citeturn515745view2

## 3. Do not overinvest in real-time yet

Streaming is useful for:

- live coaching,
- battle cards,
- objection alerts,
- monologue warnings,
- real-time CRM prompts.

Your current goal is:

- post-call extraction,
- promise tracking,
- lead facts,
- cross-call pattern mining,
- alpha discovery.

Nightly batch is better:

- cheaper,
- easier to validate,
- easier to rerun,
- less latency pressure,
- better for full-call adjudication,
- compatible with review queues.

Add streaming only after the offline system proves which signals are actually valuable.

## 4. Clone vendor metrics selectively

Clone:

| Metric/product feature | Clone? | Why |
|---|---:|---|
| Talk/listen ratio | Yes | Cheap, useful process feature |
| Longest monologue | Yes | Detect rambling/pitch overload |
| Interactivity / turn switches | Yes | Good engagement proxy |
| Topic/keyword/concept trackers | Yes | Useful for pricing, objections, competitors, sectors |
| Objection taxonomy | Yes | Directly actionable |
| Explicit next-step rate | Yes | Strong process metric |
| Commitment owner+due-date rate | Yes | Directly affects follow-up |
| Promise completion / rot | Yes | Your private moat |
| CRM auto-fill with evidence | Yes | High leverage |
| Call scorecards | Yes, but custom | Use your own VC/deal rubric |
| Sentiment | Weak feature only | Noisy, culturally fragile |
| Generic AI summaries | Low priority | View layer, not data layer |
| Auto-send follow-ups | No / review only | High reputational risk |
| “Deal risk score” | Later | Needs labeled outcomes |

## 5. Build VC-specific alpha ontology

Generic sales tools will not give you alpha. You need a taxonomy tuned to deal-flow.

Suggested alpha classes:

| Alpha class | Examples |
|---|---|
| Market timing | sudden demand, regulatory shift, customer budget opening |
| Hidden traction | named customers, usage intensity, organic pull, waitlist quality |
| Founder signal | speed of follow-up, specificity, technical depth, evasiveness |
| Fundraising pressure | runway, bridge language, valuation flexibility, insider behavior |
| Network edge | warm intro paths, co-investor info, customer intros |
| Technical defensibility | proprietary data, integration depth, distribution lock-in |
| Deal-process signal | who controls next step, urgency, decision authority |
| Negative alpha | inflated metrics, vague pilots, churn euphemisms, dependency risk |
| Information asymmetry | fact not in deck/CRM, only surfaced verbally |

Each alpha record should require:

```json
{
  "hypothesis": "...",
  "quote": "...",
  "why_non_obvious": "...",
  "why_economically_material": "...",
  "what_to_do_next": "...",
  "confidence": 0.0,
  "counterevidence_needed": ["..."]
}
```

## 6. Add a human-review queue where it matters

Review everything? Too expensive.

Review only:

- high-value alpha,
- commitments with external owner and due date,
- money amounts,
- valuation/round terms,
- ambiguous speaker attribution,
- low-confidence but high-materiality records,
- records that would trigger an outbound follow-up,
- contradictions between calls.

Obsidian card example:

```markdown
## Commitment: send updated metrics

**Status:** Open  
**Owner:** external_unknown, likely Sergey Ivanov, confidence 0.71  
**Due:** next week → normalized 2026-07-10, confidence 0.68  
**Call:** DR26-07-03-HUB-01  
**Evidence:**  
> speaker [t184]: "Yeah, I'll send you the updated metrics next week."

**Judge:** supported claim, weak owner, weak exact date  
**Action:** review before naming owner in CRM
```

---

# G. Action Plan

## Phase 0 — One-time design pass

Deliverables:

1. Commitment ontology.
2. Lead-fact schema.
3. Alpha-signal schema.
4. Speaker-resolution policy.
5. Evidence/quote policy.
6. Judge rubric.
7. Obsidian rendering template.
8. SQLite migration.

Hard policy decisions:

```text
- No quote = no record.
- No speaker evidence = no named owner.
- Request != accepted commitment.
- Suggestion != commitment.
- AI summary is never source evidence.
- Later transcript evidence can supersede earlier commitments.
- Each extracted record must be reproducible from call_id + turn_ids + prompt_version + model_version.
```

## Phase 1 — Build gold dataset

Use 30–50 calls first, not all 253.

Stratify:

- RU-only,
- EN-only,
- mixed RU/EN,
- one external speaker,
- multiple external speakers,
- founder call,
- investor call,
- follow-up call,
- noisy transcript,
- high-value deal,
- dead lead.

Annotate manually:

| Label | Target |
|---|---|
| hard commitments | owner/action/due |
| soft commitments | weak owner/action/due |
| requests | not accepted |
| decisions | agreed state change |
| lead facts | CRM-worthy |
| objections | concern taxonomy |
| alpha signals | non-obvious + material |
| false positives | “should/maybe/we’ll see” |

Metrics:

```text
commitment_precision
commitment_recall
hard_commitment_precision
owner_accuracy
due_date_accuracy
quote_support_rate
unsupported_claim_rate
duplicate_rate
speaker_attribution_error_rate
crm_fact_precision
alpha_precision_after_review
reviewer_minutes_per_call
```

## Phase 2 — Implement per-call extraction

Nightly job:

```text
1. Load new Granola transcript.
2. Normalize markdown → canonical JSON turns.
3. Join calendar + CRM metadata.
4. Run deterministic metrics.
5. Run high-recall candidate detector over windows.
6. Run structured extraction.
7. Run grounding judge.
8. Write accepted/weak/rejected records to SQLite.
9. Render Obsidian cards.
10. Update CRM fields only for supported high-confidence records.
```

Use three confidence gates:

| Gate | Destination |
|---|---|
| Supported + high confidence | auto-materialize |
| Supported but ambiguous owner/date | review queue |
| Weak/unsupported | store for eval, do not act |

## Phase 3 — Build commitment lifecycle tracker

Create event transitions:

```text
detected
  → validated
  → open
  → completed
  → overdue
  → renegotiated
  → superseded
  → cancelled
  → rotted
```

Completion detection sources:

- later calls,
- CRM notes,
- email if connected,
- manual checkboxes,
- file arrival,
- Obsidian task completion.

Important: completion also needs evidence.

Bad:

```text
status = completed because model thinks it happened
```

Good:

```json
{
  "status": "completed",
  "completion_evidence": {
    "source": "later_call",
    "quote": "I sent the metrics yesterday.",
    "turn_ids": ["t042"]
  }
}
```

## Phase 4 — Cross-call analytics dashboard

Start with boring metrics:

```sql
-- Promise rot by owner party
SELECT owner_party,
       COUNT(*) AS promises,
       SUM(status = 'completed') AS completed,
       SUM(status = 'overdue') AS overdue,
       AVG(julianday(completed_at) - julianday(created_at)) AS avg_days_to_complete
FROM commitment
GROUP BY owner_party;
```

Core dashboards:

1. Commitment volume by week.
2. Promise completion rate.
3. External vs internal promise rot.
4. Talk/listen ratio by call type/outcome.
5. Longest monologue by call.
6. Objection frequency by stage.
7. Pitch module vs outcome.
8. CRM fact density by lead source.
9. Alpha signals by type.
10. Follow-up latency after call.

## Phase 5 — Pattern mining

Weekly, not nightly.

Pipeline:

```text
validated evidence spans
  → embedding clusters
  → LLM cluster labels
  → SQL outcome analysis
  → LLM hypothesis synthesis
  → human review
  → alpha memo
```

Each pattern memo should have:

```markdown
## Pattern: Specific customer-name traction beats generic traction claims

**Signal definition:** external speaker names specific customers and deployment status.

**Support:** 14 / 31 advanced calls; 7 / 89 non-advanced calls.

**Examples:**
- call_id, quote
- call_id, quote
- call_id, quote

**Counterexamples:**
- call_id, quote

**Potential explanation:** ...
**Action:** ask for named-customer deployment status earlier in first calls.
**Confidence:** medium
```

## Phase 6 — Fine-tune only after you have labels

Do not fine-tune immediately.

Fine-tune when you have:

- at least several hundred reviewed commitment candidates,
- explicit negative examples,
- speaker-attribution edge cases,
- stable schema,
- stable judge rubric.

Use fine-tuning/local models for:

- cheap candidate classification,
- objection taxonomy,
- quote span detection,
- language routing,
- duplicate detection.

Keep frontier models for:

- hard ambiguous cases,
- final alpha synthesis,
- cross-call reasoning,
- speaker/date/owner adjudication.

---

# H. Sources

## Academic and benchmark sources

- **AMI Corpus** — 100-hour meeting corpus with synchronized recordings and annotations; useful baseline but not directly representative of VC/sales calls. citeturn550510view1  
- **QMSum** — query-based multi-domain meeting summarization benchmark with 1,808 query-summary pairs over 232 meetings; supports locate-then-summarize thinking. citeturn550510view0  
- **MeetingBank** — notes that older ICSI/AMI meeting corpora are small and emphasizes summaries that capture issues, decisions, and actions. citeturn937071view2  
- **Action-item detection research** — highlights scarcity of annotated action-item datasets and need for local/global context. citeturn371460view0  
- **Code-mixed Urdu-English action-item dataset** — useful annotation guidance for distinguishing real action items from generic instructions or future-task talk. citeturn937071view1  
- **Re-FRAME** — recent LLM meeting summarization work focusing on hallucination, omission, and fact organization. citeturn550510view4  
- **What’s Wrong? Refining Meeting Summaries with LLM Feedback** — meeting-summary error taxonomy and multi-LLM refinement. citeturn371460view2  
- **MISP-Meeting** — multimodal meeting benchmark showing downstream dependence on ASR quality. citeturn550510view6  
- **FAME/MIMIC synthetic meeting data** — shows the field moving toward synthetic/multilingual meeting corpora because high-quality meeting data is limited by privacy, IP, and collection costs. citeturn371460view1  

## Diarization / ASR / open-source infrastructure

- **pyannote.ai benchmark** — defines diarization error components: speaker confusion, missed speech, false alarm. citeturn558049view0  
- **pyannote speaker diarization model docs** — notes downmixing/resampling behavior and speaker-count options. citeturn558049view2  
- **WhisperX** — open-source ASR alignment and diarization stack with word-level timestamps and pyannote diarization. citeturn558049view1  
- **Diarization benchmarking work** — states diarization remains unsolved and errors propagate downstream. citeturn558049view4  
- **MEDSAGE** — studies ASR-like errors and their impact on summarization robustness. citeturn558049view3  

## Industry / competitive landscape

- **Gong Conversation Intelligence** — AI summaries, CRM updates, follow-ups, dashboards, scorecards, coaching recommendations. citeturn315750view0  
- **Gong Trackers** — keyword/concept tracking for product messaging, pricing, discovery, objections. citeturn315750view1  
- **Gong team performance metrics** — talk/listen ratio, longest monologue, customer story, interactivity. citeturn315750view2  
- **Gong 2025 talk/listen analysis** — warns against simplistic magic-ratio thinking. citeturn315750view3  
- **Fireflies** — talk time, sentiment, topic trackers, CRM auto-fill, tasks, AskFred. citeturn315750view4turn315750view5turn315750view6  
- **Fathom** — summaries, action items, CRM sync, deal view, coaching metrics, scorecards. citeturn315750view7turn315750view8  
- **Attention** — AI sales agents, key moments, top calls, scorecards, conversation memory. citeturn315750view9  
- **Clari Conversation Intelligence / Copilot** — objections, competitor mentions, talk/listen, scorecards, battle cards, deal central. citeturn865599view1  

## LLM pipeline and architecture sources

- **Anthropic Structured Outputs** — schema-constrained outputs, validation, schema limitations, batch compatibility. citeturn891053view0  
- **Anthropic Batch Processing** — batch API and prompt-caching support. citeturn515745view1  
- **Anthropic Prompt Caching** — reuse static prompt prefixes, useful for repetitive structured tasks. citeturn515745view2  
- **Anthropic Pricing** — Batch API 50% token discount. citeturn515745view3  
- **OpenAI Structured Outputs** — schema adherence vs plain JSON mode. citeturn515745view5  
- **Gemini long context** — 1M-token context use cases and reduced need for truncation/summarization. citeturn515745view6  
- **DocETL** — declarative LLM document-processing pipelines with validation and optimization; relevant for large-scale document/call ETL. citeturn550510view7  
- **LOTUS** — semantic operators over datasets for LLM-powered data processing. citeturn550510view8turn550510view9  
- **BERTopic** — transformer-embedding + c-TF-IDF topic modeling for interpretable clusters. citeturn898458search0turn898458search3  

## Bottom line

Build the distillation layer as a **grounded, event-sourced, evidence-first CRM intelligence system**.

Do **not** optimize for prettier summaries. Optimize for:

```text
supported claim rate
owner accuracy
due-date accuracy
commitment recall
promise rot visibility
CRM field precision
alpha hit rate after human review
```

Your likely moat is not the LLM. It is the combination of:

```text
private call corpus
+ strict evidence schema
+ commitment lifecycle tracking
+ VC-specific alpha ontology
+ cross-call denominator discipline
+ fast nightly iteration
```
