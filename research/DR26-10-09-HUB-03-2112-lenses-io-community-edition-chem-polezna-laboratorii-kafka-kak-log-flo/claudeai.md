# DR26-10-09-HUB-03-2112 — raw report from claudeai

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Lenses.io Community Edition for a Small AI-Agent Lab: Use It as a Subject to Write About, Not as Your Agent Bus

**Bottom line:** Lenses Community Edition (CE) is worth a week of your time as content and security-analysis material, not as infrastructure. Its MCP server is a well-documented, current example of "governed agent access to infrastructure", and it maps directly onto your zero-trust-for-agents thesis. But it is the wrong tool to fix your four-rail delivery-log drift. It is a closed-source, revocable, two-environment-limited UI layer that needs at least 5 GB of Docker memory, sitting on top of a single-broker Kafka your offline-prone fleet can't reliably reach.

## TL;DR
- **Start with Track 5 + Track 2 (content and a security teardown), GO, [emerging].** Write an independent analysis of the Lenses MCP OAuth 2.1 design: three coarse scopes, scopes "not enforced globally", introspection with no client authentication, and forwarding of bearer tokens. Compare it with Confluent's and Redpanda's designs and with the MCP authorization spec, then turn the lessons into features of agent-approval-gate. Prior art for this exact teardown was not found.
- **Track 1 (Lenses + Kafka as your one delivery log): NO-GO now, LATER at most, [established] on the cost side.** Your problem is four independent counters, not a missing broker. A single-node Kafka on a Windows hub adds a single point of failure for intermittently offline machines. The CE EULA also lets Lenses terminate your license "at any time and for any reason". Fix the drift with one append-only log you own first.
- **Tracks 3 and 4 (OSS contributions, hiring): LATER / low-yield for Lenses itself, GO for the wider category.** Lenses is a small Celonis subsidiary: roughly 24–28 staff per third-party profiles, acquired October 2021, not 2024. Its public repos show little outside-contributor flow. Redpanda is openly hiring Forward Deployed Engineers for agent infrastructure (UK base £131,914–£155,194), which is a far better use of a "Kafka + MCP + agents" portfolio.

## A. Executive Summary

**Track 1 — INFRA (one delivery log).** Lenses CE is a control plane and UI, not a log. The quickstart's Docker Compose starts Postgres, Lenses HQ, a Lenses Agent and a single Kafka broker, and the docs say Docker "must allocate at least 5GB of memory" (Lenses docs, "Setting up Community Edition", current 6.2 docs). The alternatives are much lighter. Core NATS uses about 11 MB RSS at 2M msgs/sec (NATS docs issue #321, older source). Redpanda idles around 349 MiB with Redpanda Console at about 42 MiB (computingforgeeks, 2026, [low-authority]). Kafbat UI and AKHQ are single Apache-2.0 containers. What actually hurts you is drift between four counters, and adding Kafka doesn't remove a rail unless every agent writes to it, including Macs and the VPS while they're offline from the hub. Kafka on one Windows box makes that harder than Syncthing does, not easier. **Verdict: NO-GO now; LATER only if a file-based single log fails. Confidence: [established] for the resource and operational costs, [emerging] for the drift diagnosis.**

**Track 2 — SECURITY / REFERENCE ARCHITECTURE.** The Lenses MCP server is a FastMCP (Python, Apache-2.0) resource server. It publishes RFC 9728 protected-resource metadata, uses Lenses HQ as the OAuth authorization server with Dynamic Client Registration and PKCE S256, validates tokens by RFC 7662 introspection, and forwards the valid bearer token to the Lenses API, which makes the final authorization decision (lensesio/lenses-mcp README, repo updated Sep 7, 2026). It has some good features. Destructive operations stay behind a human click in the UI. Audit is per user. Revocation is effectively immediate because the introspection cache TTL defaults to 0. Every MCP request emits an OpenTelemetry span. It also has notable gaps. There are only three scopes (read/write/delete). The README states "scopes are not enforced globally" and that per-tool enforcement "can be added". Introspection must run without client authentication (`unauthenticatedIntrospection: true`). The documented introspection fields don't include audience. **Verdict: GO, as the thing you analyze, copy patterns from and contribute fixes to. Confidence: [emerging].**

