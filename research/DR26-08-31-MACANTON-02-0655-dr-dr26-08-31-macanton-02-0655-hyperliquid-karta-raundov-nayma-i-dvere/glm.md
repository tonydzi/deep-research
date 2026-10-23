# DR26-08-31-MACANTON-02-0655 — raw report from glm

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
dr: DR26-08-31-MACANTON-02-0655
vendor: Z.ai GLM (chat.z.ai)
door: browser-pane-anon
rail: glm

collected: 2026-09-01T00:56:51Z
took_sec: 0
bytes: 27043
note: >-
  Анонимная веб-сессия GLM с веб-поиском и deep-think. Это НЕ вендорский режим Deep Research — уровень «умная модель + поиск».
  Забор через локальный мост dr_glm_relay.py (0 токенов, тело не проходит через контекст).
---

# Hyperliquid Ecosystem Hiring & Funding Map — August 2026

**Bottom line first:** Hyperliquid core **still refuses venture capital and is not hiring business-development roles** — its only two open roles in August 2026 are engineering positions (Backend Engineer and Senior Frontend Engineer, both APAC, on-site). However, a fast-funding, fast-hiring ecosystem of HyperEVM/HyperCore applications has formed around it, and a new institutional wrapper (Hyperliquid Policy Center, Ripple Prime, Anchorage Digital, Fireblocks) has created the first genuine, non-engineering doors for a capital-formation / ecosystem-BD profile.

**Two naming clarifications that will save you embarrassment in outreach:**
1. **"Felix" the Hyperliquid protocol is NOT the Prague AI-workflow company "Felix"** that raised $1.7M in pre-seed from XYZ Venture in April 2026 — separate companies, no relation. The Hyperliquid Felix Protocol has no publicly disclosed VC round as of this writing [no source found].
2. **"Hyperliquid Strategies Inc" (NASDAQ: PURR) is a separate SPAC/digital-asset-treasury company** that raised ~$646.6M for HYPE accumulation. It is *not* Hyperliquid Labs, and its fundraise does not contradict Hyperliquid core's "no VC" stance — several outlets conflate the two.

## 1. Funding map — HyperEVM / HyperCore ecosystem projects, 2025–2026

