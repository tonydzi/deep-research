# DR26-06-23-HUB-02 — raw report from o

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Claude Code Multi Account and Multi Machine Operations

## Executive Summary

Yes, you can keep using the **same working folder** when you switch Claude accounts, and much of your **local toolchain** will continue to work because Claude Code’s configuration model separates **user/project/local machine state** from **account-scoped cloud features**. In Anthropic’s documented model, user-scoped settings, many local MCP definitions, skills, hooks, and project-scoped `.mcp.json` live outside the account itself, while account/org selection is handled by OAuth, usage entitlements, and organization-level controls. That means the repository and a large part of your local environment can remain stable even as you re-authenticate with a different account. citeturn25view1turn25view0turn26view0

The hard boundary is this: **remote connectors, billing/usage, and a significant chunk of “Claude cloud identity” do not follow the folder**. Anthropic’s docs make clear that remote connectors are configured and brokered through the Claude account, that Team/Enterprise connectors are enabled by org owners and then connected by each user individually, and that usage limits are shared across Claude surfaces within an account. Cowork is controlled at the organization level, and account/org switching is an explicit supported workflow in Claude and Claude Code rather than an implicit profile merge. citeturn19view1turn20view0turn9view1turn19view4turn9view2turn9view4

For your specific goal of a **single shared history pool** across many machines and accounts, Anthropic currently supports only a limited set of portability paths. The supported ones are: **within the same account/workspace**, **personal-to-Team/Enterprise migration**, **Remote Control for continuing a running local session from another device**, and **resuming certain cloud sessions locally in VS Code**. Anthropic does **not** support importing exported data into another personal Claude account, and it does **not** document a supported filesystem-level method for merging per-account recent-session registries. citeturn28view0turn19view6turn9view4turn19view5turn30search7

The most important recommendation is straightforward: **do not treat live session state as a bidirectional sync database**. Back up transcripts, but do so as **cold copies or archive snapshots**, not as live files being co-written on multiple machines. Syncthing explicitly creates conflict files on concurrent edits, SQLite’s maintainers repeatedly warn about corruption and locking hazards on networked or non-local filesystems, and Anthropic’s own Claude Code changelog shows recent fixes for truncation on network/cloud-synced folders, corrupted `.claude.json` entries, and cleanup-related history loss. citeturn22search0turn21search0turn21search3turn21search6turn21search9turn11view0

On policy: Anthropic does **not** publish a simple sentence saying “multiple personal accounts for higher limits are forbidden,” but the closest official language cuts against that pattern. Consumer terms forbid sharing account credentials, the Usage Policy forbids coordinated malicious activity across multiple accounts to avoid detection or circumvent guardrails, and Claude Code docs say Pro/Max advertised usage assumes **ordinary, individual usage**. So a household or team rotating 1–3 personal accounts to spread limits is **not an officially endorsed operating model** and carries meaningful enforcement risk, especially if credentials are shared or the activity looks coordinated. The safer legitimate alternatives are Team/Enterprise seats, Premium seats where applicable, usage credits, or API/Console-based Claude Code deployments with workspace spend/rate controls. citeturn14view0turn17view0turn9view0turn9view2turn9view3turn1view3turn6search8

## Key Findings

### Account bound versus machine local matrix

The cleanest way to think about Claude Code is: **repo and local extension state are durable**, **account/org state is replaceable**, and **session portability is only partially supported**.

