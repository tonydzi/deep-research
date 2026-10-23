# DR26-07-30-MACANTON-01-1456 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR: Anti-AI-slop humanizing — ChatGPT (verbatim)

- source: chatgpt.com (Pro, аккаунт почтовый ящик, GPT-5.5 Deep Research)
- url: https://приватный чат
- title: Анти-AI стиль контента (проект «Ai Palo Alto Не удалять»)
- date: 2026-07-30 03:43 (запущен Антоном лично)
- collected: 2026-07-30, Mac, headless Firefox (AutoFF) + in-page fetch backend-api
- consumer: скилл AI_SLOPE (анти-слоп хуманизатор)

## Исходное ТЗ Антона (промпт, verbatim из чата)

Deep research
 Нужно сделать еще одну сессию в Claude: как писать, чтобы я сделал контент-фабрику.

Есть все, что мне понадобится, насколько решается контент. Особенно все, что связано с разработкой, которую делаю в Claude или в Codex.

Я хочу сделать такой скилл: анти-AI-slop. Типа анти-AI, по-моему.

То есть скилл, который берет текст, написанный роботом, и делает его более человеческим. Как бы ухудшает его по-человечески:

— убирает лишние знаки препинания;  
— убирает emoji;  
— убирает тирешки;  
— убирает букву «ё»;  
— очень сильно сокращает текст.

Я люблю писать очень длинные вещи, а у людей, которые меня читают, мало времени. Им нужно очень коротко, по делу, по существу и так далее.

Мне нужно сделать такой скилл и сначала провести глубокое исследование: как писать так, чтобы текст выглядел человеческим.

Чтобы, даже если текст написал робот, он не был избыточным. Робот написал слишком много — мы сокращаем.

Роботы привыкли добавлять emoji и прочую херню — мы все это убираем.

Делаем контент так, будто его написал очень занятый человек, у которого нет времени писать, нет времени расставлять знаки препинания, и он иногда может ошибиться.

Написание сильно сокращаем, концентрируя мысли.

И еще очень важно добавлять человечность в посты: больше событий, которые происходят у человека в жизни.

Нужно сделать отдельную сессию в Claude для разработки skill’а для контент-фабрики: **anti-AI-slop / anti-AI style**.

Задача skill’а: брать текст, написанный AI, и превращать его в более человеческий, короткий и живой текст.

Что должен делать skill:

- сильно сокращать длинные тексты
- оставлять только суть, по делу
- убирать AI-избыточность и канцелярит
- убирать emoji, лишние тире, идеальную пунктуацию, букву «ё» и прочие маркеры AI-текста
- делать стиль похожим на текст очень занятого человека: коротко, концентрировано, иногда неидеально
- можно допускать небольшую небрежность, как в реальной переписке
- добавлять больше человечности: реальные события, личные наблюдения, жизненные детали
- писать так, чтобы людям с малым количеством времени было быстро и понятно читать

Перед созданием skill’а нужно провести глубокое исследование: как именно переписывать AI-тексты так, чтобы они выглядели человеческими, не перегруженными и живыми. Особенно важно для контента и для всего, что связано с разработкой в Claude / Codex.

Summary: GPT-5.5 + whisper

 
Deep research
 Show more


---

## Отчёт (report_message, verbatim)

# Linguistic Differences: AI vs Human Writing

- **Complexity vs Clarity:** AI text tends to use more complex syntax and advanced vocabulary, often yielding higher “grade level” scores (e.g. Flesch–Kincaid, Gunning-Fog) but lower readability. Studies show AI-generated essays or abstracts have higher lexical diversity and sentence complexity than comparable human writing【11†L293-L302】【8†L721-L730】. In practice this means AI often writes in long, information-dense sentences, using formal connectors (e.g. “Additionally,” “moreover,” “in order to”) and technical terms. Human writers – especially time-pressured ones – favor shorter sentences, simpler words, and active voice【38†L100-L109】【11†L293-L302】.  

- **Vocabulary and Repetition:**  AI outputs often repeat certain “AI-style” words and phrases (e.g. *crucial, crucially, additionally, foster*【25†L13-L22】) and nominalized forms, leading to low lexical diversity in context. By contrast, human text (even if using a narrower vocabulary) tends to reuse a broader array of common words and phrases naturally. For example, one study found human essays had *fewer* unique words overall but used more varied sentence structures and discourse markers【32†L1-L4】【14†L370-L378】.  