| Project | What it does | Disclosed raise | Stage | Lead / notable investors | Source (date) |
|---|---|---|---|---|---|
| **Kinetiq** | Liquid staking (kHYPE) → HIP-3 deployer (Markets by Kinetiq) → Elysium L2 | $1.8M (some sources say $1.75M) | Seed | Maven 11 Capital, Pier Two, Chorus One, IMC, Flowdesk, SIG DTI, Susquehanna International, DeFi Dad, Comfy Capital | crypto-fundraising.info, Oct 26, 2025; CypherHunter, Oct 22, 2025 |
| **Felix Protocol** | Liquity V2 fork (feUSD CDP), Vanilla lending pools, tokenized equities via Ondo | No disclosed VC round [no source found] | Self-described "protocol" | RootData lists angels incl. Andrew Kang (Mechanism), Tarun Chitra (Gauntlet), Ansem, Vance [low-authority] | Hyperion DeFi partnership PR confirms $1B+ TVL, Oct 29, 2025 |
| **Hyperbeat** | Native yield layer / tokenized vaults | $5.2M | Seed (Aug 15, 2025) | ether.fi Ventures + Electric Capital co-led; Coinbase Ventures, Chapter One, Selini, Maelstrom, Anchorage Digital, Nonce Classic | CoinDesk, Aug 15, 2025; Bloomingbit, Aug 15, 2025 |
| **HyperLend** | Money-market lending on HyperEVM ("credit layer") | $1.7M | Announced ~Jan 20, 2026 | RockawayX, No Limit Holdings, Dewhales, Duplicate Capital, Vistula Capital, Nucleus | CryptoRank X post, Jan 20, 2026 [single-source for amount/investor list] |
| **HypurrFi** | Lending (pooled + isolated), USDXL CDP, HypurrFi card | Amount not disclosed [no source found] | Seed / backed | Robot Ventures; team ex-Aave, Kraken, Chainlink, SwissBorg, Astaria | HypurrFi site, retrieved Aug 2026 [single-source] |
| **Unit (Hyperunit)** | Cross-chain tokenization (UBTC/UETH/USOL via HIP-1); builds trade.xyz (HIP-3 DEX) | Self-funded research collective; no disclosed VC | Pre-token / self-funded | Core contributors ex-Hudson River Trading, Jump, Fortress; partnered with Hyperliquid Strategies on validator | Datawallet explainer; Investing.com, May 7, 2026 |
| **Based** (builder-code frontend / super-app) | Consumer trading + payments app | **$11.5M** | Series A (Feb 23, 2026) | **Pantera Capital led**; Coinbase Ventures, Wintermute Ventures, Karatage; equity + token warrants | The Block, Feb 23, 2026; Pantera, Feb 23, 2026 |
| **Phantom** (wallet, not HL-native) | Consumer wallet with native HL perps | N/A (raised separately) | — | $20.63M cumulative builder-code revenue on Hyperliquid (31.8% of total) | CoinGecko, May 26, 2026 |
| **pvp.trade** | Trading frontend | No disclosed raise | — | $7.2M lifetime builder-code revenue | Dwellir, Jan 16, 2026 [low-authority] |
| **Insilico Terminal** | Pro-grade trading terminal | No disclosed raise | — | $2.54M annualized fees | DefiLlama |
| *A Paradigm-backed terminal* (name not confirmed in sources) | Trading terminal | $7.6M seed | Seed | Paradigm; $5.6B cumulative volume on Hyperliquid | Blockworks, Dec 4, 2025 [single-source; company name not specified] |
| **Hyperliquid Strategies Inc (NASDAQ: PURR)** — *separate entity* | HYPE digital-asset treasury | $646.6M equity raise FY2026; $1B S-1 filed Oct 2025 | Public SPAC | Chardan Capital Markets advised | Cryptorank, Aug 28, 2026; Yahoo Finance, Oct 22, 2025 |
| **Hyperion DeFi (NASDAQ: HYPD)** — *separate entity* | First US-listed HYPE treasury; partners with Felix & HyperLend | Public | Public | — | Hyperion IR, Oct 29, 2025; Mar 9, 2026 |

**Funding-map read-outs:**
- **Kinetiq, Hyperbeat, HyperLend, Based** are the four confirmed VC-backed projects with disclosed rounds between $1.7M and $11.5M. **All raised from crypto-native VCs plus market makers (IMC, Flowdesk, SIG, Susquehanna) — the investor list itself is the BD shortlist.**
- **Felix, HypurrFi, Unit** have either no disclosed round or only a backer name on a website — handle these as "verify on a call, do not assert a number."
- **HypurrFi is winding down** its Mewler market with Euler taking over operations (announced May 15–16, 2026), and suffered a domain compromise April 3, 2026. This materially changes its BD value — [established].
- **Trade[XYZ]** (built by the Hyperunit team) controls **98% of HIP-3 volume across 92 markets** as of July 2026 — an important door for institutional capital formation via HIP-3 markets.

## 2. Hiring map — BD / ecosystem / institutional / capital-markets