| Surface or capability | Primary scope | What happens when you switch accounts | Best handling |
|---|---|---|---|
| Repository working folder | Project / filesystem | No account dependency by itself. Same repo can be used after re-login. Claude Code scopes project/user settings separately from account auth. citeturn25view1turn25view0 | Keep repo stable; switch auth, not folder. |
| User settings, skills, hooks, local memory in «внутренний путь лаборатории» | User / machine | Survive account switch unless you intentionally isolate them. User scope affects you across all projects. citeturn25view1 | Sync selectively with git/chezmoi/Syncthing; keep secrets out. |
| Project-shared MCP in `.mcp.json` | Project / VCS | Continues to work across accounts because it is repo-scoped, but some project servers require re-approval or per-user auth. Environment variables can be used for OS/path differences. citeturn26view0 | Prefer this for portable team tooling. |
| User/local MCP in «внутренний путь лаборатории» | User / machine | Remains available on that machine; not inherently tied to account. Local-scope MCP is stored per project path in «внутренний путь лаборатории»; user-scope MCP is cross-project on the machine. citeturn26view0 | Good for machine-local tools and secrets. |
| `.credentials.json` and login identity | Account auth on machine | Must be refreshed or replaced when switching accounts. Claude Code manages creds via `/login` and `/logout`; on Linux/Windows `CLAUDE_CONFIG_DIR` can relocate `.credentials.json`. citeturn25view0 | Use wrapper scripts or separate OS profiles for credential isolation. |
| Remote connectors / web connectors / remote MCP | Claude account / org | These are **account-brokered**. Team/Enterprise owners add org connectors, then each user individually connects them. Remote connectors work across Claude surfaces once connected. citeturn19view1turn20view0turn19view0 | Maintain a declarative checklist per account/org; expect reconnects. |
| Desktop extensions / local MCPB tools | Machine local | Installed on the computer and configured locally; sensitive fields are stored in OS secure storage. This matches your observation that they survive account changes. citeturn20view1 | Keep these as the stable base layer of your toolchain. |
| Cowork availability | Organization capability | Org owners can enable/disable Cowork. Desktop/Cowork integration is account/org dependent, not folder dependent. citeturn19view4turn36view0 | Treat as per-org entitlement; verify after every switch. |
| Usage limits, billing, usage credits | Account / org plan | Shared across Claude surfaces within the account. Not portable by folder. Team/Enterprise and paid plans have distinct credit/limit models. citeturn9view1turn6search5turn9view3turn1view3turn6search8 | Use proper seats/credits/API billing instead of rotation. |
| Local transcript cache under «внутренний путь лаборатории» | Machine local storage | Local transcripts support local resumption and Remote Control follows local data flow. These files do not by themselves create supported cross-account portability. citeturn34view0turn19view5 | Back up as archive data; do not co-edit live. |
| Exported Claude data between personal accounts | Official portability support | Unsupported. Exports cannot be imported into another personal Claude account. citeturn28view0turn19view6 | Do not rely on export/import for personal-account migration. |
| Personal → Team/Enterprise migration | Official portability support | Supported, including chats, projects, files, memory, connectors, and skills; one-way. citeturn9view4 | Use this when consolidating into a real org. |
| Running session across devices | Same account / same running session | Supported via Remote Control. The conversation stays in sync across connected devices, but the session still runs locally on your machine; nothing is migrated to another account. citeturn19view5 | Use this instead of filesystem surgery for live continuity. |

### What diverges on a new account

For a **new Claude account**, the items most likely to require explicit rework are the account’s OAuth identity, plan limits, any org-gated capabilities, and account-brokered remote connectors. Team/Enterprise connector flows are especially explicit: org owners add the connector, and then each member authenticates separately. On Pro/Max, the user adds the custom connector directly in the account. Enterprise can also restrict some connectors to organization accounts only, which means an otherwise valid service connection may fail until you switch back to the matching Claude org account. citeturn19view1turn19view0

By contrast, the things that usually keep working automatically are the parts Anthropic documents as **user-, project-, or machine-scoped**: repo-local `.mcp.json`, project CLAUDE files, user settings, local skills/hooks, and machine-installed desktop extensions. That is why your current setup feels mostly seamless after account changes: a large share of the practical toolchain is not actually stored in the account. citeturn25view1turn26view0turn20view1

## Contradictions and Debates

### Multi account rotation is possible, but not cleanly supported

Anthropic supports **switching between personal and organization accounts**, and Claude Code explicitly supports re-authing with a different Team/Enterprise account via `/login` or `/logout`. That is different from supporting **multiple consumer accounts as a rate-limit sharding strategy**. The product clearly assumes account switching is legitimate in some cases, but the legal/policy language points toward single-user, non-shared, non-coordinated use. Consumer terms prohibit sharing your account credentials. The Usage Policy prohibits coordinated multi-account activity to avoid detection or circumvent guardrails. Claude Code’s legal page says Pro/Max advertised limits assume ordinary, individual use. Put together, that does not amount to a categorical ban on owning more than one account, but it is strong evidence that “rotate multiple personal accounts to spread limits across a team/family” is not the intended model. citeturn9view2turn14view0turn17view0turn9view0

That leads to a practical judgment call. **A single individual switching between legitimately separate personal/work identities** is clearly within the documented product shape. **A group sharing or systematically rotating consumer accounts for more throughput** moves into policy-risk territory. The risk is higher if there is credential sharing, automation, or behavior that resembles deliberate guardrail/limit circumvention. citeturn14view0turn17view0turn9view0

### Cowork controls appear partially inconsistent across official docs

One support article says Cowork on Team/Enterprise is an organization-wide toggle and that granular per-user or per-role controls are not currently available. But release notes later announced Enterprise role-based access controls that can turn Cowork on for specific teams. The most likely explanation is that the support article is lagging or scoped more narrowly than the release note. In practice, you should assume **Team remains largely org-wide**, while **Enterprise may now have finer capability gating**. citeturn19view4turn2search8turn36view0

