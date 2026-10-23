# DR26-07-08-MACB-01 — raw report from drift

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# Approval Fatigue and Task Drift in Agentic AI Coding

## Bottom line

The two problems you described are real, common, and tightly linked. **Approval fatigue** happens when an agent asks for too many low-value approvals, so the user shifts from supervision to reflexive clicking. Anthropic says Claude Code users approved about **93%** of permission prompts, which is exactly the kind of environment where human review stops being meaningful. The broader warning-fatigue literature in cybersecurity and medicine shows the same pattern: repeated, low-specificity, interruptive prompts create habituation, lower attention, and more overrides. In contrast, the broad “decision fatigue” literature is more mixed, so the most reliable frame here is not “willpower depletion” but **warning overload + habituation + bad signal-to-noise**. citeturn2search1turn2search12turn12search11turn12search13turn23search1turn23search0

**Session drift** is a different failure mode. The agent starts with your goal, but over a long run the prompt gets diluted by tool outputs, intermediate subgoals, irrelevant files, retries, and stale corrections. Anthropic’s own guidance is blunt: as context fills, performance degrades; long conversations accumulate noise; when you have corrected the agent multiple times on the same issue, a clean session often beats continuing. Recent goal-drift research reaches a similar conclusion: modern LM agents are much better than older ones, but drift still appears under contextual pressure, and inherited trajectories from weaker or already-drifting agents can pull strong models off course. citeturn30view2turn31view0turn31view1turn22search7turn21search14

The practical response in 2025–2026 is not “add more confirmations.” It is to **move control from repeated human clicks into structure**: sandboxing, allowlists, auto-reviewers, short plan-first sessions, externalized specs, worktrees, checkpoints, and cheap session maps. That is what the strongest tools are converging on. citeturn2search0turn2search10turn17search6turn26view0turn36view0

## Diagnosis of the two problems

### Approval fatigue

Mechanistically, approval fatigue is usually created by four things at once. First, the system asks for approval too often. Second, most prompts are routine and harmless. Third, the prompts are interruptive and time-sensitive, so the user is penalized for reading carefully. Fourth, the UI implies that “supervision” is happening even after the user has fallen into autopilot. The alert-fatigue literature in healthcare describes the same loop: once alerts are excessive or low-value, users override them at high rates and start missing the important ones too. AHRQ and later CDS-stewardship work both treat this as a **system design failure**, not a user morality failure. citeturn34search0turn34search1turn33search14

The security-warning literature is even more on point for AI coding agents. Repeated exposure produces **habituation**. Users pay less visual attention, compliance drops, and warning effectiveness degrades over time. Importantly, later work found that habituation can generalize from frequent non-security notifications to critical warnings if they look and behave similarly. That maps directly onto agent approval UIs: if safe file edits, risky shell commands, and boundary-crossing actions all consume the same click gesture, the user learns one reflex for all of them. citeturn12search11turn12search13turn12search8

This is why simple “Are you sure?” dialogs usually fail unless they are rare, specific, and reserved for serious irreversible actions. Nielsen Norman’s long-standing guidance is still correct: confirmation dialogs only help when they restate the exact action and consequences clearly, and only when they are not overused. For frequent reversible actions, **undo** is usually better than confirm. citeturn35search1turn35search19turn12search2

One nuance matters: the more general “decision fatigue” literature is not clean enough to carry the full argument by itself. A 2025 systematic review in healthcare found mixed evidence, with statistically significant support in only **45%** of cases reviewed, and a 2025 large-scale field study reported no evidence for the expected pattern in one real triage setting. So if you want a strong model, use **alert/consent fatigue and habituation**, not a vague “the brain runs out of decisions” story. citeturn23search1turn23search5turn23search0turn23search2

### Session drift

Drift in agentic coding is mostly a **context architecture** problem. The agent’s effective objective is not just your opening prompt. It becomes the sum of the task, recent tool outputs, file contents, past corrections, partial implementations, reviews, and the agent’s own intermediate summaries. Anthropic explicitly notes that context is finite, fills quickly, and degrades quality as it fills. Cursor says the same thing more plainly: long conversations cause the agent to lose focus, accumulate noise, and start switching to unrelated work. citeturn14search1turn30view2turn27view0

Research now formalizes this as **goal drift** or **inherited drift**. The 2025 AIES paper on goal drift shows that agents can gradually diverge from their initial objective when exposed to competing pressures, even when the drift is subtle. The 2026 “Inherited Goal Drift” paper shows a stronger point: even good models can inherit off-target behavior when conditioned on weaker or already-biased trajectories. In plain English: once the session history itself goes crooked, the next turn is starting from corrupted rails. citeturn22search7turn21search14