| Entity | Role evidence | Source (date) | Status |
|---|---|---|---|
| **Hyperliquid Labs** | Two open roles: Backend Engineer (APAC, on-site), Senior Frontend Engineer (APAC, on-site) — **no BD/Growth** | Ashby board, Aug 2026 | [established] — engineering-only |
| Hyperliquid Labs (older) | "Business Development, Lead" posting, last seen Aug 22, 2024 | Remote3, Aug 22, 2024 | **Stale — 2 years old.** Sterling B listed as "BD Lead at Hyperliquid Labs" on LinkedIn [low-authority, single source] |
| **Hyperliquid Policy Center (HPC)** | Founded Feb 18, 2026 by Jake Chervinsky as a 501(c)(4) research & advocacy shop; **$28–29M HYPE donation** from Hyper Foundation | Fortune, Feb 18, 2026; hyperliquidpolicy.org launch PR; team page | **Founder-led; no public job listings** — the door is direct, not via ATS |
| **Ripple Prime** | Enabled support for Hyperliquid (Feb 4, 2026); institutional clients access HL via Ripple Prime as sole counterparty | Ripple PR, Feb 4, 2026; The Block | Hiring for Ripple Prime BD/institutional sales is separate but HL-relevant |
| Kinetiq | LinkedIn shows 5 employees (Magnus Lai, Dalton Weeks, Angel Sanchez, Adrián Ayora + 1) | LinkedIn, retrieved Aug 2026 | No public BD posting found [no source found] |
| HypurrFi | Site markets "Looking to list on Hyperliquid? Chat with us" — implying a partner/listing BD function | HypurrFi site [single-source] | **Beware: protocol is winding down per Euler takeover** |
| Hyper Foundation | No public job board; funds $10M grants (USDH migration, June 2026) and $29M to HPC | KuCoin, Jul 1, 2026; The Block, Feb 18, 2026 | Grants, not jobs |
| Hyperbeat | Post-raise scaling, TVL >$340M as of Aug 2025 | Bloomingbit, Aug 15, 2025 | No public BD posting found [no source found] |

**The hiring landscape in one sentence:** Hyperliquid core is engineering-only (APAC, on-site, <15 team); the BD door is at the **Hyperliquid Policy Center** (direct to Chervinsky, no ATS), at **VC-backed ecosystem projects** that raised in 2025–2026 and now need institutional distribution, and at the **institutional wrappers** (Ripple Prime, Anchorage Digital, Fireblocks) building Hyperliquid access.

## 3. Decision-makers and channels

| Project | Person | Role | Public channel | Source |
|---|---|---|---|---|
| Hyperliquid core | **Jeff Yan** | Founder/CEO, ex-Hudson River Trading quant; turned down outside capital repeatedly | X @HyperliquidX (official); **personal handle not confirmed in sources — do not guess** | Binance interview, Jul 18, 2025; blocmates, Aug 20, 2025; Colossus profile |
| Hyperliquid core | iliensinc | Co-founder (pseudonymous) | — | CryptoSlate, Jun 21, 2026 |
| Hyper Foundation | (org account) | Token, governance, grants | X @HyperFND | X |
| **Hyperliquid Policy Center** | **Jake Chervinsky** | Founder & CEO; ex-Variant CLO, ex-Blockchain Association CPO, ex-Compound GC | X @HyperliquidPolicy; HPC team page | Fortune, Feb 18, 2026 |
| Kinetiq | Omnia.hl (@0xOmnia) | Co-founder, pseudonymous | X @0xOmnia; @kinetiq_xyz | KevinWSHPod interview, Aug 2026; IQ.wiki |
| Kinetiq | **Magnus Lai** | Co-founder & Co-CEO (public-facing) | LinkedIn /in/magnus-lai-67541839; Kinetiq LinkedIn | Anchorage PR quotes him |
| Kinetiq | Justin Greenberg | Co-founder | LinkedIn (limited) | IQ.wiki [single-source] |
| Felix Protocol | **Charlie Ambrose (@0xBroze / Charlie.hl)** | Co-founder; also founder of Anthias Labs | X @0xBroze; LinkedIn /in/charlieambrose | Named in Hyperion PR — [established], multiple sources |
| HypurrFi | Androo (@androolloyd) | Co-founder | X @androolloyd | Binance Square, Apr 4, 2026; decentralised.co podcast |
| HypurrFi | Andy | Growth lead | — | decentralised.co, Aug 19, 2025 |
| Unit / Hyperunit | Pseudonymous collective | No public identified leaders | — | Datawallet — **do not guess handles** |
| Based | Edison Lim | Co-founder & CEO | — | The Block, Feb 23, 2026 |
| Hyperliquid Strategies (PURR) | David Schamis | CEO | — | PRNewswire, Aug 27, 2026 |
| Hyperbeat | No public named founder surfaced | — | — | [no source found] — verify via CoinDesk article |