### Cloud continuity exists, but not as full history portability

Anthropic has shipped several mechanisms that reduce device friction: chat search and memory in Claude, Claude Code on the web, Remote Control for continuing a local session from other devices, and the ability in VS Code to resume certain cloud sessions locally. But none of that amounts to **general cross-account session portability**. In fact, VS Code’s documented cloud-session resume is one-way in the opposite direction: web sessions can be resumed locally, and **changes are not synced back to claude.ai**. That matters because it shows Anthropic is building continuity features, but still **not** treating history as a bidirectionally portable, account-agnostic object. citeturn28view3turn19view5turn30search7turn34view0

## Best Practices

### Use a layered state model instead of a single synced blob

The best current operating model is to split Claude state into four buckets.

**Portable project state** should live in version control: `.mcp.json`, project CLAUDE instructions, project skills, hooks, and any scripts that do not contain secrets. Anthropic’s docs are pushing in exactly this direction: project-scoped MCP in `.mcp.json` is intended for version control, user/project/local settings are explicit scopes, and environment-variable expansion exists precisely so one shared config can adapt to different machines and paths. citeturn26view0turn25view1

**Machine-local accountless state** should include desktop extensions, local stdio MCP servers, local helper scripts, OS automation, and secrets that belong in Keychain or Credential Manager. Anthropic documents desktop extensions as local installs and says sensitive extension configuration is encrypted in OS secure storage. This is the layer you want to keep stable so that account switching is cheap. citeturn20view1

**Account-bound cloud state** should be treated declaratively, not manually rediscovered every time. For each account/org, keep a manifest of: required remote connectors, required org capabilities, expected login org, whether Cowork should be enabled, and which post-login checks must pass. Anthropic’s documentation makes this necessary because remote connectors are account-brokered and often require both org-side enablement and user-side connection. citeturn19view1turn19view0turn19view4

**History and archives** should be handled as backup/catalog data, not as live operational state. Claude Code stores local transcripts under «внутренний путь лаборатории» for resumption; it does not document those files as a supported interchange format between accounts or machines. Keep them, catalog them, back them up—but do not assume live sync equals product-supported portability. citeturn34view0turn33view0

### Prefer declarative MCP and environment indirection

Your different machine paths are exactly the use case for `.mcp.json` plus environment-variable expansion. Anthropic documents `${VAR}` and `${VAR:-default}` in `.mcp.json` for `command`, `args`, `env`, `url`, and `headers`. That means you should stop relying on absolute user-home paths inside committed configurations wherever possible. Instead, define a small machine-specific environment layer such as `CLAUDE_TOOL_ROOT`, `REPO_ROOT_ALIAS`, or service-specific variables, and let the shared `.mcp.json` resolve them. citeturn26view0

This is also where industry-standard dotfile tooling fits. Chezmoi explicitly supports machine-to-machine differences, templates, ignored files, and mapping the same logical content to different target paths on different systems. In other words, use git/chezmoi for **declarative config**, not Syncthing alone. Syncthing is good at file replication; chezmoi is better at **rendering correct per-machine state**. citeturn21search2turn21search5turn21search8

### Isolate credentials more aggressively than content

Anthropic explicitly documents `.credentials.json` storage and says `/login` and `/logout` manage it. On Linux and Windows, `CLAUDE_CONFIG_DIR` relocates that credential file. That is enough to justify a practical profile pattern even though Anthropic does not document a full first-class “profiles” feature: use small launcher scripts per account that set the desired credential/config location and then invoke Claude Code. Where that is not enough, use separate OS user profiles. citeturn25view0

The reason to do this is not only convenience. It reduces accidental account bleed, wrong-org login, connector confusion, and mistaken usage consumption. It also makes rollback trivial: switching back is just a different launcher or profile, not a destructive rewrite of a shared home directory. citeturn25view0turn9view2

### Treat remote connectors as re-connectable entitlements

Do not try to “port” remote connectors by copying hidden auth material between accounts. Anthropic’s own model is explicit: remote connectors are configured through the Claude account, can be org-gated, and may require each user to connect individually. On Team/Enterprise, owners add the connector and members connect it; on Pro/Max, the user adds it inside that account; in some Enterprise cases only the organization’s own Claude accounts may connect. That means the safe low-friction approach is not hidden-file surgery—it is **connector manifests plus scripted verification after login**. citeturn19view1turn19view0

## Failure Modes

### Syncing live transcripts with Syncthing is conflict prone, not truly merge safe