**Track 3 — OSS CONTRIBUTION SURFACE.** lenses-mcp has 28 stars, 3 forks, 0 open issues, 0 open PRs and no top-level CONTRIBUTING.md. agentic-engineering-for-apache-kafka is MIT-licensed, with 61 stars and 1 open issue. stream-reactor has about 1.1k stars, 86 open issues and about 89–92 open PRs, most of which appear to be Scala Steward bot PRs that close themselves as "superseded". Its releases are merged from a separate `lensesio-dev` org, which points to internal development with public PRs imported by script (subagent verification of github.com/orgs/lensesio, Oct 9, 2026). No human outside-contributor merges were visible in the last 90 days. **Verdict: LATER. Make one targeted docs/security PR to lenses-mcp alongside the Track 2 post, and don't plan a contribution campaign. Confidence: [emerging].**

**Track 4 — HIRING.** Celonis acquired Lenses.io in October 2021: its press release "Celonis Ushers in the Era of Data Execution" (PRNewswire, New York and Munich, Oct. 5, 2021) announced "Celonis Executes Acquisition of Lenses.io", and CB Insights notes "The terms of the transaction were not disclosed." The GitHub org lists "UK (London) USA (New York) - Athens/Amsterdam". Third-party headcounts range from 24 (PitchBook) to 28 (Built In), while Glassdoor's "51 to 200" figure looks stale. Lenses has career pages for Developer Advocate, Support Engineer and Kafka SRE, but I could not verify whether those roles are currently open [unverified]. The best-fit employers in this category are elsewhere. Redpanda is hiring Forward Deployed Engineers for its Agentic Data Plane in US West, US East, EMEA and Networking. Confluent, now part of IBM since March 17, 2026, is hiring for Streaming AI and Applied AI roles. **Verdict: NO-GO for targeting Lenses specifically; GO for using a Kafka + MCP + agent-governance portfolio to target Redpanda and Confluent/IBM. Confidence: [emerging].**

**Track 5 — CONTENT / DEMO.** The vendor-produced material already covers "agent drives Kafka through MCP": Lenses' three Claude Code workflows (May 2026), Confluent's MCP and Agent Skills GA, Kong's "Agentic Commit Log" blueprint (Jun 17, 2026) and AutoMQ's multi-agent coordination-log posts. What's missing is an independent, adversarial review of how these "governed MCP" designs actually enforce scope, audience and approval, written by someone shipping agent-approval tooling. **Verdict: GO, first. Confidence: [emerging].**

## B. Key Findings

1. **Lenses CE is free but tightly fenced by its EULA.** [established, primary source] The Community Edition EULA ("Last update: August, 2025") grants a "non-transferable, non-sublicensable, non-exclusive, worldwide license" and states: "You are entitled to install the Software in two environments only." It also prohibits you from:
   - "publicly disseminat[ing] performance information or analysis (including, without limitation, benchmarks)";
   - reverse engineering;
   - "modify, copy, translate or create derivative works";
   - distributing or sublicensing;
   - using the Software "to develop a product that is functionally similar".

   It says "We may terminate this Agreement and Your access to the Software at any time and for any reason," and "WE MAY IN OUR DISCRETION CHANGE THESE TERMS… WITHOUT NOTICE TO YOU." It has no fixed expiry date and no explicit telemetry or phone-home clause. Data handling is delegated to the Celonis Privacy Notice, so I found no source confirming or ruling out phone-home behaviour [no source found]. Liability is capped at US$100. **Implication for you:** a public post benchmarking Lenses CE latency or footprint against Redpanda or Kafbat would arguably breach clause 4.2(i). Describe the design; don't publish Lenses performance numbers.

2. **Lenses' own pages disagree on CE limits.** [emerging] The pricing page says "Up to 5 free users, Basic Auth, Lenses UI & MCP, Supports any Kafka". The 6.0 announcement says up to 2 Kafka clusters. The EULA says two environments. One MCP docs page says CE is "restricted by number of users and enterprise features, e.g. OAuth", while another says "e.g. SSO", and the May 2026 blog says OAuth 2.1 is the default and free in CE. The likely reading is that OAuth for MCP is in CE and SSO/SAML is not, but confirm this in a live install before publishing.