**Outreach hygiene:** Kinetiq's X handle is @kinetiq_xyz; Felix's Charlie is @0xBroze; HPC is @HyperliquidPolicy. For **Unit, HypurrFi (post-compromise) and Hyperbeat**, no verified personal handles are corroborated by at least two sources — flag these as unverified rather than risk a wrong ping.

## 4. How institutional onboarding actually works (Aug 2026)

Hyperliquid **does not run a traditional market-maker program**. The official docs state plainly: *"There is no DMM program, special rebates / fees, or latency advantages."* Onboarding is therefore **permissionless on execution, but institutionally wrapped** by four layers:

**1. Execution & fees (permissionless).** Base perps fee 0.045% taker / 0.015% maker with 14-day rolling-volume tiers and HYPE-staking discounts; maker rebates paid continuously to the trading wallet. High-volume makers (top 0.5% of 14-day platform activity) can reach effective negative fees. No KYC; you connect an EVM wallet.

**2. Custody (institutional wrappers) — this is where the intermediaries live:**
- **Anchorage Digital** — federally chartered bank; qualified custody of HYPE on HyperEVM + HyperCore, HYPE staking via Figment partnership, and kHYPE via direct Kinetiq integration (Anchorage PR, Dec 17, 2025). Self-describes as *"the institutional home for Hyperliquid."*
- **Fireblocks** — published Aug 2026 institutional approval-workflow guidance for Hyperliquid perps; $11.5B+ open interest and 9.3% of global aggregate perp OI cited as the demand driver.
- **Ripple Prime** — added Hyperliquid Feb 4, 2026 as its **first DeFi venue**; institutions face Ripple Prime as sole counterparty and cross-margin HL perps against FX, fixed income, OTC swaps.

**3. Vaults & liquidity provision.** HLP is the protocol-owned vault running market-making and liquidations; 4-day lockup, no performance fee, profits socialized pro-rata to depositors. Anyone with USDC can deposit — this is the "democratized market-maker" product, **not** a relationships-based MM program. User vaults let any trader run a strategy and attract depositors. Market-making itself is permissionless via the public API/SDK (Python, Go, Rust) — teams compete on infrastructure, not on signed agreements.

**4. HIP-3 deployers (the new "capital formation" layer).** Deployers stake **500,000 HYPE** to run a perps market with custom oracle, leverage, and parameters. **Trade[XYZ]** (Hyperunit team) has 92 markets and 98% of HIP-3 volume as of July 2026 — the most realistic venue for launching pre-TGE perps and tokenized-equity markets. **Markets by Kinetiq** is the other major HIP-3 deployer.

**Who actually intermediates institutional flow today:**

| Intermediary | Role |
|---|---|
| Anchorage Digital | Qualified custody + staking + kHYPE |
| Fireblocks | MPC infrastructure + workflow tooling |
| Ripple Prime | Prime brokerage counterparty with cross-margin to TradFi |
| FalconX | Runs validator with Chorus One; publishes HIP-3 research |
| Hyperliquid Strategies (PURR) | Public-market HYPE treasury; jointly launched validator with Unit Labs |
| Hyperion DeFi (HYPD) | Public-market HYPE treasury; uses HYPE as collateral via HyperLend private pool |
| Unit (Hyperunit) | Tokenization rails (UBTC/UETH/USOL); trade.xyz HIP-3 DEX; $64B cumulative Unit-spot volume |
| Kinetiq | Liquid staking (kHYPE) + Anchorage integration + HIP-3 deployer |