Syncthing’s model is file replication, not semantic event-log merging. Its own docs say that if a file is modified on two devices simultaneously and the content differs, one copy gets renamed as a `sync-conflict` file. That is acceptable for documents; it is bad for session logs that you expect to be a single canonical sequence. A `.jsonl` transcript shared live across devices is especially vulnerable because append-only logs still diverge if two machines append independently. citeturn22search0

Even worse, if Anthropic changes any local implementation from plaintext logs to SQLite-backed indexes or metadata—which many desktop apps do—you inherit the classic database-over-sync problems. SQLite’s official docs warn about corruption pathways, locking failures on network filesystems, and the fact that WAL mode does not work across network filesystems. The safe assumption is: **live app state is not a shared database unless the app says it is**. citeturn21search0turn21search3turn21search6turn21search9

### Claude Code itself has shown sensitivity to odd storage environments

Anthropic’s changelog recently fixed Write/Edit producing 0-byte or truncated files on network drives and cloud-synced folders. It also fixed a startup crash when `.claude.json` contained corrupted null project entries, and fixed long-running idle sessions losing history when another Claude Code process ran the 30-day transcript cleanup. Those are exactly the kinds of bugs that argue against live multi-machine mutation of operational state. The product is improving, but this area is still delicate enough that you should design conservatively. citeturn11view0turn33view0

### Filesystem-level recents merging is inherently brittle

Your current `/sync-sessions` approach that unions per-account “Recents” registries is clever, but it sits entirely outside Anthropic’s documented support surface. The main risk is not that it always fails; the risk is that **it can fail silently after an update**. Typical symptoms would be ghost sessions, duplicate entries, stale titles, broken session URLs, entries that point to sessions whose required remote connectors are unavailable under the current account, or schema changes that invalidate assumptions you baked into the merger. Anthropic documents supported migration between personal and organization accounts, but it does not document or support merging per-account recents lists between unrelated personal accounts. citeturn9view4turn28view0turn19view6

## Strategic Recommendations

The lowest-friction design is to stop trying to make **every account look identical** and instead make **every machine predictable**. That means:

Use **project-scoped and machine-local tooling as your durable base**, because that is the part Anthropic clearly supports through scopes, `.mcp.json`, local extensions, and secure local secrets. Make account switching rehydrate only the genuinely account-bound pieces: login, remote connectors, org capabilities, and usage context. citeturn25view1turn26view0turn20view1turn25view0

Adopt a **two-tier history system**. Tier one is Claude’s native local/current-account history, which you leave largely alone. Tier two is your own archive/catalog built from local transcript snapshots. The archive becomes the cross-machine, cross-account memory layer for your team, not the product’s recents registry. Anthropic’s own support boundary practically forces this separation because exported data cannot be imported into another personal account, while Remote Control and web/cloud features focus on continuity inside an account, not independent account merging. citeturn34view0turn28view0turn19view5turn30search7

If you want a genuinely compliant multi-user operation, migrate the heavy users to **Team/Enterprise or API/Console-backed Claude Code** rather than relying on consumer-account rotation. Team seats include Claude Code and more usage than Pro, Team and seat-based Enterprise can use usage credits, and Anthropic exposes workspace spend/rate controls for Claude Code in the Console. This is the official path for shared operations, monitoring, and governance. citeturn9view2turn9view3turn1view3turn28view1

For device continuity, use **Remote Control** whenever the goal is “continue this same local session from another device.” It is the closest current thing to cloud session sync for local Claude Code, but it keeps execution local and does not create extra filesystem merge pressure. For cloud execution use cases, use **Claude Code on the web** and, when useful, local resume of supported web sessions in VS Code. citeturn19view5turn34view0turn30search7turn29search3

## Action Plan

### New account checklist

When you create or activate a new Claude account on a machine, use this sequence.

First, isolate the identity. Log out of the prior Claude Code session with `/logout` if needed, then re-authenticate with `/login`. If Claude Code is not presenting the right account choice, Anthropic’s Team/Enterprise guidance is to log out, run `claude update`, restart the terminal, and log in again. On Linux/Windows, use an account-specific launcher that sets a separate `CLAUDE_CONFIG_DIR` so the credential file does not collide. citeturn9view2turn25view0

Second, verify account-bound entitlements. Check `/usage` or the account/usage surface so you confirm the active plan and limit context. Remember that usage is shared across Claude web, Desktop, and Claude Code for that account. If you expected Team/Enterprise access, confirm you selected the correct organization during OAuth. citeturn9view1turn9view2

