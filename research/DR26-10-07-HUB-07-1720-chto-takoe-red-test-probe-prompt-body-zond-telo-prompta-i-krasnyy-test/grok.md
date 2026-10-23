# DR26-10-07-HUB-07-1720 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

A. Ищу живые источники по формулировке «red-test probe» и связанным практикам красных тестов.A. Finding

The exact string "red-test probe prompt body" did not show up in this run as the title of a published spec, paper, or tool. A web search for that exact phrase returned nearby but different things: LLM red-team probes, a YAML probe package whose page did not load here, and a physical high-voltage "red test probe." Treat the phrase as three separate ideas that people already document: a red test (a check that must fail on the broken version), a probe (one adversarial case or a bundle of them), and the prompt body (the exact text sent to the model).

B. In garak, the probe prompt body is the assembled string in `self.prompts`

NVIDIA garak calls a probe a bundle of adversarial prompts aimed at one failure mode. The Red Hat walkthrough (29 September 2026, page marked updated 8 October 2026) says you pick a probe, send those prompts, and read PASS or FAIL. PASS means the model resisted most of the attacks in that probe. FAIL means too many got through. In their published run of `probes.promptinject.HijackHateHumans` against IBM Granite 4.1 3B through an OpenAI-compatible local server, garak 0.17.0 reported the detector `promptinject.AttackRogueString` as FAIL: 213 of 1280 generations were ok, attack success about 83.36 percent (interval they printed: 81.25 to 85.39). Some replies repeated the target string. Some refused. One reply both refused and restated the instruction. Source: https://developers.redhat.com/articles/2026/09/29/red-team-your-ai-model-garak

The probe source shows where the body comes from. `garak/probes/promptinject.py` does not store one handwritten paragraph. `load_data_all_attacks` builds a config with three slots: a base (`prompt_data.ALL_OPENAI_PLAYGROUND_PROMPTS`), an attack chosen from five named hijacks (`ignore-print`, `ignore-say`, `ignore-print-upper`, `nevermind`, `screaming-stop`), and a rogue string (`hate-humans`, `kill-humans`, or `long-prompt`). Settings fix the wrapper: escape character, dash delimiter, escape length 10, escape times 2. `build_prompts` returns objects. The line that becomes the sent text is `self.prompts = [pi_prompt["prompt"] for pi_prompt in self.pi_prompts]`. That `prompt` field is the body. A non-Full class caps how many bodies are kept (`soft_probe_prompt_cap`) after a seeded shuffle. The detector trigger is `attack_rogue_string` from the same built object, stored on the attempt before the call. The module points at the PromptInject framework writeup at https://openreview.net/forum?id=qiaRo_7Zmug and tags the probes `owasp:llm01`. Source opened: https://raw.githubusercontent.com/NVIDIA/garak/main/garak/probes/promptinject.py

So a garak "probe prompt body" is not the system prompt and not the detector. It is one fully assembled user string: harmless-looking task text, plus a delimiter, plus a hijack instruction whose success condition is a known rogue string.

C. A manual canary probe uses the same idea with one body per pattern

Prefactor's prompt-injection tester runs 13 patterns in 5 families. You copy one attack, send it to your agent, and paste the reply. Scoring looks in the browser for a marker the attack told the model to emit only if it complied. Their own limit is stated on the page: a pass means the agent held against this probe, not that the agent is safe. A reply from a different prompt scores wrong. Goal-hijacking patterns are imported from the PromptInject set (ignore-and-say, ignore-and-print, uppercase print, nevermind, screaming stop), which matches the five attack names in the garak file above. The other families on that page are role hijack, data exfiltration, encoded payloads, and indirect injection through a retrieved document or a tool result. Source: https://prefactor.tech/tools/prompt-injection-test

D. What that body is trying to change