## 5. Honest check — does Hyperliquid core still refuse VC money / external hires?

**The "no VC" position is corroborated and still the stated policy [established]:**
- Jeff Yan, 2025 interview: *"We started from scratch. No need for funding, so the decision was simple."* He previously engaged investors in January 2024 but only on conditions of maintaining independence.
- blocmates, Aug 20, 2025: *"Hyperliquid has rejected outside capital entirely."*
- Colossus profile: Yan "turned down $100 million."
- Yellow Research 2026 forensic breakdown: *"no venture capital, no token presale, no outside investors"*; ~31% airdropped, zero VC allocation.
- Amina Group / Grayscale / CoinShares research all repeat the structure: 97–99% of fees flow to the Assistance Fund for buyback-and-burn.
- Governance vote Dec 2025 (85% of staked weight) formally recognized Assistance Fund HYPE as permanently burned.

**But the "no external hires" framing needs nuance — steelmanning both sides:**

| Position | Evidence for | Evidence against |
|---|---|---|
| "Hyperliquid Labs is engineering-only, tiny, doesn't hire BD" | Ashby board shows only 2 engineering roles (APAC, on-site) as of Aug 2026; "<15 people" repeatedly cited; team expanded from ~11 with 1.2M HYPE distributed to core team Jan 2026 | An old BD Lead role **did** exist (Remote3, Aug 2024), and LinkedIn shows a Sterling B as "BD Lead at Hyperliquid Labs" [low-authority, single source]. This suggests BD was tried, possibly retained, but is not actively hiring. |
| "Hyperliquid doesn't take VC but does fund ecosystem via Hyper Foundation" | $10M USDH migration grants (June 2026); $29M HYPE to Hyperliquid Policy Center (Feb 2026) | These are **grants and donations, not equity investment** — consistent with the "no VC" stance rather than contradicting it. |

**What would settle it:** a Hyperliquid Labs opening for a non-engineering role, or a Hyper Foundation equity investment. Neither exists in current sources.

**The most realistic non-engineering door into the core org is via the Hyperliquid Policy Center** — Chervinsky's shop is an independent 501(c)(4) with $28–29M behind it, no public job listings, and a mandate that is exactly capital-formation-adjacent (creating legal pathways for perps).

## 6. Top 5 doors for a candidate with MM/HF network, exchange-infrastructure background, $35M fund track record

| Rank | Door | Why this profile fits | Concrete first action | Risk / trade-off |
|---|---|---|---|---|
| **1** | **Hyperliquid Policy Center — direct to Jake Chervinsky** | You'd run capital-markets narrative, market-structure research, and institutional relationships for the most-watched perp DEX; your MM/HF network is exactly what HPC needs to brief policymakers on real market-quality data | Email via hyperliquidpolicy.org; cite the launch PR and propose a **specific research deliverable** (e.g. an MM-on-HL market-quality paper) | No public listings → slower, relationship-driven close; nonprofit comp below market; HPC is independent from Hyperliquid Labs so equity upside is zero |
| **2** | **Trade[XYZ] / Hyperunit — institutional lead for HIP-3 markets** | 98% of HIP-3 volume, 92 markets, run by an ex-HRT/Jump/Fortress collective — your exchange-infrastructure background and MM network map directly onto their HIP-3 deployer expansion | Identify via trade.xyz docs and the Unit validator partnership with Hyperliquid Strategies; reach out through validator-partner intros | Team is pseudonymous; no disclosed funding → comp may be token-only; verify entity structure carefully |
| **3** | **Kinetiq — ecosystem / institutional BD for Markets + Elysium** | Kinetiq announced the first Hyperliquid L2 (Elysium) in Aug 2026 and needs institutional distribution for kHYPE (Anchorage integration is their template) and for Markets by Kinetiq (HIP-3 deployer with Kaiko oracles, CBOE-grade data partners) | LinkedIn to Magnus Lai (co-CEO, public-facing), or @kinetiq_xyz on X; reference the Kaiko/CBOE partnership as your entry wedge | $1.8M seed is a small raise → thin equity pool; team partially pseudonymous; two co-founders share CEO-type authority |
| **4** | **Based — Head of Institutional (Series A)** | $11.5M Series A closed Feb 2026, Pantera has observer board seat, building the consumer "SuperApp" on Hyperliquid; your exchange-infrastructure background is rare in a consumer-first team and Pantera will want an institutional arm | Email via Based press/LinkedIn; reference Pantera's investment memo and your MM network as distribution for their wallet/trading stack | Consumer SuperApp focus may underweight institutional; Edison Lim is CEO — single decision-maker |
| **5** | **Ripple Prime / Anchorage Digital — Hyperliquid product lead** | Both are hiring institutional-facing roles adjacent to HL; Ripple made HL its first DeFi venue Feb 2026 and Anchorage is building "the institutional home" — a capital-formation profile with an MM/HF Rolodex is the exact missing hire | Apply through Ripple Prime / Anchorage careers; reference the HL integration PRs as the product you'd expand | You're building *on* Hyperliquid, not *for* it — less ecosystem-equity upside; bigger orgs = slower decisions |