3. **The MCP server's design is good and teachable, and its weaknesses are concrete.** [emerging; primary source, README]
   - *Good:* RFC 9728 discovery at `/.well-known/oauth-protected-resource/mcp`; RFC 8414 metadata; PKCE S256; DCR; `INTROSPECTION_CACHE_TTL` default 0, so revocation takes effect on the next call; OTel spans per `tools/call` that carry the tool name and session id; and a deliberate "agents compose, humans commit" boundary. In the Lenses demo, a newly created SQL processor stays `NOT_RUNNING` until a human clicks start, and the MCP surface exposes no processor restart (Lenses blog, Jeremy Frenay, May 11, 2026). The blog advertises 43 tools.
   - *Gaps:*
     - (a) "Scopes are not enforced globally at the introspection level — a token with any subset of these scopes is accepted." Read-only sessions therefore depend on the downstream Lenses API, or on FastMCP `require_scopes` decorators being added.
     - (b) Lenses HQ must run with `unauthenticatedIntrospection: true`. RFC 7662 recommends that the introspection endpoint require authorization so it can't be used for token scanning.
     - (c) The documented introspection response lists `active`, `scope`, `client_id` and `exp` but not `aud`. The MCP spec (2025-11-25) says servers "MUST validate that access tokens were issued specifically for them as the intended audience" and "MUST NOT pass through the token" to upstream APIs. Lenses forwards the client's bearer token to the Lenses API. That is defensible only because HQ is both the issuer and the upstream, and the README doesn't say so.
     - (d) Three coarse scopes. Lenses itself says "finer-grained scopes and content-aware controls are landing soon".
     - (e) The API-key fallback keeps a long-lived static credential path alive.

4. **Lenses 6.2 is the "agentic" pivot and is recent.** [established] The Lenses 6.2 release post by Andrew Stevenson (byline April 15, 2026; page metadata says June 17, 2026) introduced OAuth 2.1 "as demanded by the MCP protocol" and packaged a remote MCP server inside Lenses. It describes the earlier setup, in which engineers created "a long-lived, static Lenses API key with broad production access" stored in a local config file, as the security problem it solves. It also added a VS Code extension preview, Studio (formerly SQL Studio), K2K replicator improvements and app migration workflows, and promised CIMD and DCR support. Lenses also publishes `agentic-engineering-for-apache-kafka`, MIT-licensed "Kafka skills" for agents (connector review, Python client, ShadowTraffic), which says it is "MCP-agnostic by design".

5. **The Celonis acquisition happened in October 2021, not 2024.** [established] Celonis announced it on October 5–6, 2021, alongside Execution Graph (Celonis press; SiliconANGLE Oct 5, 2021; Marathon VC). Factor House says "early 2022" [single-source, competitor-authored], which conflicts with these primary sources. Lenses still operates as Lenses.io Ltd: the EULA is in that name and incorporates the Celonis Privacy Notice.

6. **The competitors are lighter, more open or both.** [emerging; most comparison data comes from Factor House (vendor of Kpow, a competitor) and should be treated as low-authority]
   - Kafbat UI (Apache-2.0, fork of the abandoned Provectus kafka-ui; v1.5.0 released 20 Apr 2026 per the kafbat/kafka-ui GitHub release page, adding MessagePack serde support, Swagger UI, live consumer-lag updates and CSV export) and AKHQ (Apache-2.0; 0.28.0 released 6 Aug 2026 per the tchiotludo/akhq GitHub releases page, adding audit events for record produce, delete and empty topic) have no user or cluster caps.
   - Redpanda Console is BSL. SSO, RBAC and masking require a Redpanda Enterprise license.
   - Conduktor Community Edition allows 3 clusters and 50 users. Conduktor "overhauled our free license" in Console v1.43.0 (Conduktor release notes, Feb 2026), sunset Conduktor Desktop, and says Console needs 2 CPU, 3 GB RAM and 5 GB disk minimum.
   - Lenses CE allows 5 users and needs at least 5 GB of Docker memory.

7. **The Kafka substrate has changed, and Kafka is easier to run alone than it used to be.** [established] Apache Kafka 4.0 (March 18, 2025) removed ZooKeeper, making KRaft the only mode. It made the KIP-848 consumer protocol GA and shipped KIP-932 share groups (queues) as early access (Apache upgrade docs; Confluent blog; InfoQ, April 2025). The 4.x line has continued: the Apache Kafka downloads page lists 4.3.1 "Released June 25, 2026" and 4.2.2 "Released September 29, 2026", and Red Hat's Kafka Monthly Digest (Mar 2026) reports "Share groups (queues) are now ready for production usage. (KIP-932)". KIP-1150 "Diskless Topics", cited in the April 2026 arXiv paper AgileLog, has been accepted: Aiven's Josep Prat wrote (Mar 12, 2026) that "On March 2, 2026, the vote to accept Diskless topics into Apache Kafka passed with overwhelming support of 9 binding votes and 5 non-binding ones", though implementation KIPs 1163/1164 are still under discussion and diskless topics are not yet GA (SoftwareMill, Aug 25, 2026).