Third, rehydrate remote connectors. Re-add any Pro/Max custom connectors for that account, or, on Team/Enterprise, confirm the owner has enabled the needed connectors and then connect them as the signed-in user. For connectors that use OAuth, use the CLI-native `claude mcp login <name>` flow where applicable, including `--no-browser` over SSH. Keep a manifest of required connectors and their expected auth method. citeturn19view1turn26view0turn11view0

Fourth, verify organization capabilities. Check whether Cowork is enabled for this org and whether any Enterprise policies such as `forceLoginMethod` or `forceLoginOrgUUID` apply. These can intentionally block the wrong login type or wrong organization at startup. citeturn27view0turn27view1turn19view4

Fifth, confirm the stable machine-local layer. Open Claude Code in a known repo and verify that user/project skills, hooks, `.mcp.json`, and local extensions are visible. If project MCP uses machine-specific paths, ensure the required environment variables are set. This step should normally succeed without any account-specific work if you designed the environment correctly. citeturn25view1turn26view0turn20view1

Sixth, do not backfill history by editing live registries unless you accept that it is unsupported. If you keep your custom recents merger, make it **rollback-safe**: produce backups, validate JSON schema before writing, never mutate while the app is running, and be prepared for breakage after app updates. For durable cross-account continuity, prefer your archive/catalog layer and supported account migration paths where available. citeturn28view0turn19view6turn9view4

### Backup and sync design

For transcripts, the safest design is **local live store + synced archive mirror**. Let Claude Code write to its native «внутренний путь лаборатории» locally. Then copy only **quiescent** session directories into a separate archive tree after a session has been idle or closed. Sync that archive with Syncthing, ideally with file versioning enabled. Do not allow the same live transcript to be concurrently active on multiple writer machines. citeturn34view0turn23search0turn22search0

For portable config, use git plus chezmoi templates for machine differences. Commit `.mcp.json`, project CLAUDE files, hooks, and non-secret settings. Template OS/path differences instead of syncing absolute-path JSON verbatim. Keep `.credentials.json`, machine-specific OAuth tokens, and other local secrets out of the shared state. citeturn26view0turn21search2turn21search5turn25view0

For the undocumented desktop recents registry you already mapped, the safest stance is **backup yes, live cross-OS sync no**. If you must preserve it, snapshot it on app shutdown or at intervals into your archive, but regenerate or merge it only offline and per machine/account. Direct 1:1 raw sync across Windows/Mac path differences is a schema-coupled workaround, not a stable platform feature. This conclusion is partly an inference from Anthropic’s lack of documentation for this registry and from the broader risks of syncing live app state. citeturn28view0turn19view6turn22search0turn11view0

## Sources

The highest-value official Anthropic sources for this topic were the Claude Code docs on **settings**, **authentication**, **MCP**, **data usage**, and **Remote Control**; Claude Help Center articles on **connectors**, **account migration**, **usage limits**, **Claude Code plan access**, **data export**, and **Cowork**; Anthropic legal pages for the **Consumer Terms**, **Commercial Terms**, and **Usage Policy**; and Claude release notes covering Cowork, analytics, plugin/admin controls, and other product changes through June 2026. citeturn25view1turn25view0turn26view0turn34view0turn19view5turn19view1turn28view0turn9view4turn14view0turn14view1turn17view0turn36view0

The most useful non-Anthropic sources were Syncthing’s documentation on **conflicts**, **versioning**, and **folder modes**; SQLite’s official documentation on **corruption modes**, **locking**, **network filesystem caveats**, and **WAL limits**; and chezmoi’s documentation on **machine-to-machine differences** and **templating**. Those sources matter because the core engineering problem here is not just “where does Claude store things,” but “what kinds of files are safe to synchronize live across heterogeneous machines.” citeturn22search0turn23search0turn23search4turn21search0turn21search3turn21search6turn21search9turn21search2turn21search5

### Open questions and limitations

Anthropic does not publicly document the schema or support status of the **desktop per-account recent-session registry** you mapped, so any recommendation touching direct filesystem manipulation of that state is necessarily conservative and partly inferential rather than officially blessed. Likewise, I found no official Anthropic documentation announcing a **first-class Claude Code profile system** for multiple consumer accounts on one machine; the current documented primitives are account switching, OAuth credential management, managed login restrictions, and scoped settings. citeturn25view0turn27view0turn9view2

The bottom line is blunt: **same folder, yes; same seamless account identity, no**. If you want frictionless switching, stabilize the machine-local layer and automate the account-bound reattachment steps. If you want a real shared operational model across people and devices, move toward Team/Enterprise or API-backed Claude Code rather than leaning harder on unsupported consumer-account rotation and registry surgery. citeturn25view1turn19view1turn9view2turn1view3