In coding workflows, drift also appears when exploration, planning, implementation, testing, and review happen in one uninterrupted thread. The strongest product guidance in 2025–2026 converges on **phase separation**: explore first, then plan, then code; or plan in one session, implement in another. Claude Code, Cursor, Copilot, and Windsurf all now expose explicit plan modes or plan-first flows for exactly this reason. citeturn30view2turn26view0turn28view2turn36view0

## What the evidence says actually works

### What works

The best-supported intervention is **reducing prompt volume by design**, not by telling users to “be more careful.” In clinical alerts, best practices emphasize suppressing low-value alerts, tailoring by role and workflow, differentiating interruptive versus passive signals, and governing alerts as a portfolio instead of a pile. Security-warning research points in the same direction: differentiation and salience matter, but the deeper fix is to stop making critical warnings behave like routine noise. Anthropic reached the same destination in Claude Code: auto mode uses a classifier to handle many routine approvals instead of asking the user each time, and sandboxing moves autonomy into bounded environments instead of relying on endless prompts. OpenAI Codex similarly separates sandbox policy from approval policy and adds **auto-review**, where a reviewer agent handles boundary requests without weakening the sandbox itself. citeturn34search1turn34search16turn12search8turn2search1turn2search0turn25view3turn17search1

The second high-value intervention is **plan-first execution with an external artifact**. Cursor’s Plan Mode creates a Markdown plan with file paths and code references. Windsurf stores plan files in `~/.windsurf/plans` so they can be mentioned later in a new conversation. Copilot supports explicit Plan mode, and GitHub’s docs recommend researching and planning before opening a PR. Claude Code’s best-practices docs recommend separating spec creation from execution and starting a fresh implementation session once the spec is ready. This is not theory. It is the dominant product pattern because it lowers both drift and rework. citeturn26view0turn36view0turn28view2turn28view0turn31view1

A third intervention with strong practical support is **verifiable success criteria**. Agents stay straighter when they can test against something external: unit tests, linters, builds, screenshots, or explicit acceptance criteria. Anthropic, Cursor, Devin, and Copilot all say some variation of the same thing: give the agent a pass/fail loop, or you become the pass/fail loop. citeturn30view2turn27view1turn37view0turn28view0

A fourth intervention is **cheap recovery and context compression**. The winning tools now expose ways to compress history without rereading it: Claude Code has `/compact`, selective summarization from checkpoints, local session resume, and checkpoints with rewind; Copilot CLI has searchable session history and `/chronicle standup`; Codex has `/compact`, `/resume`, `/fork`, `/side`, and thread controls; Devin exposes a Progress tab plus command history that lets you jump through what happened. This is the correct answer to “I lost the thread and I’m not rereading 200 turns.” citeturn31view0turn18search4turn29search1turn25view0turn25view1turn37view1

A fifth intervention is **isolated branching when work bifurcates**. Cursor, Codex, Copilot app, Devin Desktop, and Windsurf all lean on worktrees or isolated workspaces so parallel tasks do not stomp on each other. This is the cleanest way to split work when a session forks into “finish the feature” and “investigate a side bug” or “try another approach.” citeturn24search1turn24search2turn24search6turn36view2

### What mostly fails

What fails is the old-school recipe: **generic confirmation popups, same-shaped prompts for radically different risks, and one giant immortal session**. The warning literature says generic repeated prompts are ignored. Privacy-consent studies show the same thing in another domain: repeated pop-ups create consent fatigue, push users into default acceptance, and degrade meaningful choice. That carries over almost perfectly to agent approvals. citeturn13search0turn13search1turn13search8

Another failure pattern is trying to fix a bad implementation by repeatedly nudging the same drifting session. Cursor explicitly recommends going back to the plan, reverting, refining the plan, and rerunning instead of endlessly patching a confused agent. Claude Code says something similar: if you have corrected the agent more than twice on the same issue, start fresh. citeturn27view0turn31view1

Finally, **full bypass on the main machine** is the wrong solution to approval fatigue. Both Anthropic and OpenAI support dangerous/no-approval modes, but both frame them as controlled-environment tools, not everyday defaults. The mature pattern is: fewer approvals because the environment is bounded, not fewer approvals because the agent is unbounded. citeturn2search6turn17search0

## What experienced users and current tools do in practice

Power users in 2025–2026 are converging on the same operating model across products: keep persistent guidance short and versioned, generate a written plan for anything nontrivial, run implementation in a bounded environment, and use a fresh reviewer context before merge. The tool labels differ; the workflow does not. citeturn30view1turn27view0turn28view0turn36view1turn25view0