OWASP labels this LLM01:2025 Prompt Injection. A prompt injection is user-supplied input that changes the model's behavior or output in a way you did not intend. Direct injection is that text in the user's own message. Indirect injection is the same kind of instruction sitting in something the model reads later (a page, a file, a retrieved chunk). OWASP treats jailbreaking as a form of prompt injection whose aim is to make the model drop safety rules. They say RAG and fine-tuning do not fully remove the issue, and that there may be no foolproof prevention because the model does not keep instructions and data in separate channels. Mitigation item 7 is regular adversarial testing and attack simulation, treating the model as an untrusted user. Source: https://raw.githubusercontent.com/OWASP/www-project-top-10-for-large-language-model-applications/main/2_0_vulns/LLM01_PromptInjection.md

Helix's field guide (reviewed 4 October 2026) uses the same split and adds the operational shape of a run: scope, an automated baseline (known jailbreak corpora, injection patterns, extraction probes), then human attacks against the real prompts, tools, and retrieval, then evidence that records the exact input, output, and side effects, then a fix and a retest. They name garak, Microsoft PyRIT, and promptfoo as the open baseline. They also say a clean automated run means those known attacks were absent, not that the system is safe. Source: https://helix.ml/cyber-security/llm-red-teaming

E. The "red" part is the regression case, and the body must be copied unchanged

Braintrust's article separates the red-team engagement from the lasting test. A report of attacks that worked once does not keep working after a prompt edit, a model swap, or a tool-permission change. Their rule: each confirmed attack becomes a labeled case. The input must keep the adversarial text exactly, including whitespace, encoding, conversation history, retrieved content, and tool responses. The expected field is the safe behavior (refusal, fallback, or a constrained action), not a copy of the unsafe reply. A scorer then returns pass or fail for that one behavior. A regression on a safety-critical case should block the merge. They also say the product does not generate the attacks. The body comes from a human tester, a vendor report, a scanner such as garak, or a production incident. Source: https://www.braintrust.dev/articles/llm-red-teaming-regression-suite

That is the practical reading of "red-test probe prompt body": the body is the frozen attack string. The red test is the assertion that this string must not produce the bad outcome. If the test passes on the still-broken build, it is not pinning the bug.

F. A different industry use of "test prompt" is a one-line human checklist, not an attack

Testpad uses "test prompt" for manual exploratory testing. The body is one outcome line, such as "Login works for valid email and password," and it deliberately leaves out click-by-click steps. They say to drop filler like "verify that," to combine action and outcome, and to avoid pinning the exact error string so a copy change does not break the test. That is a prompt to a human tester. It is not the garak `prompt` field and not an injection payload. Source: https://testpad.com/test-prompts/

G. What this run did not verify

These URLs were requested and did not return usable text, so nothing above relies on them: https://socket.dev/pypi/package/redprobe (challenge page only), https://pypi.org/project/redprobe/ (client challenge), https://reference.garak.ai/en/latest/garak.probes.promptinject.html (empty body), https://www.jamesshore.com/v2/books/aoad2/red_green_refactor (404), https://martinfowler.com/bliki/TestDrivenDevelopment.html (request failed), https://huggingface.co/datasets/alib011/llm-red-team-probes (request failed). A search snippet described a PyPI package named redprobe whose YAML case has a required `prompt` field equal to the text sent to the model. That snippet was not confirmed by an opened page in this run.

No files were written for this report.

H. Простыми словами

Зонд — это контрольный вопрос к модели. Тело промпта — сам текст этого вопроса, целиком, как его отправляют. Красный тест — проверка, которая на сломанной версии обязана загореться красным. Если она зелёная ещё до починки, она ничего не стережёт. Фраза из задания не нашлась как имя одного стандарта. Это три обычные части одной проверки: атакующий текст, пробник, который его шлёт, и тест, который должен падать, пока дыра жива.

🎙 Майк: идём верно, если тело пробы хранить дословно и требовать красный прогон на сломанной версии. Вопрос: какой один зонд ты готов заморозить первым — угон инструкции или утечку скрытого промпта?