8. **"Governed MCP access" is now a crowded vendor category.** [established]
   - **Confluent:** an open-source `@confluentinc/mcp-confluent` with OAuth (PKCE) for Confluent Cloud. Only some tool categories work under OAuth; the rest still need static API keys. It collects telemetry unless `DO_NOT_TRACK=true`. Confluent also runs a managed MCP server governed by existing RBAC (Confluent docs and blog, 2026).
   - **Redpanda:** Agentic Data Plane (launched Oct 28, 2025; AI Gateway, MCP governance, OIDC identity and OTel observability added Feb 18, 2026), with "an admin-managed approach to aggregating and controlling MCP servers" (Redpanda press release).
   - **Kong:** AI Gateway plus Event Gateway, which taps every LLM and tool call into Kafka (Kong blog, Jun 17, 2026).

   Lenses' version is the most self-hostable and free-to-inspect of these. Redpanda's is the most complete as a gateway.

9. **Kafka-as-agent-log is now a published pattern, but the evidence is mostly vendor marketing.** [emerging] Sources include Kai Waehner (Confluent field CTO, May 26, 2025), AutoMQ's coordination-ledger series, Kong (Jun 2026), and a practitioner blog (novatechflow, Feb 2026, [single-source, low-authority]) claiming "15-minute debugging instead of hours". Academic work is converging on "the log is the agent": ActiveGraph, Nakajima, arXiv 2605.21997, May 22, 2026, and AgileLog, arXiv 2604.14590. Notably, ActiveGraph's runtime makes the log the source of truth without requiring Kafka. I found no independent, numbers-backed report of a 1–3 person team running a Kafka agent bus [no source found].

10. **Redpanda hiring is unusually aligned with your profile.** [emerging; job boards are aggregators, so verify on Ashby before applying] Redpanda lists Forward Deployed Engineer roles (US, Western US, Eastern US, EMEA-remote, Networking) whose postings name "Anthropic FDE, OpenAI Deployed Engineer… Palantir FDSE" as bonus experience, along with "Background in streaming… (Kafka, Flink, Redpanda)". The EMEA UK base range is £131,914–£155,194. Redpanda is fully remote, with HQ in San Francisco (Built In SF). Confluent lists "Senior Software Engineer – Streaming AI", "Distributed Systems Software Engineer – WarpStream" (US remote), "Principal Engineer, Engineering AI Productivity" ($285K–$342K, ZipRecruiter) and "Director, Engineering – Applied AI" ($322.5k–$387k, Remote Rocketship, posted Mar 12, 2026). I did not verify current openings at Aiven, StreamNative, Conduktor, Materialize, RisingWave or Decodable [no source found].

## C. Contradictions & Debates

- **"Kafka is the natural agent memory/coordination log" vs. "it's overkill for small teams".** *For:* Waehner, Kong, AutoMQ and Redpanda argue that durable ordering, replay, fan-out and consumer offsets are exactly what multi-agent systems lack. The steelman: your drift of 2,146 pending vs 2,227 applied marks is the classic symptom of having no single offset-tracked log, and Kafka consumer groups give you per-consumer offsets for free. *Against:* nearly every pro source sells streaming infrastructure. The ActiveGraph paper gets replay, forking and lineage from an append-only log without a broker. Your own NATS JetStream pilot (July 2026) was not adopted, which suggests the bottleneck is discipline (one writer path), not technology. *What would settle it:* a 7-day experiment in which every rail is rewritten as a projection of one log (file or Kafka), measuring whether the pending/applied gap goes to zero.
- **Is Lenses' OAuth design "enterprise-grade"?** Lenses: "Agents are only given scoped, short-lived, revocable and audited access". The steelman: per-user attribution, consent screens, immediate revocation and human-gated destructive operations are real improvements over the static API keys that most MCP servers, including parts of Confluent's, still rely on. The counterpoint, from Lenses' own README: scopes aren't enforced at the MCP layer, introspection is unauthenticated, and there are three coarse scopes. Both statements are true. The design is enterprise-grade on identity and immature on authorization granularity.
- **CE feature boundaries** (OAuth in or out of CE; 2 clusters vs 2 environments vs 5 users) conflict across Lenses' own pages. Treat the EULA as binding.
- **Acquisition date:** October 2021 (Celonis, SiliconANGLE, PitchBook, CB Insights) vs "early 2022" (Factor House). The primary sources win.
- **Source bias in the Kafka-UI landscape:** almost all recent comparison tables come from Factor House, which sells Kpow. Their factual license data (Apache-2.0, BSL) matches project licenses. Their scoring is marketing.