In **Claude Code**, the practical levers are: auto mode for routine approvals; `/permissions` allowlists for safe repetitive actions; hooks for deterministic policy checks; `/sandbox` for OS-level isolation; CLAUDE.md plus auto memory for reusable context; `/compact`, `/rewind`, and `/clear` for drift control; and writer/reviewer parallel sessions or subagent code review for fresh-context validation. Anthropic’s own docs recommend starting from a clean git state, using checkpoints, running multiple sessions for isolated experiments, and using a spec in one session followed by execution in another. citeturn2search10turn1search5turn2search0turn30view1turn31view0turn31view2turn31view3turn18search9

In **Cursor**, the most relevant anti-drift moves are Plan Mode, Rules for always-on project context, Skills/Hooks for dynamic workflows, `@Past Chats` for selective recall instead of transcript dumping, and worktrees for parallel experiments. Cursor’s own guidance is explicit that long conversations lose focus and that you should start a new conversation when the task changes, the agent gets confused, or one logical unit of work is complete. If you want more manual control over write risk, users also disable or reduce automatic apply behavior and operate review-before-apply via Plan Mode. citeturn26view0turn27view0turn24search1turn6search7

In **GitHub Copilot**, the practical pattern is to shift work from interactive micro-approvals to **branch-level review**. Copilot cloud agent can research, plan, iterate on a branch, open a PR, and then continue from batched review comments. The app now exposes explicit session modes—Interactive, Plan, Autopilot—and each session runs in an isolated workspace with its own worktree or cloud sandbox. For context recovery, Copilot CLI’s `/chronicle` turns session history into standup summaries, tips, and searchable recall. Session logs provide an audit trail linking commits back to the agent run. citeturn28view2turn28view0turn28view1turn29search1turn29search7

In **Aider**, the recurring winning pattern is to keep file scope narrow, keep conventions read-only, and lean on git as the main rollback/review mechanism. Aider explicitly advises adding only the files needed for the task because too many files overwhelm the model. It supports read-only convention files, automatic commits for each edit, `/save` to reconstruct the current session’s file state, and `/copy-context` to export the current context to another surface. This is a more manual but often cleaner anti-drift style than giant IDE-native agents. citeturn9search1turn9search2turn9search15turn9search9

In **Windsurf and Devin Desktop**, the emphasis is persistent but lightweight project memory plus better session visibility. Windsurf’s Plan Mode stores an external Markdown plan that can be revived by `@mention` in a fresh context; its Memories & Rules separate ephemeral learned facts from durable project instructions, and its worktrees isolate parallel tasks. Devin adds a Progress tab, unified step log, shell command history, and the ability to jump through what happened. Those are not luxuries; they are direct treatments for “I am not rereading the whole session.” citeturn36view0turn36view1turn36view2turn37view1

## Context recovery and session splitting

The right mental model is that a long coding session needs a **cheap map**. Good maps are not full transcripts. They are structured compression: the active goal, current plan, what changed, what is blocked, and what to do next. Tool vendors are finally exposing that idea directly. Claude uses compaction and checkpoint summarization; Copilot uses `/chronicle`; Codex uses `/compact` and thread controls; Devin uses the Progress tab; Windsurf uses plan files that can be resumed from a fresh conversation. citeturn31view0turn29search1turn25view0turn37view1turn36view0

When work splits, the best practice is **not** “stay in the same chat and tell the agent to remember both threads.” It is to fork or isolate. Codex has `/fork` for a durable branch of a thread and `/side` for a temporary detour; Copilot app runs each session in its own isolated workspace and branch; Cursor and Devin Desktop use worktrees; Anthropic recommends multiple Claude sessions and explicitly calls out worktrees and fresh reviewer contexts. citeturn25view1turn25view2turn28view2turn24search1turn36view2turn31view3

The crucial part is preserving the **portable artifact** from the original thread. Usually that should be a Markdown plan, AGENTS/CLAUDE/rules file update, or a compact summary of decisions, not the raw transcript. Windsurf’s stored plan files are a very clean example: if the implementation went wrong, discard changes, refine the plan file, and implement again in a new conversation. Cursor’s plan files and Copilot’s instructions files play the same role. citeturn36view0turn26view0turn28view0turn28view4

## Techniques ranked by practicality