### Single recommendation

**Lead with Door #1 (HPC) and Door #3 (Kinetiq) in parallel.** Confidence tier: **[emerging]** — both doors are real and open, but neither has a published job spec you can point to; both depend on proactive outreach.

**Main assumption:** that Chervinsky's HPC is building out its policy-and-research bench through H2 2026 as the CFTC/SEC framework debate intensifies, and that Kinetiq is staffing the Elysium L2 launch — the Elysium announcement is <2 weeks old and the $1.8M seed means they will need an institutional-distribution hire before mainnet.

- **Why not #1 alone:** HPC is a nonprofit with unknown budget horizon; pairing it with Kinetiq (token upside, smaller raise) hedges the comp risk.
- **Why not #4 first:** Based is consumer-first; your institutional Rolodex is more valuable at the HIP-3/validator layer (Hyperunit/Trade[XYZ]) than at the retail wallet layer.
- **⚠️ Avoid:** HypurrFi BD outreach — the protocol is winding down its Mewler market as Euler takes over operations (May 2026) and suffered a domain compromise (April 2026). Unless they've announced a pivot, this door is closing.

## Source list

**Funding / raises**
- Kinetiq $1.8M seed — crypto-fundraising.info, Oct 26, 2025 — https://crypto-fundraising.info/blog/dates-19-25-oct-2025
- Kinetiq $1.75M seed — CypherHunter, Oct 22, 2025 — https://www.cypherhunter.com/en/e/kinetiq-xyz-raised-funding-2025-10-22
- Hyperbeat $5.2M seed — CoinDesk, Aug 15, 2025 — https://www.coindesk.com/tech/2025/08/15/hyperbeat-secures-usd5-2m-backing-from-ether-fi-electric-capital
- Hyperbeat $5.2M seed — Bloomingbit, Aug 15, 2025 — https://en.bloomingbit.io/feed/news/94916
- HyperLend $1.7M round — CryptoRank X post, Jan 20, 2026 — https://x.com/CryptoRank_VCs/status/2013558097986392550 [single-source]
- Based $11.5M Series A — The Block, Feb 23, 2026 — https://www.theblock.co/news/deals/2026-02-23-hyperliquid-web3-based-funding-pantera-390809
- Based — Pantera Capital memo, Feb 23, 2026 — https://panteracapital.com/investing-in-based/
- Hyperliquid Strategies $646.6M / $1B S-1 — Cryptorank, Aug 28, 2026 — https://cryptorank.io/news/feed/3aa65 ; Yahoo Finance, Oct 22, 2025 — https://finance.yahoo.com/news/hyperliquid-strategies-files-raise-1b-064341355.html
- Hyperion–Felix partnership (Felix TVL) — Hyperion IR, Oct 29, 2025 — https://ir.hyperiondefi.com/news-events/press-releases/detail/290
- Hyperion–HyperLend private lending pool — Hyperion IR, Mar 9, 2026 — https://ir.hyperiondefi.com/news-events/press-releases/detail/304
- Felix $1.7M pre-seed (**Prague AI company — NOT Hyperliquid Felix**) — TheSaaSNews, Apr 7, 2026 — https://www.thesaasnews.com