## D. Best Practices

1. **One log, many projections.** Make one append-only log authoritative and derive the stream, parcels, registry and Telegram mirror from it, each with its own consumer offset. This is Kleppmann's point, and it doesn't require a broker.
2. **Audience-bound, short-lived, introspected tokens.** Follow the MCP 2025-11-25 authorization spec: RFC 8707 `resource` on every authorization and token request, audience validation, no token passthrough, PKCE S256.
3. **Enforce scopes at the tool boundary, not only downstream.** Lenses' README concedes this is optional in their server. In your tools, a missing scope should mean the tool isn't registered at all.
4. **Keep commit actions human-gated outside the tool surface.** Lenses keeps processor start and connector restart in the UI. agent-approval-gate already does a version of this; make "not exposed as a tool" an explicit tier.
5. **Propagate trace context from the agent.** Lenses MCP emits one root span per call unless the client sends `traceparent` in `_meta`. To get the agent-run tracing your LangChain review said you lack, instrument the calling agent, not just the server.
6. **Read the EULA before publishing.** No Lenses benchmarks, no redistributed modified images, and stay within two environments.

## E. Failure Modes

- **Free tiers get withdrawn or reshaped.** Lenses Box used free developer licenses that "expire in six months" and had to be renewed by re-registering (Lenses 3.0 docs, older source). Conduktor overhauled its free license in Feb 2026 and shut down Desktop authentication. The Provectus kafka-ui project was abandoned and had to be forked as Kafbat. Lenses CE's EULA permits termination "at any time and for any reason". **Don't put a production dependency on a free proprietary UI.**
- **Lock-in through proprietary SQL and processors.** Lenses SQL processors and the Studio are not portable. If agents learn Lenses-specific tools via skills files, switching costs grow.
- **Single-hub Kafka with offline peers.** If the Windows hub sleeps, reboots for updates or loses Docker Desktop, every producer on the Macs and the VPS fails. They then need a local outbox, which is your file bus again. The VPS also needs inbound reachability to the hub through a tunnel. Syncthing tolerates partitions by design; a single broker does not.
- **Operational overhead for 1–3 people.** Even KRaft single-node needs retention, disk, JVM heap, listener and advertised-address configuration, and upgrade handling. With Lenses on top you add Postgres, HQ and an Agent. That's 4–5 always-on containers for a team that currently runs none.
- **Coarse MCP scopes give a false sense of read-only.** A "read" session is protected only by whatever the downstream API enforces.
- **Unauthenticated introspection** turns HQ into a token-validity oracle for anyone who can reach it. Never expose it beyond localhost or a trusted network.

## F. Strategic Recommendations (ranked)

| Rank | Track | Option | Trade-offs / risks | Verdict |
|---|---|---|---|---|
| 1 | 5 + 2 | **"Governed MCP, audited": an independent teardown of the Lenses, Confluent and Redpanda MCP authorization designs, plus a matching agent-approval-gate release.** It implements per-tool scope enforcement, audience checks and a "human-commit tier" | Cheap (CE runs locally), on-thesis, and gives hiring managers a portfolio signal. Risks: vendor docs change fast, and the EULA forbids benchmarks, so stick to design analysis | **GO** [emerging] |
| 2 | 1 (re-scoped) | **Collapse the four rails into one append-only JSONL/SQLite log with per-consumer offsets** (no broker), and add OTel spans from agents | Fixes the actual drift and keeps offline tolerance. Risk: you build it yourself and nobody "brands" it | **GO in parallel** [emerging] |
| 3 | 3 | **One PR to lenses-mcp**: a docs/security note on audience validation and the introspection trade-off, or per-tool `require_scopes` | Low effort and visible to the maintainers you'd cite. Risk: may go unanswered (no outside-PR merges observed) | **LATER (week 2)** [speculative] |
| 4 | 4 | **Apply to Redpanda FDE (EMEA or US) and Confluent/IBM Streaming AI roles** with the Track-1 post as portfolio | Direct fit to stated targets. Risk: postings move; verify on Ashby or careers.confluent.io | **GO after the post ships** [emerging] |
| 5 | 1 (original) | Lenses CE + Kafka as the fleet bus | ≥5 GB RAM, a single point of failure, proprietary and revocable | **NO-GO / LATER** [established] |