| Technique | What it treats | Cost and complexity | Evidence base | Confidence |
|---|---|---:|---|---|
| Bound autonomy with **sandbox + allowlist + boundary-only approvals** | Approval fatigue without losing control | Medium | Anthropic recommends auto mode, allowlists, and sandboxing; Codex separates sandbox from approval policy; warning-fatigue research supports reducing repetitive low-signal prompts rather than adding more of them. citeturn2search10turn2search0turn17search1turn17search2turn12search11turn34search1 | High |
| **Auto-review / classifier-reviewed approvals** instead of human clicks for routine escalations | Approval fatigue at sandbox boundary | Medium to high | Claude Code auto mode and Codex Auto-review both replace many manual approvals with narrower automated review while keeping environment limits intact. citeturn2search1turn25view3 | High |
| **Plan-first, code-second**, with a Markdown plan/spec | Goal drift, hidden scope creep, bad first implementations | Low to medium | Claude, Cursor, Copilot, and Windsurf all ship explicit plan-first flows and recommend reviewable plans before implementation. citeturn31view1turn26view0turn28view2turn36view0 | High |
| External **verification targets**: tests, lints, screenshots, acceptance checks | Task drift, false confidence, “looks done” failures | Low to medium | Claude, Cursor, and Devin all emphasize pass/fail checks that the agent can run itself. citeturn30view2turn27view1turn37view0 | High |
| Short, versioned **persistent context files**: `CLAUDE.md`, `AGENTS.md`, rules | Repetition across sessions, restart cost, forgotten constraints | Low | Claude memory docs, Copilot instructions docs, Cursor rules, and Windsurf rules all converge on short repo-level instruction files. citeturn30view1turn28view0turn28view4turn27view0turn36view1 | High |
| **Hard reset thresholds**: new session after task switch, repeated confusion, or noisy context | Session drift | Low | Cursor recommends new conversations when task changes or the agent gets confused; Claude recommends `/clear` and fresh starts after repeated corrections. citeturn27view0turn31view1 | High |
| **Checkpoints / rewind / auto-commits** | Recovery when the agent goes wrong | Low | Claude checkpoints and rewind, Aider auto-commits, and Windsurf plan reruns all reduce the cost of aborting a bad trajectory. citeturn18search3turn18search4turn9search15turn36view0 | High |
| Fresh-context **reviewer agent / adversarial review step** | Blind spots, self-justifying bad code | Medium | Claude explicitly recommends subagent review of the diff in fresh context; Copilot uses PR review loops and session logs; Codex exposes `/review`. citeturn31view2turn28view1turn25view1turn15search10 | Medium-high |
| **Worktrees / isolated branches / forked threads** for branchy work | Bifurcated tasks, conflicting edits, muddled context | Medium | Cursor, Copilot, Codex, Windsurf, and Devin all now support isolated workspaces or worktrees for parallel tasks. citeturn24search1turn24search6turn24search2turn36view2turn28view2 | High |
| Cheap **session maps**: compaction, chronicle/standup, progress tab, session logs | Lost thread in long sessions | Low | Claude `/compact`, Copilot `/chronicle`, Codex `/compact`, and Devin Progress all provide condensed recovery surfaces. citeturn31view0turn29search1turn25view0turn37view1 | High |
| **Semantic drift / goal-adherence detection** via embeddings or goal-drift evals | Hidden drift before output quality visibly collapses | High | Academic work on goal drift and inherited drift is promising, but product adoption is still limited compared with simpler structural methods. citeturn22search7turn21search14 | Medium |
| Generic **“Are you sure?” prompts** for frequent reversible actions | Usually worsens friction more than safety | Low | UX and warning research consistently shows overused confirmations become wallpaper; undo is better for common reversible actions. citeturn35search1turn12search11turn35search19 | High |

## Top recommendations for a solo AI-coding workflow

### Replace click-supervision with bounded autonomy

For a solo user, the single biggest upgrade is to stop supervising the agent one shell command at a time. Instead, move to **bounded autonomy**. On Claude Code, that means some combination of auto mode, allowlisted routine actions, hooks, and sandboxing. On Codex, the equivalent is `workspace-write` or `read-only` plus `on-request` approvals, rules, and optionally auto-review. The key is to make the environment safe enough that most routine actions do not need your attention, while truly boundary-crossing actions still do. This matches both the product direction and the science: the failure was never “too little vigilance,” it was “too many pointless vigilance requests.” citeturn2search1turn2search10turn17search0turn25view3turn12search13

**Confidence: high.**

### Make every nontrivial task spec-first

If the task will touch multiple files, modify behavior, or require judgment, do not let the agent code immediately. Force a spec or plan first, and make it an external Markdown artifact. Then start a **fresh implementation session** against that artifact. This is now an explicit pattern in Claude Code, Cursor, Copilot, and Windsurf, because it sharply reduces invisible drift and makes review cheaper. citeturn31view1turn26view0turn28view0turn36view0