**Hiring / org**
- Hyperliquid Labs Ashby job board — retrieved Aug 2026 — https://jobs.ashbyhq.com/Hyperliquid%20Labs
- Hyperliquid Labs BD Lead (stale, Aug 2024) — https://www.remote3.co/remote-jobs/business-development-lead-hyperliquid-labs
- Sterling B, BD Lead — https://www.linkedin.com/in/sterlingbarnett [low-authority, single source]
- HPC launch PR — hyperliquidpolicy.org, Feb 18, 2026 — https://hyperliquidpolicy.org/blog/launch-press-release ; team page — https://hyperliquidpolicy.org/team
- HPC launch — Fortune, Feb 18, 2026 — https://fortune.com/2026/02/18/hyperliquid-launches-defi-focused-policy-shop-led-by-prominent-crypto-lawyer-jake-chervinsky/
- HPC $29M HYPE donation — The Block, Feb 18, 2026 — https://www.theblock.co/news/regulation/2026-02-18-hyperliquid-foundation-sets-up-defi-policy-advocacy-group-with-29-million-hype-token-donation-390326
- HPC on Blockchain Association job board (no open jobs) — https://jobs.theblockchainassociation.org/companies/hyperliquid-policy-center
- Hyper Foundation USDH $10M grants — KuCoin, Jul 1, 2026 ; crypto.news, Jun 29, 2026 — https://crypto.news/hyper-foundation-allocates-10m-in-grants-to-support-usdh-migration/
- Hyperliquid Labs 1.2M HYPE to core team — Cryptorank, Dec 28, 2025 — https://cryptorank.io/news/feed/45d4d
- Hyperliquid Labs "<15 people" — https://cryptocurrencyjobs.co/startups/hyperliquid
- Ripple Prime–Hyperliquid integration — Ripple PR, Feb 4, 2026 — https://ripple.com/ripple-press/ripple-announces-support-for-hyperliquid-expanding-institutional-access-to-onchain-liquidity ; The Block — https://www.theblock.co/news/defi/2026-02-04-ripple-prime-hyperliquid-defi-388454
- Kinetiq LinkedIn (team) — https://www.linkedin.com/company/kinetiq-research
- Anchorage–Kinetiq kHYPE integration — Anchorage, Dec 17, 2025 — https://www.anchorage.com/insights/anchorage-digital-support-khype-through-integration-kinetiq
- Anchorage institutional staking (Magnus Lai quote) — https://www.anchorage.com/platform/staking
- Anchorage "institutional home for Hyperliquid" — Jun 16, 2026 — https://www.anchorage.com/insights/anchorage-digital-is-the-institutional-home-for-hyperliquid