- **Voice and Tone:**  AI writing is characteristically neutral and formal (often emotionless). It seldom uses first-person pronouns, contractions, or hedges. Humans typically inject personal voice: using “I/we/you,” colloquial language, and markers of uncertainty or emphasis (e.g. *maybe, guess, honestly*). Research notes that students’ essays contain many modals and personal discourse signals, whereas ChatGPT’s output was grammatically polished but rhetorically tentative【14†L370-L378】【26†L1-L4】. In short, AI lacks subjective details and context (“Why is the writer saying this?”) that humans naturally include.

- **Filler and Redundancy:**  AI often includes filler phrases and over-elaboration. It may provide excess background (“AI tends to do X; furthermore, Y; additionally, Z”), whereas a busy human writer would cut such padding. Experts observe AI responses can be *verbose or tangential*, violating conversational maxims of brevity【26†L1-L4】【35†L169-L177】. Human communication usually omits fillers like “in order to,” “basically,” or long digressions【35†L169-L177】【38†L100-L109】.

- **Punctuation and Formatting:**  AI typically produces well-punctuated text with proper grammar. A real person in a hurry might omit some punctuation, run clauses together, or use casual markers (like ellipses or dashes) inconsistently. (For example, the user’s spec prohibits “—” and extra hyphens, since “AI” often uses perfectly-formed lists or lists with dashes.) AI also sometimes overuses formatting like lists or markdown, which humans rarely write spontaneously.

# Humanization Strategies

- **Cut Filler and Clutter:**  Remove unnecessary words and phrases to sharpen meaning. As Nielsen Norman Group advises, use *plainspoken words* and *short sentences*, cutting “in order to, basically, just, furthermore,” etc.【35†L169-L177】【38†L100-L109】. This improves readability (target ~8th-grade level for general audience【38†L100-L109】) and mimics a busy writer who writes tersely.  

- **Shorten and Simplify:**  Break long sentences into shorter ones. Replace complex clauses with simple statements. Omit subordinate clauses and passive constructions. For instance, transform “In order to achieve X, we have implemented Y, which has resulted in Z” into “We did Y and got Z.” This aligns with findings that shorter, active sentences improve user comprehension【38†L100-L109】.

- **Use Conversational Voice:**  Insert first-person or second-person pronouns and contractions (e.g. “I think,” “we’re,” “you’ll”). Ask rhetorical questions. Emulate the pattern of thinking-out-loud (“Here’s the deal…”, “You know…”). This adds a human tone absent in AI text【14†L370-L378】. For example, if the AI text says “It is important to consider…,” a humanized version might say “I noticed it’s really important to think about….”

- **Add Personal Details/Anecdotes:**  Weave in brief personal or concrete examples to illustrate points. AI text is abstract and context-free; humans ground ideas in experience. Even a generic detail (“Yesterday, I ran into a problem where…”) can make the text feel lived-in【14†L409-L413】. According to educators, AI writing’s lack of “subjective detail” and “contextual anchoring” is a key giveaway【14†L409-L413】. Adding a small anecdote or everyday example counteracts that.

- **Allow Imperfections:**  Introduce minor grammar slips or informal punctuation. A hurried human might forgo a comma or capitalize a word incorrectly. Punctuate conversationally: use em-dashes for asides, ellipses for trailing thoughts, or even sentence fragments (e.g. “Kind of mess up? Happens.”). (Sources note that AI’s rigid correctness can sound robotic【14†L438-L445】, so small “flaws” can increase authenticity.) However, avoid major typos or confusion.

- **Replace “AI” Vocabulary:**  Swap out formal/abstract words that AI often uses. For example, instead of “additionally” or “furthermore,” use “also,” “so,” or just break into two sentences. Change “utilize” to “use,” “methodology” to “approach,” “issue” to “problem,” etc. Avoid buzzwords and corporate-speak. The Microsoft style guide warns that vague phrases like “cutting-edge technology” sound generic – replace them with specific terms or examples【35†L138-L147】.

- **Trim to the Essentials:**  Focus only on the core message. If AI output is long-winded, extract the main points and discard the rest. You can use summarization heuristics (e.g. pick key sentences by thematic relevance) or simply ask “What is the one thing the reader must know?” and rewrite to that. The result should read like notes, not a full lecture.