Minimum viable spec:

- goal and non-goals
- files/modules likely involved
- acceptance tests
- explicit out-of-scope
- rollback point

**Confidence: high.**

### Keep persistent context short, versioned, and load-bearing

Create one repo-level instruction file and keep it brutally short. In Claude Code this is `CLAUDE.md`; in Copilot and Codex, `AGENTS.md` is increasingly portable; in Cursor, rules serve the same role. Only put things there that are always worth loading: build/test commands, canonical patterns, “never do X,” architecture constraints, and naming conventions. If a correction must be repeated twice, promote it into the file. If it is long, conditional, or narrow, do not put it in the always-on context; move it to a workflow, skill, or path-scoped rule. citeturn30view1turn28view4turn28view0turn27view0turn36view1

**Confidence: high.**

### Use explicit reset and split rules

Adopt a hard operational rule: if the task changes, the agent gets confused, or you have redirected it twice on the same issue, **start fresh**. If the work bifurcates, **fork or move to a worktree**, do not continue in one transcript. Tools now support this directly: Claude sessions, Cursor worktrees, Copilot isolated workspaces, Codex `/fork` and `/side`, Windsurf/Devin worktrees. citeturn31view1turn24search1turn28view2turn25view1turn36view2

A simple personal rule set works well:

- same feature, same thread  
- new feature, new thread  
- side investigation, side thread/worktree  
- repeated confusion, rewind or restart  
- competing approaches, parallel worktrees and compare diffs

**Confidence: high.**

### Demand cheap session maps, not transcript archaeology

At the end of every long run, force the agent or the tool to produce a compact session map: what was attempted, what changed, what passed, what failed, what remains, and where the authoritative plan lives. Use the native tools when available—Claude `/compact`, Copilot `/chronicle standup`, Codex `/compact` and pinned threads, Devin Progress tab, Windsurf stored plan file. The correct recovery interface is a **state summary**, not a giant chat log. citeturn31view0turn29search1turn25view0turn37view1turn36view0

**Confidence: high.**

## What to avoid

The fastest way to make these systems worse is to use them like a 2023 chatbot with a 2026 autonomy engine bolted on. A few anti-patterns are now clear.

Do not run a **single endless mega-session** just because the tool can technically continue. Long contexts accumulate stale constraints, dead branches, and misleading summaries. All major tools now provide explicit restart, compaction, or thread-fork controls because they need them. citeturn31view0turn27view0turn25view0

Do not equate **more confirmations** with **more safety**. Frequent generic prompts create the illusion of control while degrading actual control. If every action needs the same click, you are training yourself to ignore the distinction between “save a file” and “cross a trust boundary.” citeturn2search12turn12search13turn34search0

Do not use **danger-full-access / bypass permissions** on your real machine as the default fix for annoyance. If you want fewer prompts, use a sandboxed or isolated environment and keep the dangerous mode for disposable contexts. Anthropic and OpenAI both explicitly caution that bypass-style modes are for controlled environments. citeturn2search6turn17search0

Do not keep “fixing” a drifting implementation with ever more follow-up prose. Once the session is polluted, your best move is usually revert → refine spec → rerun in fresh context. Cursor and Claude both say this directly. citeturn27view0turn31view1

Do not rely on auto-generated memory alone for durable project rules. Windsurf says to prefer Rules or `AGENTS.md` for durable knowledge; Claude says auto memory is context, not an enforced policy. Persistent guidance should be inspectable, short, and versioned. citeturn36view1turn30view1

Do not ask the same agent to be the only judge of its own work in the same context that produced it. The current best practice is a **fresh-context review**: another thread, subagent, or PR review pass against a diff and acceptance criteria. citeturn31view2turn28view1turn25view1

## Confidence assessment

Confidence is **high** on the main operational conclusions: repeated interruptive approvals degrade supervision; structure beats prompts; spec-first plus verification plus session splitting is the current best practice across top tools; and cheap session maps are the correct recovery layer. That confidence comes from convergence between product design, official best-practice docs, and mature alert-fatigue research from medicine and security. citeturn2search1turn34search1turn12search11turn30view2turn27view0turn28view0turn36view0

Confidence is **medium** on research-y drift detectors such as semantic-distance monitoring of agent trajectories. The academic signal is real, but the most deployed real-world fixes in 2025–2026 are still much simpler: plans, checkpoints, restarts, worktrees, and fresh reviewers. citeturn22search7turn21search14

If you want one sentence to operationalize all of this: **replace approval spam with bounded autonomy, replace giant chats with plan artifacts, and replace rereading logs with session maps.** citeturn2search0turn26view0turn31view0turn29search1