**Decision-makers**
- Jeff Yan no-VC interview — Binance, Jul 18, 2025 ; blocmates, Aug 20, 2025 ; Colossus profile — https://colossus.com
- Kinetiq founders (Omnia, Magnus, Justin) — IQ.wiki, Oct 16, 2025 — https://iq.wiki/wiki/kinetiq
- Kinetiq Omnia interview — KevinWSHPod X, Aug 2026 — https://x.com/KevinWSHPod/status/2093292663554580794
- Felix co-founder Charlie Ambrose — Hyperion PR via Nasdaq, Oct 29, 2025 — https://www.nasdaq.com/press-release/hyperion-defi-announces-strategic-partnership-felix-launch-hip-3-powered-perpetual ; X @0xBroze — https://x.com/0xBroze
- HypurrFi founders Androo & Andy — decentralised.co, Aug 19, 2025 — https://www.decentralised.co/p/ep-45-making-debt-move-faster-with ; Binance Square, Apr 4, 2026
- Based CEO Edison Lim — The Block, Feb 23, 2026
- Hyperliquid Strategies CEO David Schamis — PRNewswire, Aug 27, 2026 — https://www.prnewswire.com/news-releases/hyperliquid-strategies-inc-reports-financial-results-for-the-fiscal-year-ended-june-30-2026-302861372.html
- Hyper Foundation X — https://x.com/HyperFND
- iliensinc — CryptoSlate, Jun 21, 2026 — https://cryptoslate.com/companies/hyperliquid
- Magnus Lai LinkedIn — https://au.linkedin.com/in/magnus-lai-67541839

**Institutional onboarding / market structure**
- Hyperliquid market-making docs (no DMM program) — Aug 17, 2026 — https://hyperliquid.gitbook.io/hyperliquid-docs/trading/market-making
- Fees docs — https://hyperliquid.gitbook.io/hyperliquid-docs/trading/fees
- HLP vault docs — https://hyperliquid.gitbook.io/hyperliquid-docs/hypercore/vaults/protocol-vaults
- Builder codes docs — https://hyperliquid.gitbook.io/hyperliquid-docs/trading/builder-codes
- HIP-3 spec (500K HYPE stake) — https://hyperliquid.gitbook.io/hyperliquid-docs/hyperliquid-improvement-proposals-hips/hip-3-builder-deployed-perpetuals
- Trade[XYZ] 92 markets / 98% HIP-3 volume — KuCoin, Jul 15, 2026 — https://www.kucoin.com/news/flash/trade-xyz-builds-92-markets-and-98-hip-3-volume-on-hyperliquid-in-8-months
- Unit / Hyperunit explainer — https://www.datawallet.com/crypto/hyperunit-explained
- Unit + Hyperliquid Strategies validator — Investing.com, May 7, 2026 — https://www.investing.com/news/company-news/hyperliquid-strategies-and-unit-labs-launch-new-validator-node-93CH-4669861
- Phantom $20.6M builder-code revenue — CoinGecko, May 26, 2026 — https://www.coingecko.com/research/publications/top-hyperliquid-builders
- pvp.trade $7.2M lifetime builder revenue — Dwellir, Jan 16, 2026 [low-authority]
- Insilico Terminal fees — DefiLlama
- FalconX HIP-3 research — https://www.falconx.io/newsroom/the-transformational-potential-of-hyperliquids-hip-3
- Fireblocks institutional workflows — https://www.fireblocks.com/blog/institutional-approval-workflows-hyperliquid
- Kinetiq Elysium L2 announcement — crypto.news, Aug 25, 2026 — https://crypto.news/kinetiq-unveils-elysium-l2-with-hype-gas/
- HypurrFi wind-down / Euler takeover — Binance Square, May 15, 2026 ; Cryptotimes, May 16, 2026
- HypurrFi domain compromise — Binance Square, Apr 4, 2026 — https://www.binance.com/en/square/post/308854710432770 ; Finance.biggo, Apr 3, 2026
- Assistance Fund burn governance — Investing.com, Aug 31, 2026 — https://www.investing.com/analysis/hype-the-18-billion-question--can-hyperliquid-outgrow-its-valuation-200686830
- Hyperliquid no-VC forensic breakdown — Yellow Research 2026 — https://yellow.com/research/hyperliquid-no-vc-funding-dex-volume-record
- CoinStats Hyperliquid investment analysis — Aug 1, 2026 — https://coinstats.app/ai/a/investment-analysis-hyperliquid