# Transformation Pipeline (Rules & Heuristics)

Implement the above strategies in a systematic rewriting process. For example, one might design a pipeline or “skill” as follows:

1. **Preprocess (Filter Out AI “Tells”):** Use regex or NLP tools to strip emojis, emoticons, and special characters. Remove the letter “ё” if present (as requested). Delete extra dashes or em-dashes (—) used in lists. Normalize text spacing (remove double spaces, etc.).  

2. **Summarization Step:** Extract or generate a concise summary of each paragraph/block. This can be done by identifying the main clause (using dependency parsing or TF-IDF to score sentences) and discarding subordinate info. Alternatively, prompt a model (Claude/GPT) with “Summarize this in 1-2 sentences:” to get a terse outline.  

3. **Rewriting to Human Voice:** For each summarized point, rewrite with human-style heuristics:  
   - **Tone Prompting:** Use a directive in the LLM or your code like “Rewrite this as a busy person’s note to a friend.” or “Simplify and add a personal tone.” This can yield a draft with contractions and examples.  
   - **Grammar Adjustments:** Enforce active voice (change “was done by” to “we did”), shorten phrases (“In order to” → “To”), and drop unnecessary adjectives.  
   - **Personalization:** Where possible, the writer (or model) can insert small personal touches. E.g., add a clause “I had a similar situation…” or a casual lead like “FYI,” depending on context.  
   
   You could encode these rules in a script (e.g. a Codex-generated Python script using `spaCy` or `NLTK` to find passive constructions and convert them) or rely on a prompt with those instructions.

4. **Post-Editing:** Finally, manually (or via rules) scan for any remaining AI markers. Remove formal conjunctions (“however,” “moreover”) if they don’t fit. Replace overly precise punctuation with simpler forms (e.g. change semicolons to periods, drop some commas). Add imperfection by occasionally leaving a missing comma or a shorthand (“didnt” with apostrophe). Ensure the flow feels like spoken thought.  

5. **Tone Consistency Check:** Optionally, use tools (e.g. readability analyzers or tone detectors) to verify the tone. For instance, Grammarly or Hemingway Editor can flag “formal” tone; aim for a lower grade-level score.  

   <!-- Example transformation rule in pseudo-format (not real JSON) -->
   ```
   Ruleset:
     - Remove: emoji, ‘—’, letter “ё”
     - Delete filler phrases: ["in order to", "basically", "just", "furthermore"]
     - Replace complex words: {"additionally":"also", "utilize":"use", "crucial":"big", ...}
     - Sentence length: split any sentence >25 words into two.
     - Tone: insert a first-person pronoun every 2-3 sentences.
   ```  

# Evaluation & Trade-offs

- **Readability Metrics:** Use automated scores (e.g. Flesch-Kincaid grade level, Hemingway readability) to ensure the text is easier. The goal is a lower reading level and higher “readability” score than the AI original【11†L293-L302】【38†L100-L109】. Short sentences and familiar words will drive the score down.  

- **Human-Likeness:** Where possible, test if humans can distinguish the output. Small-scale user studies (asking volunteers whether text feels human-written) are ideal. Alternatively, see if AI-detectors (like GPT-2 Output Detector) lower their “AI-written” confidence after rewriting. These are imperfect, but a decrease suggests more natural flow.  

- **Content Integrity:** Check that key facts or meanings are preserved. Aggressive shortening risks losing nuance. Always verify that the summary captures the original intent.  

- **Authenticity Checks:** The “soft” goal is authenticity. Metrics like lexical diversity and use of personal words can be tracked (e.g. count of first-person pronouns, presence of personal names/events). There’s no standard formula, but one can roughly gauge: human texts usually score higher on empathy/tone scales and lower on “perplexity” by large-language models.

**Trade-offs & Limits:** Over-humanizing can backfire. Excessive personal details or errors may confuse readers or degrade credibility. Removing too much structure/punctuation might make text choppy. Also, adding anecdotes must stay relevant to avoid fabricating misleading content. It’s impossible to algorithmically “know” what real life details to add unless given a persona. In practice, some manual tuning is needed. Finally, these heuristics were derived from academic and usability research, so their effectiveness can vary by genre and audience【38†L100-L109】【14†L409-L413】.

