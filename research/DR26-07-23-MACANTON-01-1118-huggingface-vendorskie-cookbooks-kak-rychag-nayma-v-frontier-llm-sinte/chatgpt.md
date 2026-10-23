# DR26-07-23-MACANTON-01-1118 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "DR26-07-23-MACANTON-01-1118 — ChatGPT Pro Deep Research (verbatim capture) — HF/vendor cookbooks as a hiring lever"
type: dr-report
dr_id: DR26-07-23-MACANTON-01-1118
vendor: chatgpt
model: Pro Deep Research (o3/GPT-5.2)
captured: 2026-08-24


chat_url: https://приватный чат
run_stats: "14 min · 31 citations · 239 searches"
extraction_method: "agent-chrome a11y snapshot of the deep-research sandbox iframe; Executive Summary captured verbatim. Full body (B–I: tables, flowchart, gantt, 31 sources) remains in the cross-origin sandbox — retrieve via Export→Markdown from chat_url."
tags: [dr, dr-report, chatgpt, verbatim, mission2, huggingface, hiring, oss]
---

# ChatGPT Pro Deep Research — DR26-07-23-MACANTON-01-1118 (verbatim, Executive Summary)

> ⚠️ Только Executive Summary снят дословно из a11y-дерева. Полные разделы B–I (Key Findings с таблицами весов, Named Precedents table, Contradictions, Best Practices, Failure Modes, Strategic Recs, 30/60/90 Gantt, Sources) живут в cross-origin sandbox чата — забрать через Export→Markdown по chat_url.

## Title
Hugging Face and Vendor Cookbooks as a Hiring Lever Into Frontier LLM Companies

## A. Executive Summary (verbatim)

The highest-leverage way to use Hugging Face for hiring is not to accumulate unrelated public artifacts. It is to build a compact, mutually reinforcing chain in which the same technical idea is independently legible across several surfaces: maintainer-reviewed code → reproducible benchmark/data → interactive demonstration → technical explanation → repeated interaction with the same maintainers → role-specific application. Hugging Face is unusually good for this strategy because those surfaces live in one identity graph. A Cookbook author can link directly to a Hugging Face or GitHub profile; a merged recipe appears in the public Open-Source AI Cookbook; the author can request membership in the public huggingcooks organization; the same identity can own Models, Datasets and Spaces; Hub discussions and pull requests can notify mentioned participants; and the contributor can then move from the relatively accessible Cookbook/Hub layer into higher-selectivity library repositories such as smolagents, lighteval, TRL, or transformers. The Cookbook README explicitly directs new-recipe contributors to tag HF maintainers Merve Noyan and Steve Liu and says contributors can request the huggingcooks badge after contributing.

For Palo Alto AI Research Lab specifically, the optimal sequence is therefore [the report gives a sequenced plan — see H. Action Plan in full report]. This is a better strategy than opening ten shallow cookbook PRs.

The strongest documented Hugging Face hiring precedents involve sustained technically valuable contributions and repeated exposure to maintainers, not a single viral post or notebook. Niels Rogge describes beginning with Transformers model contributions in 2020 and says that, after working on multiple PRs, Hugging Face eventually asked him to join. Xuan-Son Nguyen describes contributing to llama.cpp/llama-server, being contacted by Hugging Face CTO Julien Chaumond around mid-2024, and joining in August 2024.

The sharp ranking depends on whether we ask about raw hiring weight or signal per unit effort [ranking table in full report]. There is one major exception: external adoption can overwhelm review selectivity. A benchmark Dataset that becomes standard infrastructure, a Model with genuine downstream use, or a Featured/viral Space can be a stronger hiring signal than one ordinary library PR. Hugging Face itself now exposes decentralized evaluation results on model pages and benchmark leaderboards, including community-submitted evaluations, which makes adopted evaluation infrastructure especially legible.

For your existing assets, evaluation + governance of agents is a considerably stronger positioning vector than "we build multi-agent demos." The most compelling artifact family would combine citation-faithfulness evaluation, adversarial verifier agents, correct-defer/human-escalation behavior, authority/tool-policy gating, agent memory failure cases, and the quality/latency/cost trade-off between pipeline and barrier concurrency. That maps cleanly to Open Source ML, Agents, Evals/Research Engineering, and Developer Experience roles. Google DeepMind describes Research Engineers as the bridge between theory and implementation who build and scale systems to test and evaluate ideas; OpenAI currently advertises a Developer Experience Engineer role centered on demos, sample applications and technical content, while its Codex Deployment Engineer role explicitly includes contributing examples, guides and patterns to the OpenAI Cookbook.

Bottom line: a good HF Cookbook merge can get you known to the right two or three people. A good HF library PR can make those people take your engineering ability seriously. An adopted benchmark can give other labs a reason to care. The hiring conversion comes from connecting all three and then making a low-friction routing ask—not from waiting for a recruiter to discover a star count.

## Rails note
- CLI fanout (grok, codex) для этого тяжёлого DR не сработал: grok вернул 656-байтную преамбулу-пустышку, codex дважды таймаут (300s+540s), gemini CLI мёртв (IneligibleTier — Google убил Code Assist для физлиц). Единственная живая рельса DR-класса на Mac16 = ChatGPT Pro в real Chrome.
- Хабу отправлен параллельный TASK на 3-вендорный веер (Syncthing доехал; TG-03 в тот тик падал). Если хаб соберёт Grok/Gemini — добор в этот же файл.

Related: multi-vendor-cookbook-radar, oss-backlog-multi-vendor-wave2, mission-get-noticed-hired-by-llm-company, ship-github-no-plus-wait.
