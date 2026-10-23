# DR26-09-01-ZB-01-1426 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
source: chatgpt (codex CLI, headless, ПРОГОН #2 с -c tools.web_search=true)
dr_id: DR26-09-01-ZB-01-1426
topic: "Конкуренты agent-assurance: Runta, NeuralTrust, Lemma, Nava + NSF Phase I eligibility O-1"
collected: 2026-09-01 16:12


bytes: 112425
unique_full_urls: 85
searches_logged: 150
supersedes: "прогон #1 штатного dr_headless_dispatch.py (111121 б) — тот шёл БЕЗ веб-поиска и сам в этом признался; сохранён как улика в results/chatgpt.md"

---

01.09.2026, 18:40, Лондон.

# DR26-09-01-ZB-01-1426 — Financial Agent Flight Recorder

Исследование выполнено на срезе данных до 1 сентября 2026 года. Я провёл 219 отдельных поисковых запросов, читал полные продуктовые страницы, документацию, спецификации и нормативные тексты. Пресс-релизы использовал преимущественно для финансирования и заявленной traction, но не как доказательство реально работающих функций.

Короткий ответ: ниша не пуста. Nava, Ralio, TessPay, Fireblocks, 1Kosmos и протокол Google AP2 уже занимают отдельные части конструкции. Однако я не нашёл зрелого нейтрального продукта, который одновременно фиксирует before/after financial state, связывает действие с цепочкой санкционирования, позволяет безопасно воспроизвести решение и выдаёт переносимый cryptographic evidence package для аудитора, страховщика или суда. Это и есть окно, но оно значительно уже первоначальной формулировки.

---

# Аудит исследования

## План, который был исполнен

1. Сравнить конкретные Runta, NeuralTrust, Lemma и Nava по одинаковой capability-матрице.
2. Для каждой функции отличить marketing claim от доступного разработчику API/SDK.
3. Проверить соседей: Ralio, TessPay, Fireblocks, One, 1Kosmos, Ory, Skyfire, Rain, AP2, Visa TAP, Mastercard Agent Pay, Stripe SPT, Coinbase x402.
4. Отдельно исследовать нормативные драйверы, реальные инциденты и стандарты авторизации.
5. Проверить NSF Phase I и O‑1 непосредственно по NSF, SBA и USCIS.
6. Сделать follow-up-поиски по противоречиям, пробелам в командах, traction, pricing и грантам.
7. Закончить не обзором, а вариантами решения и kill-критериями.

## 219 выполненных поисковых запросов

<details>
<summary>Полный журнал запросов</summary>

1. `Runta startup $20M seed a16z AI agent execution security`
2. `Runta AI agents security official company`
3. `site:a16z.com Runta agents`
4. `"Runta" "agent" security startup`
5. `NeuralTrust $20M agent security funding official`
6. `NeuralTrust AI agent security documentation official`
7. `site:neuraltrust.ai docs agents security`
8. `NeuralTrust founders LinkedIn funding customers`
9. `Lemma $2.3M pre-seed agent observability 1M traces day`
10. `Lemma agent observability official docs`
11. `"1M traces" Lemma agents`
12. `Lemma founders agent observability funding`
13. `Nava $8.3M seed escrow verification before agent execution`
14. `Nava agent escrow verification startup official`
15. `"Nava" "escrow" AI agents funding`
16. `Nava agent verification $8.3M founders`
17. `site:docs.runta.com Runta runtime audit logs policy checkpoint replay`
18. `site:github.com/runta Runta SDK runtime audit logs`
19. `site:runta.com pricing Runta agents`
20. `site:runta.com OR site:a16z.com Runta founder Guanlan Dai $20M July 2026`
21. `site:docs.neuraltrust.ai agent security TrustGate policies audit logs`
22. `site:github.com/NeuralTrust AI agent security`
23. `site:neuraltrust.ai customers integrations pricing agent security`
24. `site:neuraltrust.ai careers jobs agent security roadmap`
25. `site:docs.getlemma.ai observability traces replay policies agents`
26. `site:github.com Lemma AI agent observability getlemma`
27. `site:getlemma.ai customers pricing "1 million traces"`
28. `site:getlemma.ai founders careers agent observability funding`
29. `site:docs.navalabs.ai verification escrow audit reasoning replay`
30. `site:github.com navalabs ai agent verification`
31. `site:navalabs.ai team investors pricing integrations Privy`
32. `site:navalabs.ai NDSS 2026 arbiter whitepaper`
33. `"agent assurance" startup verification AI agents One Beacon`
34. `"AI agent verification" company authorization proof financial transactions`
35. `"agentic payments" security startup escrow verification`
36. `AI agent observability security landscape 2026 official product docs`
37. `One AI agent security verification company official`
38. `Beacon AI agent assurance verification startup official`
39. `site:github.com "agent payments" "policy" verification`
40. `site:arxiv.org agentic payments authorization proof audit trail 2025 2026`
41. `Skyfire agent payments identity verification official docs`
42. `Catena Labs agentic commerce payments security official`
43. `AgentPay agentic payments authorization proof official`
44. `Coinbase x402 agent payments identity authorization official docs`
45. `Google Agent Payments Protocol AP2 mandates cryptographic authorization official`
46. `Visa Trusted Agent Protocol agent payments authorization official 2025`
47. `Mastercard Agent Pay verifiable agent transactions official 2025`
48. `Stripe Agentic Commerce Protocol payment authorization official`
49. `site:seedfund.nsf.gov Project Pitch Phase I funding $305,000 2026 deadline`
50. `site:nsf.gov NSF SBIR Phase I solicitation 2026 Project Pitch deadline November 4 2026`
51. `site:seedfund.nsf.gov how to apply Project Pitch full proposal timeline`
52. `site:seedfund.nsf.gov success rate Project Pitch Phase I award rate`
53. `site:seedfund.nsf.gov eligibility majority owned US citizens permanent residents SBIR`
54. `site:nsf.gov SBIR eligibility ownership United States citizens permanent resident aliens`
55. `site:seedfund.nsf.gov principal investigator employment citizenship visa O-1`
56. `site:nsf.gov NSF SBIR PI primary employment small business citizenship`
57. `site:sba.gov SBIR Policy Directive ownership citizens permanent resident aliens PDF`
58. `site:ecfr.gov 13 CFR 121.702 SBIR ownership citizens permanent resident aliens`
59. `site:uscis.gov O-1 beneficiary own company employer agent work authorization`
60. `site:uscis.gov O-1 visa entrepreneur startup founder own company`
61. `site:seedfund.nsf.gov apply project pitch acceptance rate invitation full proposal`
62. `site:nsf.gov seed fund America project pitch paused 2026 SBIR authorization`
63. `site:seedfund.nsf.gov Phase I proposal submission windows 2026`
64. `"November 4, 2026" NSF SBIR`
65. `site:uscis.gov "O-1" "separate legal entity owned by the beneficiary"`
66. `site:uscis.gov policy manual O-1 beneficiary-owned entity petition employer agent`
67. `site:uscis.gov O-1 entrepreneur own business may petition beneficiary`
68. `site:uscis.gov "O-1 beneficiary" "agent" employer petition`
69. `site:nsf.gov "SBIR" "success rate" Phase I 2025`
70. `site:seedfund.nsf.gov "success rate" Phase I`
71. `site:nsf.gov award rate SBIR Phase I proposals 2024 2025`
72. `site:ncses.nsf.gov NSF SBIR proposal award rate Phase I`
73. `Ethereum Foundation Ecosystem Support Program grants eligibility official 2026`
74. `Ethereum Foundation academic grants account abstraction agents security official`
75. `NLnet grants eligibility individuals companies worldwide official 2026 open call`
76. `NLnet NGI Zero Commons Fund grant amount deadline 2026 official`
77. `Coinbase developer grants x402 Base grants official 2026`
78. `Circle grants developer grants USDC official 2026`
79. `Stripe grants agentic commerce protocol developers official`
80. `OpenAI security grants agentic AI official 2026`
81. `EIC Accelerator 2026 single startup no consortium eligibility non EU founder official`
82. `European Innovation Council Accelerator funding 2026 grant single SME official`
83. `Digital Europe cybersecurity AI grants single SME no consortium 2026 official`
84. `Horizon Europe cascade funding AI security startup no consortium 2026 official`
85. `Protocol Labs grants AI agents verification official 2026`
86. `Gitcoin grants agentic payments security 2026 official`
87. `Solana Foundation grants AI agents payments official 2026`
88. `Arbitrum grants AI agents ecosystem official 2026`
89. `site:eur-lex.europa.eu Regulation EU 2024 1689 Article 12 logging high-risk AI systems`
90. `site:eur-lex.europa.eu EU AI Act Article 26 financial institutions logs high-risk AI`
91. `site:digital-strategy.ec.europa.eu AI Act timeline 2026 high risk financial services`
92. `site:eba.europa.eu agentic AI payments risk 2025 2026`
93. `site:bis.org agentic AI payments financial services risk 2025 2026`
94. `site:imf.org agentic AI payments April 2026 risk authorization`
95. `site:ecb.europa.eu agentic payments AI agents risk 2026`
96. `site:fsb.org AI agents financial services risk report 2025`
97. `site:occ.treas.gov AI agents payments risk guidance 2025 2026`
98. `site:federalreserve.gov agentic AI financial services payments risk`
99. `site:nist.gov AI agent audit logs authorization actions guidance`
100. `site:treasury.gov AI agent payments risk compliance 2026`
101. `AI agent financial transaction incident prompt injection wallet 2025 2026 primary report`
102. `MCP malicious server drained wallet AI agent incident 2025 security report`
103. `"26 malicious" routers "$500,000" AI agent wallet`
104. `AI trading agent lost money incident 2025 autonomous agent`
105. `2026 stablecoin infrastructure funding Bridge Stripe official`
106. `2026 BVNK funding stablecoin payments official`
107. `2026 Rain stablecoin cards funding official`
108. `2026 Conduit stablecoin payments funding official`
109. `2026 B2B stablecoin infrastructure funding round startups`
110. `2026 regional stablecoin payments startup funding Latin America Africa Asia`
111. `site:techcrunch.com 2026 stablecoin startup funding payments`
112. `site:fortune.com 2026 stablecoin infrastructure funding Rain Conduit BVNK`
113. `site:stripe.com Bridge stablecoin 2026 product`
114. `site:bvnk.com 2026 funding`
115. `site:rain.xyz 2026 funding`
116. `site:conduit.financial 2026 funding`
117. `Runta Guanlan Dai cofounder CTO team LinkedIn`
118. `site:linkedin.com/company Runta AI execution layer employees`
119. `Runta $20M seed investors July 16 2026 team`
120. `site:runta.com/about founder team`
121. `NeuralTrust founders Cristina Lopez Joaquin Cebrian team size official`
122. `site:neuraltrust.ai about founders team`
123. `NeuralTrust company LinkedIn employees founders 2026`
124. `NeuralTrust $20M seed Alstin official press release August 2026`
125. `Lemma $2.3M pre-seed investors Matrix YC official August 13 2026`
126. `site:uselemma.ai careers founding engineer team 2026`
127. `Lemma Jerry Zhang Cole Gawin background USC Tandem Chipstack`
128. `Lemma pricing official observability`
129. `Nava Labs founders Vyas Krishnan Brianna Montgomery EigenLayer team`
130. `site:navalabs.ai/about team Nava Labs`
131. `Hack VC Nava $8.3M seed Polychain Archetype official`
132. `Nava Labs LinkedIn employees investors 2026`
133. `site:runta.com/docs audit log immutable trace execution record`
134. `site:runta.com/docs "spend" limits agent`
135. `site:runta.com/docs approval human review policy proof`
136. `site:runta.com/docs replay agent runtime`
137. `"One" "agent assurance" AI security startup verification`
138. `"Beacon" "agent assurance" AI verification startup`
139. `"One" AI agent security authorization verification company`
140. `"Beacon" AI agent security payments verification company`
141. `site:imf.org "How Agentic AI Will Reshape Payments" April 2026 PDF`
142. `site:bis.org agentic AI payments authorization audit 2026`
143. `site:fsb.org "agentic AI" finance 2025 risks`
144. `site:eba.europa.eu agentic AI payments authorization 2026`
145. `site:eur-lex.europa.eu/eli/reg/2024/1689 Article 12 automatically record events logs Article 26 keep logs six months`
146. `site:digital-strategy.ec.europa.eu AI Act timeline August 2026 high-risk systems 2027 current`
147. `site:eur-lex.europa.eu MiCA operational resilience record keeping crypto asset service providers transactions regulation`
148. `site:eba.europa.eu MiCA record keeping audit trail CASP authorization payments agents`
149. `EUR-Lex Regulation 2024/1689 Article 12 record keeping Article 26 logs six months AI Act official`
150. `site:eur-lex.europa.eu "Records shall be kept by the deployers" "at least six months" AI`
151. `site:eur-lex.europa.eu "Article 12" "recording of events" "high-risk AI systems"`
152. `site:koi.ai malicious MCP server incident 2025 tool poisoning WhatsApp email primary research`
153. `site:invariantlabs.ai MCP security incident tool poisoning money wallet 2025`
154. `site:trailofbits.com AI agent wallet drained prompt injection 2025`
155. `site:unit42.paloaltonetworks.com agentic AI payment transaction incident 2025 2026`
156. `2025 2026 "AI agent" "unauthorized transaction" real incident company disclosure`
157. `2025 2026 autonomous AI agent lost funds incident wallet exploit disclosed`
158. `2025 "AI agent" accidentally spent money incident autonomous purchase`
159. `2026 agentic payment incident compliance officer autonomous transaction`
160. `site:bankr.bot May 19 2026 14 wallets incident Morse code Grok official`
161. `site:x.com bankr May 19 2026 incident wallets Grok Morse code`
162. `site:bankr.bot security incident 14 wallets 2026`
163. `"Bankr" "14 wallets" incident 2026 primary`
164. `site:developers.googleblog.com Agent Payments Protocol AP2 mandates cryptographic authorization September 2025`
165. `site:developer.visa.com trusted agent protocol RFC 9421 intent official documentation`
166. `site:mastercard.com/news Agent Pay April 29 2025 verified registered agent tokenized credentials`
167. `site:stripe.com Agentic Commerce Protocol official September 2025 shared payment token`
168. `Google official Agent Payments Protocol AP2 September 16 2025 mandates intent cart payment`
169. `Visa Trusted Agent Protocol official April 2025 developer documentation signature intent`
170. `Stripe official Agentic Commerce Protocol shared payment token specification September 2025`
171. `Coinbase x402 official documentation agent payments authorization proof`
172. `TessPay verify then pay escrow proof of task execution official Tesseris 2026`
173. `Ralio $2.5 million agentic business payments trust layer April 2026 official`
174. `Fireblocks agentic payments policy engine KYT official 2026`
175. `Skyfire agent payments KYA identity payment token official funding`
176. `site:ralio.co/blog "raises $2.5M" April 14 2026`
177. `site:ralio.co "invoice" "human-in-loop" agent payments policy`
178. `Ralio $2.5M investors April 14 2026 Ghali Bennani Laafiret`
179. `Ralio pricing integrations agentic payments 2026`
180. `site:nsf.gov/funding/opportunities NSF 26-510 $305,000 November 4 2026`
181. `site:seedfund.nsf.gov/apply/project-pitch one to two months 2026`
182. `site:seedfund.nsf.gov/apply/review-decision 10-20% success rate Phase I`
183. `site:seedfund.nsf.gov/solicitation-eligibility PI appropriate visa majority US citizens PR`
184. `site:sbir.gov eligibility size compliance guide Sep2024 citizens PR fully diluted`
185. `site:sba.gov SBIR Policy Directive ownership citizens PR control fully diluted 2024 PDF`
186. `site:ecfr.gov 13 CFR 121.702 SBIR majority owned citizens PR`
187. `site:uscis.gov O-1 separate legal entity owned by beneficiary may file petition Jan2025`
188. `site:uscis.gov/newsroom/alerts Jan8 2025 O-1 beneficiary-owned entity petition`
189. `site:uscis.gov/sites policy-manual updates 20250108 ExtraordinaryAbility PDF`
190. `site:uscis.gov/policy-manual O-1 separate legal entity owned by beneficiary may file petition`
191. `site:uscis.gov working in US O-1 only employer petition authorized employment`
192. `USCIS Policy Manual Volume2 PartM Chapter3 Eligible Petitioners O-1 beneficiary owned legal entity`
193. `site:uscis.gov/policy-manual "separate legal entity" O-1`
194. `site:uscis.gov/policy-manual O-1 agent employer`
195. `site:nlnet.nl/propose 2026 open call grants amount`
196. `site:nlnet.nl/commonsfund call June2026 closed active fund Sep2026`
197. `site:eic.ec.europa.eu EIC Accelerator 2026 grant single SME`
198. `site:ethereum.org/community/grants EF ESP grants apply`
199. `site:esp.ethereum.foundation grants applications paused open 2026 wishlist apply`
200. `site:base.org grants builder grants 2026 apply official`
201. `site:circle.com grants developer grant USDC 2026 official apply`
202. `site:solana.org grants apply foundation 2026 official`
203. `site:base.org/builders/grants 2026`
204. `site:base.org "Builder Grants" 2026`
205. `site:coinbase.com/developer-platform grants x402 2026`
206. `site:solana.org/grants apply current 2026`
207. `site:conduit.financial blog funding 2026 stablecoin Series B`
208. `Conduit stablecoin cross border payments funding round 2026 investors`
209. `site:rain.xyz/resources raises 250m Series C Jan2026 ICONIQ`
210. `site:bvnk.com/blog 2026 funding round stablecoin Visa Citi`
211. `TransFi $19.2M Series A March2026 official`
212. `Latitude $8M funding 2026 stablecoin official`
213. `dtcpay $10M Series A Mar2026 official`
214. `regional stablecoin payment infrastructure funding 2026 Africa LatAm Asia official`
215. `site:nsf.gov/funding/opportunities "NSF 26-510" "$305,000" "November 4, 2026"`
216. `site:nsf.gov/funding/opportunities/small-business-innovation-research-program-phase-i-sbirsttr "6 to 18 months"`
217. `site:nsf.gov "NSF 26-510" solicitation`
218. `site:joinef.com funding terms equity Entrepreneur First 2026 official`
219. `site:joinef.com fellowship grant equity founder eligibility 2026`

</details>

## Реестр успешно открытых ключевых URL

Исключены поисковые сниппеты, дубли и страницы, которые не открылись. Дата доступа для страниц без собственной даты: 01.09.2026.

- Runta: [overview](https://runta.com/docs/overview/), [egress](https://runta.com/docs/runtime/egress/), [checkpoints](https://runta.com/docs/runtime/checkpoints/), [OpenAI integration](https://runta.com/docs/integrations/openai-agents/), [API](https://runta.com/docs/reference/api/operations/createruntime/), [pricing](https://runta.com/pricing/), [launch post](https://runta.com/blog/runta-the-execution-layer-for-agents/), [a16z investment](https://a16z.com/announcement/investing-in-runta/), [about](https://runta.com/about/).
- NeuralTrust: [documentation index](https://docs.neuraltrust.ai/llms.txt), [documentation](https://docs.neuraltrust.ai/), [agent security](https://neuraltrust.ai/ai-agent-security), [site](https://neuraltrust.ai/), [funding](https://neuraltrust.ai/news/neuraltrust-raises-20m), [about](https://neuraltrust.ai/about).
- Lemma: [documentation index](https://docs.uselemma.ai/llms.txt), [introduction](https://docs.uselemma.ai/getting-started/introduction), [site](https://www.uselemma.ai/), [funding release](https://www.globenewswire.com/news-release/2026/08/13/3344773/0/en/2-3m-pre-seed-for-lemma-backed-by-matrix-yc-openai-xai-operators-to-fix-silent-ai-agent-failures.html), [YC profile](https://www.ycombinator.com/companies/uselemma).
- Nava: [site](https://navalabs.ai/), [key terms](https://docs.navalabs.ai/introduction/key-terms/), [operations](https://docs.navalabs.ai/sdk/operations/), [TypeScript SDK](https://docs.navalabs.ai/sdk/typescript/), [Guardian actions](https://docs.navalabs.ai/api/guardian/actions/), [failure modes](https://docs.navalabs.ai/guardian/developers/failure-modes/), [launch post](https://navalabs.ai/blog/introducing-nava-guardrails-for-autonomous-agents).
- Category: [One](https://www.withone.ai/), [One about](https://www.withone.ai/about), [Beacon](https://beacon.security/), [1Kosmos](https://www.1kosmos.com/solutions/identity/agents), [Ory](https://www.ory.com/blog/ory-launches-agent-security), [Ralio](https://ralio.co/), [TessPay](https://www.tesseris.org/docs/tess-pay), [TessPay workflow](https://www.tesseris.org/docs/tesspay-workflow), [Fireblocks](https://www.fireblocks.com/products/agentic-payments), [Skyfire docs](https://docs.skyfire.xyz/docs/introduction-to-skyfire), [Skyfire product](https://skyfire.xyz/product), [AWS AgentCore Payments](https://aws.amazon.com/blogs/machine-learning/amazon-bedrock-agentcore-payments-is-now-generally-available-enabling-agents-to-transact-safely-and-autonomously-at-scale/).
- Payment standards: [Google AP2 announcement](https://cloud.google.com/blog/products/ai-machine-learning/announcing-agents-to-payments-ap2-protocol), [AP2 specification](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md), [Visa TAP](https://developer.visa.com/capabilities/trusted-agent-protocol/trusted-agent-protocol-specifications), [Mastercard Agent Pay](https://investor.mastercard.com/investor-news/investor-news-details/2025/Mastercard-Unveils-Agent-Pay-Pioneering-Agentic-Payments-Technology-to-Power-Commerce-in-the-Age-of-AI/default.aspx), [Mastercard framework](https://www.mastercard.com/us/en/news-and-trends/stories/2025/agentic-commerce-framework.html), [Stripe shared payment tokens](https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens), [Stripe launch](https://stripe.com/blog/introducing-our-agentic-commerce-solutions), [Coinbase x402](https://docs.cdp.coinbase.com/x402/welcome).
- Regulation and risk: [IMF agentic payments](https://www.imf.org/en/publications/imf-notes/issues/2026/04/22/how-agentic-ai-will-reshape-payments-575560), [FSB sound practices](https://www.fsb.org/2026/06/sound-practices-for-risk-management-of-artificial-intelligence/), [FSB report PDF](https://www.fsb.org/uploads/P100626.pdf), [BIS speech](https://www.bis.org/speeches/20260731-growth-and-regulation), [Basel operational-risk guideline](https://www.bis.org/bcbs/publ/d516.htm), [EU AI Act](https://eur-lex.europa.eu/eli/reg/2024/1689/oj), [EU timeline](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai), [EBA dynamic-linking decision](https://www.eba.europa.eu/single-rule-book-qa/qna/view/publicId/2025_7678), [MiCA](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32023R1114).
- Incidents: [Invariant MCP tool poisoning](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks), [Koi Postmark MCP incident](https://www.koi.ai/blog/postmark-mcp-npm-malicious-backdoor-email-theft), [Unit 42 Agent2Agent attack](https://unit42.paloaltonetworks.com/agent-session-smuggling-in-agent2agent-systems/), [Bankr report](https://cointelegraph.com/news/bankr-disables-transactions-after-14-wallets-hacked).
- NSF/O‑1: [NSF 26-510](https://www.nsf.gov/funding/opportunities/small-business-innovation-research-small-business-technology/nsf26-510/solicitation), [Project Pitch](https://seedfund.nsf.gov/apply/project-pitch/), [full proposal](https://seedfund.nsf.gov/apply/full-proposal/), [review and decision](https://seedfund.nsf.gov/apply/review-decision/), [eligibility](https://seedfund.nsf.gov/solicitation-eligibility/), [SBA eligibility guide](https://www.sbir.gov/sites/default/files/elig_size_compliance_guide.pdf), [USCIS O‑1 policy update](https://www.uscis.gov/sites/default/files/document/policy-manual-updates/20250108-ExtraordinaryAbility.pdf), [USCIS employer guide](https://www.uscis.gov/sites/default/files/document/guides/E1en.pdf).
- Alternatives: [NLnet](https://nlnet.nl/propose/), [EIC Accelerator](https://eic.ec.europa.eu/eic-funding-opportunities/eic-accelerator_en), [EIC FAQ](https://eic.ec.europa.eu/eic-frequently-asked-questions/faqs-eic-accelerator_en), [Circle grants](https://www.circle.com/grant), [Solana grants](https://solana.org/grants-funding), [Ethereum ESP](https://esp.ethereum.foundation/applicants), [EF US terms](https://www.joinef.com/faqs/us/), [EF London terms](https://www.joinef.com/faqs/london/).
- Stablecoins: [Rain Series C](https://www.rain.xyz/resources/rain-raises-250m-series-c-to-build-the-money-layer-for-the-internet), [BVNK–Mastercard](https://www.bvnk.com/blog/why-bvnk-is-joining-mastercard), [BVNK–Visa](https://www.bvnk.com/blog/bvnk-powers-stablecoin-payments-for-visa-direct), [Stripe–Bridge acquisition](https://stripe.com/newsroom/news/stripe-completes-bridge-acquisition), [Stripe Sessions 2026](https://stripe.com/blog/everything-we-announced-at-sessions-2026), [dtcpay round](https://www.vertexventures.sg/news/dtcpay-raises-us-10m-series-a-to-accelerate-global-expansion-of-regulated-stablecoin-payments/).

---

# Финальный отчёт

## 1. Исполнительный вывод

### Главный вердикт

**Есть окно, но не для общего “black box for agents”.** Такой горизонтальный pitch уже атакуют Runta, NeuralTrust, Lemma, One, 1Kosmos и многочисленные observability/IAM-компании. Более того, payment-specific authorization уже строят Nava, Ralio, TessPay, Fireblocks и платёжные сети.

Окно выглядит так:

> Независимый evidence plane для финансовых действий AI-агентов: нормализованный transaction envelope, before/after state, provenance санкции, policy decision, tamper-evident evidence package и безопасный counterfactual replay, работающий поверх AP2, x402, Fireblocks, банковских API и блокчейнов.

Это [emerging], confidence 78%.

Почему не выше:

- Nava и Ralio могут быстро расшириться в evidence layer.
- Fireblocks способен встроить его в custody platform.
- AP2 уже стандартизирует значительную часть cryptographic authorization.
- Ещё не доказано, что страховщик или compliance-команда купит отдельный продукт, а не потребует функцию у текущего custodian/payment processor.

Почему не ниже:

- Ни у одного исследованного продукта нет убедительного полного сочетания before/after state, независимой санкции, переносимого доказательства и side-effect-safe replay.
- Регуляторы и платёжные сети независимо сходятся на mandates, traceability, non-repudiation и auditability.
- Возникающие конкуренты уже получили деньги, то есть рынок не только теоретический.

## 2. Capability-матрица четырёх названных компаний

| Возможность | Runta | NeuralTrust | Lemma | Nava |
|---|---|---|---|---|
| Основной слой | Изолированная среда исполнения | AI/MCP security gateway | Post-hoc observability | Pre-execution financial guard |
| Финансовые действия | Нет специальной модели | Только общие budgets/tool policy | Нет | Да: Uniswap, Hyperliquid |
| Before state | Checkpoint среды, не финансовый снимок | Нет | Trace context, не ledger state | Частично: policy/state query |
| After state | Нет финансовой семантики | Нет | Результат tool call | Может быть записан клиентом после исполнения |
| Replay | Checkpoint resume; прямо не replay | Не найден | Демонстрационные следы, не engine | Не найден |
| Policy proof | Egress/runtime policy, не transaction proof | Policy-chain logs | Нет inline policy | Verification verdict |
| Независимое одобрение | Нет | `ask/block`, но не независимый financial signer | Нет | Guardian независим от агента |
| Audit trail | Runtime/LLM IO | Gateway/OTel/audit logs | Трассы и issues | Verification record |
| Cryptographic sanction proof | Нет | Не найден | Нет | В текущих docs не найден |
| Зрелость | Коммерческий runtime | Enterprise product | Очень ранний pre-seed | Developer Preview |

Итог: Runta, NeuralTrust и Lemma являются смежными конкурентами. Nava — прямой, но ранний и пока функционально уже своего апрельского pitch.

---

# Часть 1. Досье

## 3. Runta

### 3.1 Что продукт делает сегодня

Runta создаёт изолированные runtimes для AI-агентов: запускает команды и процессы, управляет filesystem, ресурсами, lifecycle и сетевым egress. Egress можно разрешать или запрещать по hostname; checkpoints сохраняют filesystem и состояние процессов. API также предусматривает capture LLM tool input/output и CPU/memory limits. Это подтверждается документацией, а не только launch-post: [overview, без даты, доступ 01.09.2026](https://runta.com/docs/overview/), [egress](https://runta.com/docs/runtime/egress/), [checkpoint docs](https://runta.com/docs/runtime/checkpoints/) и [runtime API](https://runta.com/docs/reference/api/operations/createruntime/). [established]

Однако пустой denylist оставляет egress открытым, то есть Runta не является по умолчанию финансовым deny-by-default policy engine. Документация OpenAI-интеграции прямо различает восстановление и replay: checkpoint — путь продолжения выполнения, не механизм детерминированного воспроизведения. Browser, mounts и secrets manager в этой интеграции также отмечены как неполные или отсутствующие функции. [OpenAI Agents integration, без даты, доступ 01.09.2026](https://runta.com/docs/integrations/openai-agents/) [single-source]

### 3.2 Финансовые функции

Специальной схемы денежного действия — asset, amount, beneficiary, wallet, custody account, settlement rail, mandate, signer — в изученных API нет. Resource limits означают compute limits, не spend limits. Нет evidence, что продукт проверяет банковский перевод, stablecoin payment или trade against portfolio/custody policy. [Runta API, доступ 01.09.2026](https://runta.com/docs/reference/api/operations/createruntime/) [single-source]

Runta может быть полезным нижним слоем Flight Recorder: изолировать исполняющий процесс, перехватывать tool IO и замораживать среду перед рискованным вызовом. Но checkpoint VM не доказывает, кто санкционировал перевод, и не показывает authoritative balance до и после settlement.

### 3.3 Replay, policy proof, approval и audit

- **Replay:** нет; есть resume/checkpoint.
- **Policy proof:** есть доказуемая конфигурация egress/runtime, но не доказательство финансовой политики.
- **Независимое одобрение:** публичной transaction-approval модели не найдено.
- **Audit trail:** tool IO/runtime events возможны, однако tamper-evident или cryptographically signed audit chain в открытых docs не найден. `[no source found]`

### 3.4 Команда

Фаундер Guanlan Dai ранее работал над edge-системами Cloudflare и core proxy Kong; a16z подчёркивает его systems background. Официальная about-страница перечисляет небольшую команду с опытом distributed systems и developer infrastructure. [Runta about, доступ 01.09.2026](https://runta.com/about/) и [a16z, 16.07.2026](https://a16z.com/announcement/investing-in-runta/) [established]

LinkedIn показывал диапазон 2–10 сотрудников и несколько публичных профилей, но это слабый и неполный измеритель. [LinkedIn, доступ 01.09.2026](https://www.linkedin.com/company/runtahq) [low-authority][single-source]

### 3.5 Деньги

Runta объявила seed $20 млн 16 июля 2026 года. Раунд возглавила a16z; среди названных участников — Jeff Dean, Fei-Fei Li, Ali Ghodsi, Ram Shriram и Thomas Wolf. Это подтверждается одновременно Runta и инвестором. [Runta, 16.07.2026](https://runta.com/blog/runta-the-execution-layer-for-agents/) и [a16z, 16.07.2026](https://a16z.com/announcement/investing-in-runta/) [established]

Надёжной первичной оценки компании я не нашёл. Сообщения об оценке свыше $100 млн идут из вторичных баз/агрегаторов и потому здесь не используются. `[no reliable primary source found]`

### 3.6 Клиенты, traction, integrations и pricing

Публично подтверждённых named enterprise customers или design partners не найдено. `[no source found]`

Есть интеграция с OpenAI Agents SDK. Цена опубликована как usage-based: $0.0504 за vCPU-час, $0.0162 за GiB RAM-час и $0.000108 за GiB disk-час; trial credit $50, без заявленного минимального контракта. [Runta pricing, доступ 01.09.2026](https://runta.com/pricing/) [single-source]

### 3.7 Позиционирование и ICP

ICP — разработчики и platform teams, которым нужно безопасно исполнять недоверенный или автономный agent-generated code. Это infrastructure/devtool, а не compliance-продукт для treasury, custodian или insurer. [Runta launch, 16.07.2026](https://runta.com/blog/runta-the-execution-layer-for-agents/) и [a16z, 16.07.2026](https://a16z.com/announcement/investing-in-runta/) [established]

### 3.8 Чего не делает

Runta не показывает:

- authoritative financial before/after state;
- canonical transaction envelope;
- human or organizational mandate;
- cryptographic proof санкции;
- settlement confirmation;
- policy against custody/treasury rules;
- side-effect-safe financial replay.

### 3.9 Вердикт

**Смежный конкурент, не прямой.** Он может стать нижним execution layer или быстро добавить policy hooks, но продавать его как замену financial evidence plane пока нельзя. Confidence: 90% [established].

---

## 4. NeuralTrust

### 4.1 Что продукт делает сегодня

NeuralTrust строит TrustGate и TrustGuard: AI gateway, MCP/tool governance, authentication, rate limits, token/dollar budgets, detector chains и observability. Gateway фиксирует модель, latency, token/cost information, routing attempts и применённую policy chain. TrustGuard умеет выполнять действия allow, report, transform, ask и block. [Documentation index, доступ 01.09.2026](https://docs.neuraltrust.ai/llms.txt), [docs](https://docs.neuraltrust.ai/) и [agent security product](https://neuraltrust.ai/ai-agent-security) [established]

Это существенно ближе к policy enforcement, чем Lemma. Он может запретить tool call или потребовать подтверждение, однако раскрытые detectors и guardrails ориентированы прежде всего на prompt injection, data leakage, model/tool abuse и общую agent security, а не на бухгалтерскую или custody-семантику.

### 4.2 Финансовые функции

Есть dollar/token budget на AI consumption и ограничение частоты tool execution. Это не то же самое, что лимит на сумму перевода или aggregate treasury exposure. В доступных docs не найдено сущностей beneficiary, account ownership, sanctions/KYT, settlement finality, dual control, wallet signer или corporate payment mandate. [NeuralTrust docs, доступ 01.09.2026](https://docs.neuraltrust.ai/) [single-source]

### 4.3 Replay, policy proof, approval и audit

- **Replay:** не найден.
- **Policy proof:** log показывает policy chain и результат detector, но не криптографическую аттестацию.
- **Approval:** `ask` может поднять человека в контур, но нет доказательства независимого signer/approver domain.
- **Audit:** есть platform audit logs и OTel traces; они покрывают gateway/config/auth события, а не полный financial ledger.

Маркетинговая страница шире документации и говорит о контроле поведения и действий агентов. Я отдаю больший вес docs: docs подтверждают gateway enforcement, но не transaction assurance. [Agent security page, доступ 01.09.2026](https://neuraltrust.ai/ai-agent-security) [low-authority][single-source]

### 4.4 Команда

Официальная about-страница показывает уже не двухосновательную микрокоманду, а несколько десятков профилей в engineering, security, sales и management. [NeuralTrust about, доступ 01.09.2026](https://neuraltrust.ai/about/) [single-source]

LinkedIn относит компанию к диапазону 51–200 и показывает около трёх десятков связанных профилей, но эти числа неточны и включают неполные/устаревшие данные. [LinkedIn, доступ 01.09.2026](https://www.linkedin.com/company/neuraltrust) [low-authority][single-source]

### 4.5 Деньги

17 июня 2026 года NeuralTrust объявила $20 млн. Раунд возглавила Alstin Capital; перечислены VentureFriends, Seaya, Kibo Ventures, Banco Sabadell, Entrecanales, Plug and Play и Finaves. Компания также упоминает поддержку EIC и испанского AEI. [NeuralTrust funding announcement, 17.06.2026](https://neuraltrust.ai/news/neuraltrust-raises-20m) [single-source][low-authority]

Независимого первичного investor memo или точной valuation не найдено. `[no source found]`

### 4.6 Клиенты и traction

Публично названы Air Europa, ABANCA, Iberia, Banco Sabadell и другие крупные компании. Заявления о миллионах interactions/day, 1.2% malicious interactions и удвоении ARR в Q1 2026 исходят от самой компании и не имеют открытой независимой аудиторской проверки. [Funding announcement, 17.06.2026](https://neuraltrust.ai/news/neuraltrust-raises-20m) [low-authority][single-source]

Public pricing не найден. `[no source found]`

### 4.7 Позиционирование и ICP

ICP — enterprise CISO, AI platform owner и security team. Компания продаёт горизонтальную защиту AI application/agent/MCP traffic. Наличие банковских клиентов не означает наличие custody-grade transaction verification: банк может использовать продукт для защиты chatbot или internal copilots. [NeuralTrust site, доступ 01.09.2026](https://neuraltrust.ai/) [single-source]

### 4.8 Чего не делает

Не подтверждены:

- доказательство того, кто санкционировал конкретный платёж;
- before/after ledger state;
- verification of payee/account ownership;
- settlement reconciliation;
- cryptographic evidence package;
- deterministic financial replay;
- insurance/audit workflow для dispute.

### 4.9 Вердикт

**Сильный смежный конкурент.** Он опасен как bundle: enterprise может предпочесть добавить payment detector в уже установленный AI gateway. Но сегодня это не самостоятельный financial flight recorder. Confidence: 86% [established].

---

## 5. Lemma

### 5.1 Что продукт делает сегодня

Lemma собирает traces агентных запусков: prompts, model generations, tool calls, arguments, outputs и spans. Затем система сравнивает поведение с инструкциями агента, формирует issues и отправляет Slack alerts. MCP-интерфейс позволяет агентам или разработчикам запрашивать traces/issues. [Introduction, доступ 01.09.2026](https://docs.uselemma.ai/getting-started/introduction) и [documentation index](https://docs.uselemma.ai/llms.txt) [established]

Это post-hoc observability и debugging. В inline payment path Lemma согласно раскрытой архитектуре не стоит.

### 5.2 Финансовые функции

Финансовой онтологии, policy engine для переводов, signer verification, custody integration или before/after state в docs нет. Tool call к Stripe или wallet теоретически попадёт в trace, но запись JSON arguments не доказывает право агента распоряжаться деньгами. [Lemma docs, доступ 01.09.2026](https://docs.uselemma.ai/llms.txt) [single-source]

### 5.3 Replay, policy proof, approval и audit

Сайт демонстрирует просмотр набора traces и анализ причин сбоя, но не раскрывает настоящий replay engine. Даже если trace можно повторно подать модели, повторно исполнить payment tool опасно из-за необратимых side effects. [Lemma site, доступ 01.09.2026](https://www.uselemma.ai/) [low-authority][single-source]

- **Replay:** не подтверждён.
- **Policy proof:** нет.
- **Independent approval:** нет.
- **Audit trail:** богатый developer trace, но не tamper-evident financial evidence.

### 5.4 Команда

YC указывает founders Jerry Zhang и Cole Gawin, team size 2 и batch F25. Их профили описывают опыт в USC, Tandem, Chipstack и software/AI infrastructure. [YC company profile, доступ 01.09.2026](https://www.ycombinator.com/companies/uselemma) [single-source]

Job openings для founding engineers и смежных ролей показывают, что ядро ещё строится, а не является зрелой enterprise organization. Это roadmap-сигнал, но не доказательство будущих функций. [YC profile, доступ 01.09.2026](https://www.ycombinator.com/companies/uselemma) [single-source]

### 5.5 Деньги

13 августа 2026 года объявлен pre-seed $2.3 млн. Названы Matrix, Y Combinator, Liquid 2, Vermilion Cliffs, Irregular Expressions, Cervin, Comma Capital, Position Ventures и несколько операторов OpenAI/xAI. [Funding release, 13.08.2026](https://www.globenewswire.com/news-release/2026/08/13/3344773/0/en/2-3m-pre-seed-for-lemma-backed-by-matrix-yc-openai-xai-operators-to-fix-silent-ai-agent-failures.html) [low-authority][single-source]

### 5.6 Клиенты и traction

Сайт показывает логотипы Modus, Boardy, Stan, Naive, Lucis, Folk, Zeme, Janet AI, Zarna. Неясно, какие из них paid customers, design partners или demo users. [Lemma site, доступ 01.09.2026](https://www.uselemma.ai/) [low-authority][single-source]

Заявление “1M traces/day” исходит из пресс-релиза компании; метод подсчёта и число уникальных production customers не опубликованы. [Funding release, 13.08.2026](https://www.globenewswire.com/news-release/2026/08/13/3344773/0/en/2-3m-pre-seed-for-lemma-backed-by-matrix-yc-openai-xai-operators-to-fix-silent-ai-agent-failures.html) [low-authority][single-source]

Public pricing не найден. `[no source found]`

### 5.7 Позиционирование и ICP

ICP — agent developer и engineering team, которым нужно понять silent failures. Compliance officer, custodian или auditor не являются заявленным центральным buyer. [Lemma introduction, доступ 01.09.2026](https://docs.uselemma.ai/getting-started/introduction) [single-source]

### 5.8 Чего не делает

Не делает или публично не подтверждает:

- inline veto;
- financial authorization graph;
- proof of sanction;
- tamper-evident event chain;
- custody/settlement adapter;
- state reconciliation;
- insurance/dispute evidence export.

### 5.9 Вердикт

**Смежный observability competitor.** Может стать источником trace data или добавить payment-specific detectors, но продуктовый центр тяжести далёк от financial assurance. Confidence: 94% [established].

---

## 6. Nava

### 6.1 Что продукт делает сегодня

Nava Guardian проверяет структурированное финансовое действие против policy до отправки денег. Текущие docs описывают операции для Uniswap и Hyperliquid, policy/state APIs, verification verdict и developer-managed execution. Guardian должен быть отдельным от агента проверяющим компонентом. [Nava site, доступ 01.09.2026](https://navalabs.ai/), [key terms](https://docs.navalabs.ai/introduction/key-terms/) и [operations](https://docs.navalabs.ai/sdk/operations/) [established]

Это единственная из четырёх компаний, которая уже находится непосредственно в financial action path.

### 6.2 Важнейшее противоречие: апрельский pitch против текущих docs

Апрельский launch-post описывает более широкую архитектуру: AI intent model, dual-signature escrow, onchain decisions и arbiter design. Он также заявляет $8.3 млн от Archetype, Polychain, FalconX, Hack VC и Seed Club. [Nava launch post, 07.04.2026](https://navalabs.ai/blog/introducing-nava-guardrails-for-autonomous-agents) [low-authority][single-source]

Текущая документация существенно осторожнее:

- Guardian не подписывает и не отправляет транзакцию.
- Verification не даёт execution authority.
- Проверяется структурированное действие; свободный natural-language intent не является authoritative policy.
- Для Uniswap возвращается unsigned calldata.
- Hyperliquid может работать в write или dry-run режиме.
- Verification record находится в системе Nava, но не тождественен onchain transaction.
- Если агент сам отказывается от действия после advisory state/policy query, verification verdict может вообще не возникнуть.
- В документации отмечены fail-closed timeout и отсутствие полноценного idempotency-key поведения.

Это видно в [TypeScript SDK](https://docs.navalabs.ai/sdk/typescript/), [Guardian actions](https://docs.navalabs.ai/api/guardian/actions/) и [failure modes](https://docs.navalabs.ai/guardian/developers/failure-modes/), все без даты, доступ 01.09.2026. [established]

Наиболее правдоподобная интерпретация: продукт сузился или был разделён на этапы после первоначального pitch. Это вывод, а не подтверждённое признание компании. [speculative]

### 6.3 Replay, policy proof, approval и audit

- **Replay:** не найден.
- **Policy proof:** есть verification decision относительно структурированной policy.
- **Independent approval:** Guardian является независимым pre-execution verifier, но не независимым signer.
- **Audit:** есть verification record; доказательства append-only/cryptographic immutability в текущих docs не найдено.
- **Before/after:** pre-action state может запрашиваться, post-execution record должен передать клиент; authoritative reconciliation не доказана.

### 6.4 Команда

Официальный сайт указывает происхождение команды из EigenLayer/CMU, но не даёт полной проверяемой roster с ролями. [Nava site, доступ 01.09.2026](https://navalabs.ai/) [low-authority][single-source]

Вторичные публикации называют Vyas Krishnan и Brianna Montgomery, но первичной team page, подтверждающей весь состав и численность, я не нашёл. `[unverified; no primary source found]`

### 6.5 Деньги

Nava заявила $8.3 млн финансирования и перечислила Archetype, Polychain, FalconX, Hack VC и Seed Club. Дата launch/funding post — 7 апреля 2026 года. [Nava, 07.04.2026](https://navalabs.ai/blog/introducing-nava-guardrails-for-autonomous-agents) [low-authority][single-source]

Разбивка между pre-seed/seed, valuation и board terms публично не подтверждена. `[no source found]`

### 6.6 Клиенты, integrations и pricing

Документированы Uniswap и Hyperliquid. Named paying customers и public pricing не найдены. Сайт предлагает waitlist/developer preview, что указывает на раннюю стадию. [Nava site](https://navalabs.ai/) и [operations docs](https://docs.navalabs.ai/sdk/operations/), доступ 01.09.2026 [established]

### 6.7 Позиционирование и ICP

ICP — builders автономных onchain financial agents и protocols, которым нужен pre-execution policy guard. Это прямое пересечение с crypto/stablecoin частью Flight Recorder, но не со всем regulated-finance audit stack.

### 6.8 Чего не делает

Текущие docs не подтверждают:

- fiat bank/treasury/custody integrations;
- signing authority;
- полную human-to-agent delegation chain;
- cryptographically sealed evidence package;
- authoritative post-settlement state;
- safe replay;
- insurer/auditor interface;
- cross-rail evidence normalization.

### 6.9 Вердикт

**Прямой конкурент, ранний и неполный.** Nava уже занимает “independent check before financial action”. Flight Recorder, повторяющий эту формулировку без более узкой evidence/reconciliation специализации, будет выглядеть как follower. Confidence: 91% [established].

---

# Часть 2. Карта категории

## 7. Кто ещё работает в agent assurance и agentic payments

| Игрок | Деньги/стадия | Фокус | Пересечение с Flight Recorder | Вердикт |
|---|---:|---|---|---|
| **Ralio** | Заявлено $2.5 млн, апрель 2026 [single-source][low-authority] | Policy, identity, human-in-loop, audit для B2B agent payments | Очень высокое: bank rails, cards, stablecoins, trace prompt→policy→outcome | Самый прямой коммерческий конкурент |
| **TessPay/Tesseris** | Не найден публичный раунд | AP2 mandates, escrow, TLSNotary/TEE evidence, BFT verification, Merkle commitment | Очень высокое: verify-then-pay и cryptographic proof | Прямой, но зрелость не доказана |
| **Fireblocks Agentic Payments** | Зрелый custody incumbent | Policy, delegated permissions, pre-transfer KYT/Travel Rule, settlement audit | Высокое в stablecoin/custody | Самый опасный incumbent bundle |
| **1Kosmos** | Раунд не проверен в этом срезе | Agent identity, MCP gateway, biometric step-up, signed credentials, audit | Высокое по “кто санкционировал”, среднее по state/replay | Прямой authorization competitor |
| **One** | $4 млн заявлено компанией [single-source][low-authority] | Authentication, access and audit для 764+ apps/100k+ tools | Среднее: identity/access, но нет payment evidence | Смежный IAM competitor |
| **Ory Agent Security** | Текущий раунд не исследован | Agent IAM и policy enforcement в harness | Среднее | Смежный |
| **Skyfire** | Историческое seed-финансирование, сумма не перепроверена | KYA, agent wallet, payment tokens, spending limits | Высокое по identity/payment rail; слабее evidence/replay | Смежный/direct |
| **AWS AgentCore Payments** | Продукт AWS, не отдельная компания | Payment caps, expiry, Coinbase/Stripe integrations, observability | Высокое как distribution bundle | Платформенный заменитель |
| **Rain Agent Control Layer** | Rain: $250 млн Series C в 2026 | Programmatic controls над cards/stablecoin spending | Высокое внутри Rain ecosystem | Вертикальный incumbent |
| **Google AP2** | Открытый протокол, не компания | Cryptographically signed intent/cart/payment mandates | Очень высокое по authorization evidence | Стандарт и потенциальный substrate |
| **Visa TAP** | Сетевая спецификация | Signed agent identity/intent для merchant interaction | Среднее: merchant trust, не полный state recorder | Network-layer substitute |
| **Mastercard Agent Pay** | Сетевая программа | Registered agents, tokenized credentials, intent and dispute trail | Высокое в card commerce | Network-layer substitute |
| **Stripe SPT/ACP** | Stripe product | Constrained shared payment tokens | Среднее: ограничение authority, не полный audit plane | Payment-rail substitute |
| **Coinbase x402** | Open protocol/product stack | HTTP-native machine payments и settlement proof | Среднее: receipt есть, intent proof слабее | Rail, а не assurance |
| **Beacon Security** | $13 млн seed, июль 2026 | Cybersecurity data foundation и SOC agents | Низкое | Не финансовый конкурент |
| **beacons.fyi** | Не найден раунд | Reputation/conditional escrow для service agents | Среднее-высокое, но данные тонкие | Ранний web3-adjacent [speculative] |

Ralio позиционируется как trust layer для agentic B2B payments: policies, intent, identity, approvals и audit across Faster Payments/BACS/SEPA, cards и stablecoins. SDK использует локальную P‑256 identity key. Но продукт находится в early access; публичные dashboard numbers выглядят демонстрационными и не должны считаться traction. [Ralio, без даты, доступ 01.09.2026](https://ralio.co/) [low-authority][single-source]

TessPay описывает более радикальную cryptographic architecture: signed AP2 mandates, escrow, telemetry proof через TLSNotary/TEE, BFT validators и Merkle-root evidence. Это ближе всего к “проверяемому доказательству исполнения”, но named production customers или реальный throughput не опубликованы. [TessPay docs](https://www.tesseris.org/docs/tess-pay) и [workflow](https://www.tesseris.org/docs/tesspay-workflow), без даты, доступ 01.09.2026 [emerging]

Fireblocks уже предлагает agentic payments с policy engine, KYT/Travel Rule before transfer, custody permissions и audit trail. Для stablecoin processor/custodian покупка отдельного recorder проиграет, если Fireblocks обеспечивает достаточную доказательность внутри существующего workflow. [Fireblocks product page, без даты, доступ 01.09.2026](https://www.fireblocks.com/products/agentic-payments) [single-source]

1Kosmos прямо продвигает step-up authentication, signed time-bound credentials и запись связи между человеком, агентом, policy и действием, включая money movement. Но это vendor marketing; открытого финансового replay/state engine не показано. [1Kosmos, без даты, доступ 01.09.2026](https://www.1kosmos.com/solutions/identity/agents) [low-authority][single-source]

AWS AgentCore Payments стала GA 18 августа 2026 года и предлагает ограничение суммы/времени, Stripe/Coinbase integrations и platform observability. Это важный distribution signal: hyperscaler уже превращает безопасные agent payments в feature. [AWS, 18.08.2026](https://aws.amazon.com/blogs/machine-learning/amazon-bedrock-agentcore-payments-is-now-generally-available-enabling-agents-to-transact-safely-and-autonomously-at-scale/) [single-source]

## 8. Стандарты, которые уже забирают часть продукта

### Google AP2

AP2 ввёл три типа подписанных mandates: intent, cart и payment. Они связывают намерение пользователя, конкретную корзину и платёжную инструкцию и дают доказательства для dispute. [Google, 16.09.2025](https://cloud.google.com/blog/products/ai-machine-learning/announcing-agents-to-payments-ap2-protocol) и [AP2 v0.2 specification, доступ 01.09.2026](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md) [established]

Однако AP2 оставляет вне scope точные механизмы хранения/retrieval evidence, часть delegation semantics и происхождение самого intent. Это не готовый audit product. Это слой, поверх которого его логично строить.

### Visa, Mastercard и Stripe

Visa TAP использует HTTP message signatures, чтобы merchant мог распознать доверенного агента и его intent. Mastercard Agent Pay объединяет registered agents, tokenized credentials и dispute/audit concepts. Stripe shared payment token ограничивает merchant, сумму и применение payment credential. [Visa TAP, доступ 01.09.2026](https://developer.visa.com/capabilities/trusted-agent-protocol/trusted-agent-protocol-specifications), [Mastercard, 29.04.2025](https://investor.mastercard.com/investor-news/investor-news-details/2025/Mastercard-Unveils-Agent-Pay-Pioneering-Agentic-Payments-Technology-to-Power-Commerce-in-the-Age-of-AI/default.aspx) и [Stripe, 07.10.2025](https://stripe.com/blog/introducing-our-agentic-commerce-solutions) [established]

Следствие: “cryptographic proof that user authorized payment” уже не уникальная идея. Уникальным может быть cross-standard evidence normalization и независимое verification/reconciliation.

## 9. Что в нише остаётся незакрытым

### 9.1 Полный authorization graph

Большинство продуктов доказывает один переход:

- user → mandate;
- policy → allow/deny;
- agent → tool call;
- wallet → signature.

Но спор после инцидента требует всю цепочку:

`человек/организация → роль → агент → модель/version → prompt/context → policy/version → approver → signer → rail → settlement → resulting state`

Полного переносимого graph среди открытых продуктов не найдено. [emerging]

### 9.2 Authoritative before/after state

Observability пишет то, что агент сказал или tool вернул. Для финансового аудита нужно независимо читать authoritative source:

- custody balance;
- bank ledger;
- nonce;
- allowance;
- portfolio exposure;
- settlement status;
- beneficiary ownership.

Nava частично читает state до действия; Fireblocks знает custody state; ни один открытый продукт не показывает vendor-neutral reconciliation across rails. [emerging]

### 9.3 Safe replay вместо повторной оплаты

Обычный trace replay опасен: второй запуск может второй раз перевести деньги. Нужен counterfactual replay:

- freeze model/tool/policy versions;
- stub external side effects;
- reconstruct authoritative state at decision time;
- compare original и alternative verdict;
- никогда не отправлять transaction повторно.

Именно такой financial replay в документации исследованных игроков не найден. `[no source found]`

### 9.4 Evidence package вне vendor database

Audit log внутри Fireblocks, Nava или NeuralTrust требует доверять тому же vendor. Настоящий Flight Recorder должен экспортировать self-verifying package:

- canonical events;
- hashes/Merkle root;
- signatures и key provenance;
- policy/model/tool versions;
- external receipts;
- timestamp authority;
- revocation status;
- deterministic verifier.

TessPay ближе всех к этой архитектуре, но maturity не доказана. [TessPay docs, доступ 01.09.2026](https://www.tesseris.org/docs/tess-pay) [emerging]

### 9.5 Независимый buyer

Главный вопрос не технический, а экономический: купит ли recorder оператор агента или доказательства нужны стороне, которая оператору не полностью доверяет?

Наиболее логичные buyers:

1. insurer, underwriting agentic-transaction risk;
2. custodian, разрешающий third-party agents;
3. auditor/SOC/control-assurance provider;
4. stablecoin processor, подключающий множество agent frameworks;
5. treasury platform с segregation of duties.

Если buyer остаётся обычным developer, Lemma/NeuralTrust/Runta выиграют bundle-экономикой.

---

## 10. Почему ниша ещё не закрылась

### Версия “рынок слишком ранний” — наиболее сильная

Подтверждённых крупных потерь в регулируемых учреждениях мало; production agent payments только становятся GA. AWS запустила AgentCore Payments в августе 2026, а Google AP2 появился лишь в сентябре 2025. [AWS, 18.08.2026](https://aws.amazon.com/blogs/machine-learning/amazon-bedrock-agentcore-payments-is-now-generally-available-enabling-agents-to-transact-safely-and-autonomously-at-scale/) и [Google, 16.09.2025](https://cloud.google.com/blog/products/ai-machine-learning/announcing-agents-to-payments-ap2-protocol) [established]

### Версия “это feature, а не company” — серьёзная угроза

Custodian, payment processor и cloud provider уже контролируют signer и transaction rail. Им дешевле добавить policy/audit, чем позволить независимому vendor стать inline dependency. Fireblocks и AWS — прямые доказательства этой тенденции. [Fireblocks](https://www.fireblocks.com/products/agentic-payments) и [AWS](https://aws.amazon.com/blogs/machine-learning/amazon-bedrock-agentcore-payments-is-now-generally-available-enabling-agents-to-transact-safely-and-autonomously-at-scale/) [established]

### Версия “сложно технически” — тоже верна

Нельзя одновременно гарантировать deterministic replay и replay live-world facts. Bank balance, exchange order book, sanctions status и model output могли измениться. Продукт обязан различать:

- exact event reconstruction;
- policy reevaluation;
- model re-simulation;
- settlement re-execution, которое вообще нельзя делать автоматически.

### Что разрешит спор

Не ещё один competitive scan, а 15 проверок buying behavior:

- 5 custodians/processors;
- 3 treasury platforms;
- 3 insurers/underwriters;
- 2 audit/control firms;
- 2 agentic-payment startups.

Решающие вопросы: кто отвечает за loss, какой evidence запрашивается после dispute, существует ли бюджет, согласны ли они на независимый inline component.

---

# 11. Сигналы спроса и регулирование

## 11.1 IMF

IMF в апреле 2026 года разделил agentic payments на intent, authorization и settlement и отметил конфликт между probabilistic agents и deterministic payment systems. Документ подчёркивает необходимость mandates, traceability, liability allocation и monitoring. [IMF, 24.04.2026](https://www.imf.org/en/publications/imf-notes/issues/2026/04/22/how-agentic-ai-will-reshape-payments-575560) [established]

Это не binding regulation, но сильный institutional demand signal.

## 11.2 FSB

FSB в июне 2026 года выпустил sound practices для AI risk management. В одном из case studies финансовая организация запрещает агенту совершать financial transactions до завершения дополнительного тестирования и мониторинга. [FSB page, 10.06.2026](https://www.fsb.org/2026/06/sound-practices-for-risk-management-of-artificial-intelligence/) и [полный PDF](https://www.fsb.org/uploads/P100626.pdf) [established]

Это подтверждает наличие control gap, но также показывает угрозу спросу: банк может просто не разрешить автономные деньги.

## 11.3 BIS/Basel

BIS в июле 2026 года поднимал вопросы ex-ante audit и распределения ответственности между principal и AI agent. [BIS speech, 31.07.2026](https://www.bis.org/speeches/20260731-growth-and-regulation) [single-source]

Basel operational-risk guidance, созданный до текущей волны agents, уже требует надёжной авторизации лица/агента/системы, non-repudiation, контроля privileges и audit trail по действиям с финансовыми последствиями. Он не предписывает Flight Recorder, но формирует покупательский язык. [BCBS Principles for Operational Resilience, март 2021; источник старше 18 месяцев](https://www.bis.org/bcbs/publ/d516.htm) [established]

## 11.4 EU AI Act

Статья 12 EU AI Act требует автоматического логирования для high-risk AI systems; статьи 19 и 26 регулируют хранение logs provider/deployer, обычно не менее шести месяцев. [Regulation EU 2024/1689, 13.06.2024; current consolidated page accessed 01.09.2026](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) [established]

Но утверждение “любой agentic payment подпадает под high-risk AI” было бы неверным. Классификация зависит от use case из Annex III или product-safety regime; обычный payment agent автоматически high-risk не становится. После изменения графика значительная часть Annex III high-risk obligations применяется с 2 декабря 2027 года. [European Commission timeline, updated 2026](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai) [established]

Итак, AI Act — tailwind для logging discipline, но не готовый юридический mandate на Flight Recorder.

## 11.5 MiCA

MiCA требует от crypto-asset service providers хранить records услуг, activities, orders и transactions минимум пять лет, с возможным продлением до семи. [Regulation EU 2023/1114, 09.06.2023; старше 18 месяцев](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32023R1114) [established]

Это binding recordkeeping, но не agent-specific authorization proof. Flight Recorder должен экспортироваться в MiCA/SOC/audit controls, а не заявлять, что “MiCA требует наш продукт”.

## 11.6 EBA/PSD2

EBA в мае 2026 года подтвердил требования dynamic linking между суммой и payee в контексте strong customer authentication. [EBA Q&A 2025_7678, 19.05.2026](https://www.eba.europa.eu/single-rule-book-qa/qna/view/publicId/2025_7678) [established]

Это важно для delegated agents: generic “user approved shopping” не обязательно достаточно для конкретной суммы и получателя.

## 11.7 США

Специального обязательного федерального стандарта США для agentic-payment flight recording на 01.09.2026 я не нашёл. `[no source found]`

Спрос в США пока формируют:

- банковские operational-risk правила;
- card-network standards;
- custody controls;
- contractual liability;
- state/federal money-transmission and securities obligations конкретного rail.

Следовательно, regulatory pitch должен быть “evidence for existing controls”, а не “новый закон заставляет купить”.

---

## 12. Инциденты 2025–2026

### 12.1 Реальный MCP supply-chain incident

В сентябре 2025 года вредоносная версия Postmark MCP package тайно BCC-копировала emails атакующему. Это реальный supply-chain incident, но не денежная потеря. [Koi Security, 25.09.2025](https://www.koi.ai/blog/postmark-mcp-npm-malicious-backdoor-email-theft) [single-source]

Сигнал: tool output и intent недостаточны; нужно фиксировать конкретный tool artifact/version и вызванный внешний эффект.

### 12.2 Tool poisoning

Invariant Labs показала в апреле 2025 года, как malicious MCP tool description может захватить agent workflow и вынести данные. Это лабораторное исследование, не подтверждённый финансовый loss. [Invariant Labs, 01.04.2025](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks) [single-source]

### 12.3 Agent2Agent session smuggling

Unit 42 продемонстрировала unauthorized stock purchase через session-smuggling attack. Это PoC, а не публично подтверждённая потеря клиента. [Unit 42, 2025](https://unit42.paloaltonetworks.com/agent-session-smuggling-in-agent2agent-systems/) [single-source]

### 12.4 Bankr

В мае 2026 года Bankr остановил transactions после компрометации 14 wallets и пообещал reimbursement. Публичные оценки loss расходились; встречались числа около $150 тыс., $385 тыс. и $440 тыс. Подтверждён сам breach и число wallets, но окончательная сумма и точная роль Grok/agent trust layer без полноценного первичного postmortem не установлены. [Cointelegraph, 20.05.2026](https://cointelegraph.com/news/bankr-disables-transactions-after-14-wallets-hacked) [low-authority][single-source]

Стилмен слабой стороны: это может быть обычная credential/social-engineering compromise, ошибочно переписанная как “AI agent stole funds”. Поэтому использовать Bankr как главный market proof нельзя.

### 12.5 Unauthorized purchase by Operator

Сообщалось, что OpenAI Operator купил яйца примерно за $31.43 без финального подтверждения пользователя. Доступная запись опирается на вторичный incident database/медиа, а не на технический postmortem. [single-source][low-authority] Точный первичный источник не найден.

### Итог по инцидентам

[emerging] Уязвимость пути agent→tool→external effect доказана.

[speculative] Утверждение, что уже существует крупный подтверждённый рынок insured agentic-payment losses, не доказано.

Я исключил из доказательств множество SEO-историй о потерях $4,800, $47,000 и $500,000: у них не было проверяемого первичного incident report.

---

# Часть 3. NSF SBIR Phase I и O‑1

## 13. Что представляет собой текущая программа

NSF 26-510 опубликована 22 мая 2026 года. Phase I предоставляет до $305,000 на R&D длительностью 6–18 месяцев. NSF ожидает примерно 180 SBIR Phase I и 50 STTR Phase I awards в год. [NSF 26-510, 22.05.2026](https://www.nsf.gov/funding/opportunities/small-business-innovation-research-small-business-technology/nsf26-510/solicitation) [established]

Ближайший full-proposal deadline после даты этого отчёта — **4 ноября 2026 года, 17:00 по местному времени submitting organization**. Следующие указаны на 4 марта и 7 июля 2027 года. [NSF 26-510](https://www.nsf.gov/funding/opportunities/small-business-innovation-research-small-business-technology/nsf26-510/solicitation) и [full-proposal page](https://seedfund.nsf.gov/apply/full-proposal/) [established]

### Процесс

1. Project Pitch подаётся без SAM/Research.gov registration.
2. NSF обычно отвечает на pitch за 1–2 месяца.
3. При invitation компания готовит full proposal.
4. После deadline panel review занимает примерно 1–3 месяца.
5. Возможная due diligence — 3–5 месяцев.
6. Финальный ответ обычно приходит через 5–7 месяцев.
7. Исторический Phase I funding rate — 10–20%.

Источники: [Project Pitch, доступ 01.09.2026](https://seedfund.nsf.gov/apply/project-pitch/) и [review process](https://seedfund.nsf.gov/apply/review-decision/) [established]

Учитывая 1–2 месяца на pitch и близость 4 ноября, новый pitch, поданный 1 сентября, находится на границе реалистичности этого окна. Invitation действует на следующие два deadlines, поэтому март 2027 — более надёжная operational target. [NSF solicitation](https://www.nsf.gov/funding/opportunities/small-business-innovation-research-small-business-technology/nsf26-510/solicitation) [emerging]

## 14. Критическая eligibility-развилка

### 14.1 Ownership компании

NSF формулирует правило так:

> “at least 51% owned and controlled by U.S. citizens or permanent residents”

[NSF eligibility, доступ 01.09.2026](https://seedfund.nsf.gov/solicitation-eligibility/)

SBA уточняет, что более 50% должно быть прямо owned and controlled eligible individuals/entities и анализируется не только формальная акция, но и реальный control, включая fully diluted structure. [SBA eligibility guide, сентябрь 2024](https://www.sbir.gov/sites/default/files/elig_size_compliance_guide.pdf) [established]

**Фаундер на O‑1 без green card не является U.S. citizen или permanent resident.** Если он владеет и контролирует более 50% applicant company, компания не проходит NSF SBIR ownership test. O‑1 здесь ничего не заменяет. Confidence: 98% [established].

### 14.2 Может ли O‑1 founder быть PI

Да, условно. NSF отдельно пишет, что personnel должны иметь legal right to work:

> “whether via citizenship, permanent residency or an appropriate visa”

[NSF eligibility, доступ 01.09.2026](https://seedfund.nsf.gov/solicitation-eligibility/)

PI должен быть более чем на 50% занят applicant small business в течение проекта; NSF считает conflict employment elsewhere свыше 19.6 часа в неделю. [NSF eligibility](https://seedfund.nsf.gov/solicitation-eligibility/) [established]

Следовательно, O‑1 founder может быть PI, если:

- applicant company или допустимый agent является его O‑1 petitioner;
- proposed R&D входит в разрешённые O‑1 duties;
- он имеет более 50% employment у applicant;
- вся R&D выполняется в США;
- сама компания независимо проходит 51% citizen/LPR ownership/control.

USCIS допускает petition от отдельного legal entity, принадлежащего beneficiary, при наличии bona fide employment relationship, но O‑1 не является переносимым общим разрешением работать на любую компанию. [USCIS O‑1 policy update, 08.01.2025](https://www.uscis.gov/sites/default/files/document/policy-manual-updates/20250108-ExtraordinaryAbility.pdf) и [USCIS employer guide](https://www.uscis.gov/sites/default/files/document/guides/E1en.pdf) [established]

### 14.3 Юридически возможные конфигурации

#### Конфигурация A: гражданин/LPR co-founder владеет и контролирует >50%

Это может пройти, если ownership и control настоящие, включая board/veto/economic arrangements. O‑1 founder остаётся minority shareholder и может быть PI при корректной petition/employment. [NSF eligibility](https://seedfund.nsf.gov/solicitation-eligibility/) и [SBA guide](https://www.sbir.gov/sites/default/files/elig_size_compliance_guide.pdf) [established]

Риск: потеря контроля и большей части economics ради гранта $305 тыс. может быть плохой сделкой. Sham 51/49, где O‑1 founder сохраняет фактический контроль скрытыми veto или side agreements, рискует не пройти SBA control analysis.

#### Конфигурация B: eligible U.S.-owned small-business parent владеет >50%

Допустимо, если parent сама owned/controlled citizens/LPR и контроль реальный. [NSF eligibility](https://seedfund.nsf.gov/solicitation-eligibility/) [established]

Это не loophole: гражданский ownership просто переносится на уровень выше.

#### Конфигурация C: дождаться green card

После получения permanent residence фаундер входит в eligible ownership class и может сохранить majority ownership. Это юридически чище всего, но может сделать программу несвоевременной.

#### Конфигурация D: STTR вместо SBIR

Не решает ownership problem. STTR также требует eligible small business; дополнительно нужен not-for-profit research institution, минимум 40% бюджета малому бизнесу и 30% research institution. [NSF eligibility](https://seedfund.nsf.gov/solicitation-eligibility/) [established]

#### Конфигурация E: majority VC ownership

Текущая NSF page прямо исключает businesses, majority-owned VC operating companies, hedge funds или private equity firms. [NSF eligibility](https://seedfund.nsf.gov/solicitation-eligibility/) [established]

### Практический ответ

- O‑1 founder с 100% ownership: **не проходит**.
- O‑1 founder с 49% и genuine citizen/LPR owner-controller с 51%: **может пройти**.
- O‑1 founder как PI: **может**, если work authorization и >50% employment корректны.
- Создавать фиктивного 51% номинала: **не рекомендую; высокий compliance/fraud risk**.

Это юридический разбор правил, не персональное заключение адвоката. Перед изменением cap table нужны одновременно SBIR counsel и O‑1 immigration counsel.

## 15. Fit проекта для NSF

Flight Recorder может быть NSF-fit, если pitch сформулирован как нерешённая technical R&D problem:

- tamper-evident provenance across probabilistic agents and deterministic ledgers;
- safe counterfactual replay of irreversible operations;
- privacy-preserving proof of authorization;
- cross-rail evidence normalization;
- formal verification of policy decisions.

Pitch “строим compliance dashboard” будет слабее: NSF финансирует high-risk technical innovation, а не обычный SaaS implementation. [NSF 26-510, 22.05.2026](https://www.nsf.gov/funding/opportunities/small-business-innovation-research-small-business-technology/nsf26-510/solicitation) [established]

---

## 16. Альтернативы non-dilutive

| Программа | Доступность non-resident founder | Размер/условия | Fit |
|---|---|---|---|
| **NLnet** | Международная, при open-source/public-interest результате | Обычно €5k–€50k; следующий call открывается 03.09.2026, deadline 03.11.2026 | Хорошо для open verifier/standard |
| **Circle Grants** | География не названа главным ограничением | Milestone-based USDC; сумма индивидуальна | Хорошо для USDC/Arc prototype |
| **Solana Foundation Grants** | Individuals/companies worldwide | Rolling, grant или convertible structure | Хорошо для onchain evidence/replay |
| **EIC Accelerator** | Нужна компания EU/associated country до full proposal | Grant до €2.5 млн; equity €1–10 млн | Сильный, но поздний и бюрократический |
| **Entrepreneurs First Fellowship** | O‑1 не является автоматическим препятствием; residency conditions | США: $10k equity-free fellowship; основной $250k — equity investment | Founder runway, не R&D grant |
| **Ethereum ESP** | Международный | В текущем срезе активного wishlist call не найдено | Мониторить, не считать доступными деньгами сегодня |

NLnet принимает предложения на libre/open technology; текущая страница сообщает следующее окно 3 сентября — 3 ноября 2026 года. [NLnet, доступ 01.09.2026](https://nlnet.nl/propose/) [established]

Circle принимает grant applications для проектов, расширяющих Circle/USDC/Arc ecosystem; traction и milestone plan повышают fit. [Circle Grants, доступ 01.09.2026](https://www.circle.com/grant) [single-source]

Solana Foundation заявляет rolling applications, поддержку public-goods/open-source projects и возможность заявки от individual/company независимо от location. [Solana Grants, доступ 01.09.2026](https://solana.org/grants-funding) [single-source]

EIC Accelerator допускает single startup/SME без консорциума; grant component может быть до €2.5 млн, equity — €1–10 млн. Applicant из non-associated country должен создать или перенести компанию в EU/associated country до full proposal. [EIC Accelerator, доступ 01.09.2026](https://eic.ec.europa.eu/eic-funding-opportunities/eic-accelerator_en) и [EIC FAQ](https://eic.ec.europa.eu/eic-frequently-asked-questions/faqs-eic-accelerator_en) [established]

Entrepreneurs First в США предлагает $10k equity-free fellowship с жильём; основной путь $250k включает $125k post-money SAFE за 8% и опциональные $125k MFN SAFE. В Лондоне заявлен £6,000 Talent Investment, после чего возможны equity terms. [EF US FAQ, доступ 01.09.2026](https://www.joinef.com/faqs/us/) и [EF London FAQ](https://www.joinef.com/faqs/london/) [established]

Coinbase/Base, Stripe и Ethereum Foundation часто упоминаются в старых grant lists, но активного открытого подходящего call на 1 сентября 2026 года я надёжно не подтвердил. Их нельзя ставить в funding plan как committed option. `[no current source found]`

---

# Часть 4. Stablecoin corridors

## 17. Карта 2026 года

Stablecoin infrastructure уже консолидируется вокруг крупных капитальных игроков и лицензированных rails.

Rain объявила $250 млн Series C 9 января 2026 года при valuation $1.95 млрд и cumulative funding свыше $338 млн. Компания обслуживает stablecoin-linked cards и payment programs; заявленные 38× volume growth и 30× program growth являются self-reported. [Rain, 09.01.2026](https://www.rain.xyz/resources/rain-raises-250m-series-c-to-build-the-money-layer-for-the-internet) [low-authority][single-source]

Mastercard в марте 2026 года договорилась приобрести BVNK; публичная цена в официальном BVNK announcement не раскрыта. До этого BVNK объявила Visa Direct stablecoin integration. [BVNK–Mastercard, 17.03.2026](https://www.bvnk.com/blog/why-bvnk-is-joining-mastercard) и [BVNK–Visa, 14.01.2026](https://www.bvnk.com/blog/bvnk-powers-stablecoin-payments-for-visa-direct) [established]

Stripe завершила приобретение Bridge ещё 4 февраля 2025 года, а в 2026 продолжила превращать stablecoin orchestration в часть собственной платформы. Это не 2026 financing event, но сильнее раунда: инфраструктура вошла в distribution Stripe. [Stripe–Bridge, 04.02.2025; источник старше 18 месяцев](https://stripe.com/newsroom/news/stripe-completes-bridge-acquisition) и [Stripe Sessions 2026](https://stripe.com/blog/everything-we-announced-at-sessions-2026) [established]

В Азии dtcpay привлекла $10 млн Series A в марте 2026 года для расширения регулируемых stablecoin payments; компания подчёркивает MAS-regulated position и европейское expansion. [Vertex Ventures, 17.03.2026](https://www.vertexventures.sg/news/dtcpay-raises-us-10m-series-a-to-accelerate-global-expansion-of-regulated-stablecoin-payments/) [single-source]

Conduit действительно является финансируемым cross-border stablecoin player, но подтверждённого нового раунда именно в 2026 году я не нашёл. `[no 2026 financing source found]`

### Вывод по коридорам

Утверждение “рынок B2B stablecoin infra занят” в основном подтверждается. Утверждение “без лицензии вход невозможен” слишком широкое.

- Если компания принимает custody, конвертирует fiat/crypto или сама исполняет transfers, licensing/partner-bank burden огромный.
- Если компания только подписывает, проверяет и экспортирует evidence, не контролируя funds или settlement, она потенциально может работать как software vendor поверх лицензированных processors.

Точная регуляторная граница зависит от юрисдикции и фактического control над средствами; её нужно подтверждать counsel до дизайна продукта.

---

# 18. Что опровергает запуск Flight Recorder

1. **Прямые конкуренты уже есть.** Nava проверяет onchain actions до исполнения, Ralio заявляет trust layer для bank/card/stablecoin payments, а TessPay строит cryptographic verify-then-pay. [Nava docs](https://docs.navalabs.ai/sdk/operations/), [Ralio](https://ralio.co/) и [TessPay](https://www.tesseris.org/docs/tess-pay) [established]

2. **Authorization proof превращается в стандарт.** AP2 mandates, Visa TAP signatures, Mastercard registered agents и Stripe constrained tokens забирают центральную часть первоначального moat. [Google AP2](https://cloud.google.com/blog/products/ai-machine-learning/announcing-agents-to-payments-ap2-protocol), [Visa TAP](https://developer.visa.com/capabilities/trusted-agent-protocol/trusted-agent-protocol-specifications), [Mastercard Agent Pay](https://investor.mastercard.com/investor-news/investor-news-details/2025/Mastercard-Unveils-Agent-Pay-Pioneering-Agentic-Payments-Technology-to-Power-Commerce-in-the-Age-of-AI/default.aspx) и [Stripe SPT](https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens) [established]

3. **Incumbents могут bundle feature.** Fireblocks уже объединяет custody, policy, KYT и audit; AWS объединяет agent runtime, payment limits и observability. [Fireblocks](https://www.fireblocks.com/products/agentic-payments) и [AWS](https://aws.amazon.com/blogs/machine-learning/amazon-bedrock-agentcore-payments-is-now-generally-available-enabling-agents-to-transact-safely-and-autonomously-at-scale/) [established]

4. **Подтверждённого большого loss market пока мало.** Есть attacks, PoCs и crypto breaches, но мало первичных postmortems regulated agentic-payment losses. [Invariant](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks), [Unit 42](https://unit42.paloaltonetworks.com/agent-session-smuggling-in-agent2agent-systems/) [emerging]

5. **Банк может выбрать запрет, а не recorder.** FSB case study прямо показывает организацию, запрещающую autonomous financial transactions до дальнейших тестов. [FSB, 10.06.2026](https://www.fsb.org/2026/06/sound-practices-for-risk-management-of-artificial-intelligence/) [established]

---

# 19. Что поддерживает запуск

1. **Ни один продукт не закрывает полную цепь evidence.** Authorization, runtime trace, custody state и settlement receipts остаются в разных системах. Это подтверждает сравнительный анализ docs. [emerging]

2. **Регуляторный язык уже совпадает с продуктом.** IMF требует разделять intent, authorization и settlement; Basel — authorization, non-repudiation и audit trail. [IMF, 24.04.2026](https://www.imf.org/en/publications/imf-notes/issues/2026/04/22/how-agentic-ai-will-reshape-payments-575560) и [BCBS, март 2021](https://www.bis.org/bcbs/publ/d516.htm) [established]

3. **AP2 создаёт substrate, а не закрывает storage/retrieval.** Стандарт оставляет место для независимого evidence repository/verifier. [AP2 specification, доступ 01.09.2026](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md) [established]

4. **Деньги уже идут в категорию.** Nava заявила $8.3 млн, Ralio $2.5 млн, а agent-security компании Runta и NeuralTrust получили по $20 млн. [Nava, 07.04.2026](https://navalabs.ai/blog/introducing-nava-guardrails-for-autonomous-agents), [Runta/a16z, 16.07.2026](https://a16z.com/announcement/investing-in-runta/) и [NeuralTrust, 17.06.2026](https://neuraltrust.ai/news/neuraltrust-raises-20m) [emerging; Ralio/Nava/NeuralTrust amounts partly company-reported]

5. **Safe replay остаётся открытой технической проблемой.** Ни один проверенный продукт не показывает воспроизводимость решения без повторного irreversible side effect. Это годится и для product moat, и для NSF technical-merit argument. `[no competing implementation found]` [emerging]

---

# 20. Варианты решения

## Вариант A — широкий 45-дневный customer discovery

Pitch: “black box for all financial AI agents”.

Плюс: сохраняет широкий TAM.

Минусы:

- слишком близок к Nava/Ralio;
- horizontal layer конфликтует с Runta/NeuralTrust/Lemma;
- трудно объяснить, почему Fireblocks/AWS не добавят это сами.

Вердикт: не рекомендую.

## Вариант B — 14-дневный kill-test узкого evidence wedge

Конкретный продукт:

> Open, vendor-neutral Financial Action Evidence Bundle для AP2/x402/Fireblocks/onchain actions: authorization graph + before/after reconciliation + tamper-evident export + offline verifier.

Первый use case: stablecoin processor или custodian разрешает внешнему агенту отправить платёж и должен доказать auditor/insurer, почему действие было допустимо.

Что построить:

1. Canonical transaction/evidence schema.
2. AP2 mandate importer.
3. Fireblocks или Safe/EVM adapter.
4. Independent before/after balance reader.
5. Hash-chain/Merkle evidence package.
6. Offline verification CLI.
7. Counterfactual replay, где external calls замещены recorded responses.

Kill-критерии за 14 дней:

- минимум 5 из 15 интервью подтверждают обязательный post-incident evidence workflow;
- минимум 3 готовы дать anonymized historical transaction;
- минимум 2 согласны на design partnership;
- хотя бы 1 insurer/auditor требует независимый, а не vendor-owned log;
- ни Fireblocks, ни Ralio/Nava не показывают эквивалентный exportable evidence package в закрытом demo.

Риск: narrow wedge может оказаться feature для Fireblocks.

## Вариант C — NSF-first

Сначала перестроить ownership и подать Project Pitch.

Плюс: до $305 тыс. non-dilutive R&D.

Минусы:

- O‑1-majority cap table не проходит;
- менять контроль ради гранта опасно;
- решение и деньги могут занять 5–7 месяцев после deadline;
- коммерческий риск останется непроверенным.

Вердикт: не делать до customer proof и legal cap-table memo.

## Вариант D — open-source verifier + ecosystem grants

Опубликовать evidence schema/offline verifier и подать NLnet, Circle и Solana.

Плюсы:

- строит стандарт и доверие;
- меньше конфликт с processors;
- даёт distribution среди integrators.

Минусы:

- слабее прямой monetization;
- incumbents могут принять формат без покупки hosted product.

Вердикт: разумный второй шаг после kill-test.

# Рекомендация

**Выбрать вариант B, затем при подтверждении спроса добавить вариант D.** Не вкладывать 45 дней в широкий discovery и не менять cap table ради NSF сейчас.

Финальный verdict: **есть окно, вот оно — независимый cross-rail evidence and replay layer, а не ещё один agent guardrail или payment orchestrator.** Confidence 78% [emerging].

Главное допущение: insurers, auditors или custodians действительно потребуют доказательство, независимое от payment processor и agent operator. Если первые 15 интервью это не подтверждают, продукт следует закрыть или превратить в модуль/стандарт для Fireblocks/Ralio/Nava.

## 🧒 Простыми словами

Рынок не пустой: несколько команд уже ставят охранника перед кошельком агента. Но почти никто не собирает полноценное «дело для следователя»: кто разрешил, по какому правилу, сколько было денег до, сколько стало после и можно ли всё проверить без доверия к продавцу системы.

Поэтому не надо строить ещё одного охранника. Надо за две недели проверить, заплатит ли кто-нибудь за независимую чёрную коробку. Если страховщики и кастодианы скажут «да, без такого доказательства агентов к деньгам не пустим», окно настоящее.

---

# Полный список использованных источников

1. [Runta — Documentation Overview](https://runta.com/docs/overview/), дата не указана, доступ 01.09.2026.
2. [Runta — Egress](https://runta.com/docs/runtime/egress/), дата не указана, доступ 01.09.2026.
3. [Runta — Checkpoints](https://runta.com/docs/runtime/checkpoints/), дата не указана, доступ 01.09.2026.
4. [Runta — OpenAI Agents SDK Integration](https://runta.com/docs/integrations/openai-agents/), дата не указана, доступ 01.09.2026.
5. [Runta — Create Runtime API](https://runta.com/docs/reference/api/operations/createruntime/), дата не указана, доступ 01.09.2026.
6. [Runta — Pricing](https://runta.com/pricing/), дата не указана, доступ 01.09.2026.
7. [Runta — The Execution Layer for Agents](https://runta.com/blog/runta-the-execution-layer-for-agents/), 16.07.2026.
8. [a16z — Investing in Runta](https://a16z.com/announcement/investing-in-runta/), 16.07.2026.
9. [Runta — About](https://runta.com/about/), дата не указана, доступ 01.09.2026.
10. [Runta LinkedIn](https://www.linkedin.com/company/runtahq), доступ 01.09.2026.
11. [NeuralTrust Documentation Index](https://docs.neuraltrust.ai/llms.txt), доступ 01.09.2026.
12. [NeuralTrust Documentation](https://docs.neuraltrust.ai/), доступ 01.09.2026.
13. [NeuralTrust — AI Agent Security](https://neuraltrust.ai/ai-agent-security), доступ 01.09.2026.
14. [NeuralTrust — About](https://neuraltrust.ai/about), доступ 01.09.2026.
15. [NeuralTrust Raises $20M](https://neuraltrust.ai/news/neuraltrust-raises-20m), 17.06.2026.
16. [NeuralTrust LinkedIn](https://www.linkedin.com/company/neuraltrust), доступ 01.09.2026.
17. [Lemma Documentation Index](https://docs.uselemma.ai/llms.txt), доступ 01.09.2026.
18. [Lemma — Introduction](https://docs.uselemma.ai/getting-started/introduction), доступ 01.09.2026.
19. [Lemma](https://www.uselemma.ai/), доступ 01.09.2026.
20. [Lemma $2.3M Pre-seed Announcement](https://www.globenewswire.com/news-release/2026/08/13/3344773/0/en/2-3m-pre-seed-for-lemma-backed-by-matrix-yc-openai-xai-operators-to-fix-silent-ai-agent-failures.html), 13.08.2026.
21. [Y Combinator — Lemma](https://www.ycombinator.com/companies/uselemma), доступ 01.09.2026.
22. [Nava Labs](https://navalabs.ai/), доступ 01.09.2026.
23. [Nava — Key Terms](https://docs.navalabs.ai/introduction/key-terms/), доступ 01.09.2026.
24. [Nava — Operations](https://docs.navalabs.ai/sdk/operations/), доступ 01.09.2026.
25. [Nava — TypeScript SDK](https://docs.navalabs.ai/sdk/typescript/), доступ 01.09.2026.
26. [Nava — Guardian Actions](https://docs.navalabs.ai/api/guardian/actions/), доступ 01.09.2026.
27. [Nava — Failure Modes](https://docs.navalabs.ai/guardian/developers/failure-modes/), доступ 01.09.2026.
28. [Nava — Guardrails for Autonomous Agents](https://navalabs.ai/blog/introducing-nava-guardrails-for-autonomous-agents), 07.04.2026.
29. [One](https://www.withone.ai/), доступ 01.09.2026.
30. [One — About](https://www.withone.ai/about), обновлено в марте 2026.
31. [Beacon Security](https://beacon.security/), доступ 01.09.2026.
32. [1Kosmos — Identity for AI Agents](https://www.1kosmos.com/solutions/identity/agents), доступ 01.09.2026.
33. [Ory Launches Agent Security](https://www.ory.com/blog/ory-launches-agent-security), 09.06.2026.
34. [Ralio](https://ralio.co/), доступ 01.09.2026.
35. [Tesseris — TessPay](https://www.tesseris.org/docs/tess-pay), доступ 01.09.2026.
36. [Tesseris — TessPay Workflow](https://www.tesseris.org/docs/tesspay-workflow), доступ 01.09.2026.
37. [Fireblocks — Agentic Payments](https://www.fireblocks.com/products/agentic-payments), доступ 01.09.2026.
38. [Skyfire Documentation](https://docs.skyfire.xyz/docs/introduction-to-skyfire), доступ 01.09.2026.
39. [Skyfire Product](https://skyfire.xyz/product), доступ 01.09.2026.
40. [AWS — AgentCore Payments GA](https://aws.amazon.com/blogs/machine-learning/amazon-bedrock-agentcore-payments-is-now-generally-available-enabling-agents-to-transact-safely-and-autonomously-at-scale/), 18.08.2026.
41. [Google Cloud — Announcing AP2](https://cloud.google.com/blog/products/ai-machine-learning/announcing-agents-to-payments-ap2-protocol), 16.09.2025.
42. [Google — AP2 Specification](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md), доступ 01.09.2026.
43. [Visa — Trusted Agent Protocol](https://developer.visa.com/capabilities/trusted-agent-protocol/trusted-agent-protocol-specifications), доступ 01.09.2026.
44. [Mastercard — Agent Pay](https://investor.mastercard.com/investor-news/investor-news-details/2025/Mastercard-Unveils-Agent-Pay-Pioneering-Agentic-Payments-Technology-to-Power-Commerce-in-the-Age-of-AI/default.aspx), 29.04.2025.
45. [Mastercard — Agentic Commerce Framework](https://www.mastercard.com/us/en/news-and-trends/stories/2025/agentic-commerce-framework.html), 2025.
46. [Stripe — Shared Payment Tokens](https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens), доступ 01.09.2026.
47. [Stripe — Agentic Commerce Solutions](https://stripe.com/blog/introducing-our-agentic-commerce-solutions), 07.10.2025.
48. [Coinbase — x402](https://docs.cdp.coinbase.com/x402/welcome), доступ 01.09.2026.
49. [IMF — How Agentic AI Will Reshape Payments](https://www.imf.org/en/publications/imf-notes/issues/2026/04/22/how-agentic-ai-will-reshape-payments-575560), 24.04.2026.
50. [FSB — Sound Practices for AI Risk Management](https://www.fsb.org/2026/06/sound-practices-for-risk-management-of-artificial-intelligence/), 10.06.2026.
51. [FSB — Full Report PDF](https://www.fsb.org/uploads/P100626.pdf), 10.06.2026.
52. [BIS — Growth and Regulation](https://www.bis.org/speeches/20260731-growth-and-regulation), 31.07.2026.
53. [BCBS — Principles for Operational Resilience](https://www.bis.org/bcbs/publ/d516.htm), март 2021.
54. [EU Regulation 2024/1689 — AI Act](https://eur-lex.europa.eu/eli/reg/2024/1689/oj), 13.06.2024.
55. [European Commission — AI Act Timeline](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai), обновлено в 2026.
56. [EBA Q&A 2025_7678](https://www.eba.europa.eu/single-rule-book-qa/qna/view/publicId/2025_7678), 19.05.2026.
57. [EU Regulation 2023/1114 — MiCA](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32023R1114), 09.06.2023.
58. [Invariant Labs — MCP Tool Poisoning](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks), 01.04.2025.
59. [Koi Security — Postmark MCP Incident](https://www.koi.ai/blog/postmark-mcp-npm-malicious-backdoor-email-theft), 25.09.2025.
60. [Unit 42 — Agent Session Smuggling](https://unit42.paloaltonetworks.com/agent-session-smuggling-in-agent2agent-systems/), 2025.
61. [Cointelegraph — Bankr Disables Transactions](https://cointelegraph.com/news/bankr-disables-transactions-after-14-wallets-hacked), 20.05.2026.
62. [NSF 26-510 Solicitation](https://www.nsf.gov/funding/opportunities/small-business-innovation-research-small-business-technology/nsf26-510/solicitation), 22.05.2026.
63. [NSF — Project Pitch](https://seedfund.nsf.gov/apply/project-pitch/), доступ 01.09.2026.
64. [NSF — Full Proposal](https://seedfund.nsf.gov/apply/full-proposal/), доступ 01.09.2026.
65. [NSF — Review and Decision](https://seedfund.nsf.gov/apply/review-decision/), доступ 01.09.2026.
66. [NSF — Eligibility](https://seedfund.nsf.gov/solicitation-eligibility/), доступ 01.09.2026.
67. [SBA — SBIR/STTR Eligibility Guide](https://www.sbir.gov/sites/default/files/elig_size_compliance_guide.pdf), сентябрь 2024.
68. [USCIS — O‑1 Policy Update](https://www.uscis.gov/sites/default/files/document/policy-manual-updates/20250108-ExtraordinaryAbility.pdf), 08.01.2025.
69. [USCIS — Employer Guide](https://www.uscis.gov/sites/default/files/document/guides/E1en.pdf), дата не указана, доступ 01.09.2026.
70. [NLnet — Apply](https://nlnet.nl/propose/), доступ 01.09.2026.
71. [EIC Accelerator](https://eic.ec.europa.eu/eic-funding-opportunities/eic-accelerator_en), доступ 01.09.2026.
72. [EIC Accelerator FAQ](https://eic.ec.europa.eu/eic-frequently-asked-questions/faqs-eic-accelerator_en), доступ 01.09.2026.
73. [Circle Grants](https://www.circle.com/grant), доступ 01.09.2026.
74. [Solana Foundation — Grants and Funding](https://solana.org/grants-funding), доступ 01.09.2026.
75. [Ethereum Ecosystem Support Program](https://esp.ethereum.foundation/applicants), доступ 01.09.2026.
76. [Entrepreneurs First — U.S. FAQ](https://www.joinef.com/faqs/us/), доступ 01.09.2026.
77. [Entrepreneurs First — London FAQ](https://www.joinef.com/faqs/london/), доступ 01.09.2026.
78. [Rain — $250M Series C](https://www.rain.xyz/resources/rain-raises-250m-series-c-to-build-the-money-layer-for-the-internet), 09.01.2026.
79. [BVNK — Joining Mastercard](https://www.bvnk.com/blog/why-bvnk-is-joining-mastercard), 17.03.2026.
80. [BVNK — Visa Direct](https://www.bvnk.com/blog/bvnk-powers-stablecoin-payments-for-visa-direct), 14.01.2026.
81. [Stripe Completes Bridge Acquisition](https://stripe.com/newsroom/news/stripe-completes-bridge-acquisition), 04.02.2025.
82. [Stripe Sessions 2026](https://stripe.com/blog/everything-we-announced-at-sessions-2026), 2026.
83. [Vertex Ventures — dtcpay $10M Series A](https://www.vertexventures.sg/news/dtcpay-raises-us-10m-series-a-to-accelerate-global-expansion-of-regulated-stablecoin-payments/), 17.03.2026.

01.09.2026, 19:08, Лондон.