**Single recommendation:** spend the next week on Track 5 + 2. Ship an independent "governed MCP" teardown with a working agent-approval-gate patch that closes the gaps you found, run on Lenses CE locally as a lab specimen and nothing more. In parallel, collapse your four rails into one log you own. **Confidence: [emerging].** **Main assumption:** hiring managers in agent-infrastructure roles (Redpanda ADP, Confluent/IBM, AI labs' FDE teams) value demonstrated, critical security reasoning about MCP authorization more than a demo of yet another agent driving Kafka through MCP. Vendors themselves have already published the latter.

## G. Action Plan

### Week 1 (recommended): content + security teardown
- **Day 1:** On the Windows hub, set Docker Desktop to at least 6 GB, then run `ACCEPT_EULA=true docker compose up -d --wait` using the CE compose file. Change the admin/admin password. Bind the ports to localhost only. Record container RAM with `docker stats`, but keep this internal (EULA §4.2(i)).
- **Day 2:** Connect Claude Code (`claude mcp add --transport http Lenses http://localhost:8000/mcp`). Capture the DCR, PKCE and consent flow. Test whether a read-only-consented token can call a write tool. Check whether introspection returns `aud`. Check that revocation blocks the next call.
- **Day 3:** Run the same tests against `@confluentinc/mcp-confluent` with local Kafka (static-key path) and read the Redpanda ADP docs. Build a comparison table: scope granularity, enforcement point, audience binding, token passthrough, human-commit tier, audit and telemetry defaults.
- **Day 4:** Implement in agent-approval-gate: per-tool scope enforcement (tool not registered without scope), an RFC 8707 audience check, a "human-commit" tier, and `traceparent` propagation.
- **Day 5:** Publish the post on blog, X, GitHub and Facebook. Open one issue or PR on lensesio/lenses-mcp that summarizes the audience/introspection findings constructively. Post in the Lenses Slack.
- **Success criteria:** post shipped; at least 1 maintainer response (issue/PR/Slack) within 10 days; agent-approval-gate release tagged; at least 2 FDE/DevRel applications sent citing the post.
- **Kill criterion:** if by Day 2 the OAuth flow can't run in CE (that is, OAuth is enterprise-only, as one docs page suggests), drop Lenses as the specimen and redo the teardown on Confluent's open-source server and the MCP spec alone.

### Separate 1-week infra pilot (only if you insist on testing Kafka)
- Run Kafka 4.x KRaft single-node + Kafbat UI (Apache-2.0) in Docker on the hub, not Lenses. Mirror (don't replace) all four rails into one topic for 7 days, with agents on the Macs and VPS writing through a local outbox file.
- **Success:** a zero gap between pending and applied counts derived from consumer offsets; hub RAM increase under 1.5 GB; no lost events across at least 3 induced hub outages.
- **Kill criterion:** more than 2 hours/week of broker care, or any lost or duplicated-unreconciled event during an outage. Revert to the single-file log.

**Prior art for each proposed idea:**
- "Agent drives Kafka via Lenses MCP": Lenses blog (May 11, 2026).
- "Kafka as agent commit log": Kong (Jun 17, 2026); Waehner (May 26, 2025); AutoMQ series; novatechflow (Feb 2026).
- "Kafka skills for agents": lensesio/agentic-engineering-for-apache-kafka (2026); Confluent Agent Skills GA.
- Independent security teardown of Lenses/Confluent/Redpanda MCP OAuth enforcement: **prior art not found**. I looked at Lenses, Confluent and Redpanda blogs and docs, MCP-auth explainers (Scalar, Authgear, Permit.io, Medium) and the mcpgrade.com review of Confluent MCP; I did not search Reddit, HN or X.
- PR adding audience validation or scope enforcement to lenses-mcp: **prior art not found** (repo shows 0 open issues and 0 open PRs).

## H. Sources

Sources are attributed inline by publisher, title and date throughout; a separate citation pass attaches links. Items marked [unverified] or [no source found] were not confirmed: current Lenses careers openings, Lenses telemetry behaviour, and current openings at Aiven, StreamNative, Conduktor, Materialize, RisingWave and Decodable.

## Sources (links extracted from the claude.ai artifact DOM, 2026-10-09 21:45; chat reports 285 sources consulted)

- https://docs.lenses.io/latest/getting-started/quickstart
- https://lenses.io/legals/eula
- https://github.com/lensesio/lenses-mcp
- https://builtin.com/company/lensesio
- https://github.com/nats-io/nats.docs/issues/321
- https://computingforgeeks.com/deploy-redpanda-docker-compose/
- https://factorhouse.io/resources/kafka/tools/kafka-management-tools-lending/
- https://lenses.io/blog/lenses-mcp-server-oauth-secure-streaming-apps-with-ai-agents
- https://github.com/orgs/lensesio/repositories
- https://github.com/lensesio/stream-reactor/releases/tag/12.1.2
- https://github.com/lensesio/stream-reactor/releases
- https://github.com/lensesio
- https://pitchbook.com/profiles/company/223011-55
- https://lenses.io/careers/support-engineer
- https://startup.jobs/company/redpanda-data
- https://en.wikipedia.org/wiki/Confluent
- https://www.confluent.io/blog/ai-developer-tools-mcp-server-agent-skills-ga/
- https://konghq.com/blog/engineering/agentic-commit-log-kafka-kong
- https://lenses.io/product/pricing
- https://lenses.io/blog/lenses-6-panoptes-announcement
- https://docs.lenses.io/latest/mcps
- https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization
- https://lenses.io/blog/lenses-6-2-release-agentic-engineering-kafka-migrations
- https://github.com/lensesio/agentic-engineering-for-apache-kafka
- https://www.therecursive.com/german-software-company-celonis-acquires-data-ops-startup-lenses-io-to-bring-real-time-data-to-business-operations/
- https://factorhouse.io/articles/lenses/
- https://factorhouse.io/articles/top-kafka-ui-tools-in-2026-a-practical-comparison-for-engineering-teams/
- https://factorhouse.io/articles/best-free-kafka-ui-tools/
- https://docs.conduktor.io/guide/release-notes
- https://www.confluent.io/blog/latest-apache-kafka-release/
- https://github.com/confluentinc/mcp-confluent
- https://docs.confluent.io/cloud/current/ai/ai-tools/open-source-mcp-server.html
- https://mcpservers.org/servers/confluentinc/mcp-confluent
- https://docs.confluent.io/cloud/current/ai/ai-tools/managed-mcp-server.html
- https://www.redpanda.com/press/redpanda-introduces-ai-gateway-to-bring-control-and-governance-to-enterprise-ai
- https://www.automq.com/blog/coordination-ledgers-for-enterprise-ai-agent-fleets
- https://arxiv.org/pdf/2605.21997
- https://jobs.ashbyhq.com/redpanda-data/578668b4-4e0d-4896-9b5b-924bf1b9a035
- https://www.opentoworkremote.com/view/1466912
- https://bebee.com/gb/jobs/forward-deployed-engineer-emea-redpanda-data--ss-gb-1vfwowh
- https://www.builtinsf.com/company/redpanda-data
- https://www.ziprecruiter.com/co/confluent/Jobs?lk=z6NdLyvekRz3uKP283oUvw
- https://www.automq.com/blog/design-patterns-for-multi-agent-coordination-logs-in-event-driven-ai-systems
- https://www.kai-waehner.de/blog/2025/05/26/agentic-ai-with-the-agent2agent-protocol-a2a-and-mcp-using-apache-kafka-as-event-broker/
- https://siliconangle.com/2021/10/05/following-1b-funding-round-celonis-buys-data-streaming-startup-lenses-io/
- https://medium.com/@ayshsandu/the-evolution-of-mcp-auth-every-spec-every-lesson-2024-11-05-2026-07-28-draft-e3f165a12fdb
- https://anomity.ai/blog/oauth-for-mcp-servers-explained/
- https://docs.lenses.io/3.0/dev/lenses-box/
- https://docs.conduktor.io/desktop/conduktor-first-steps/licenses-and-activations/


