# DR26-08-25-MACANTON-14-0740 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "Исследование первых команд"
type: deep-research-original
source: chatgpt

dr_id: DR26-08-25-MACANTON-14-0740
conversation_id: 6a847278-5a70-83eb-8d34-df8b82397d4e
url: "https://приватный чат"
research_started_at: 2026-08-18T14:57:18.520308Z
research_stopped_at: 2026-08-18T15:06:26.411614Z
dr_status_in_tool: completed
harvested_by: dr_harvest_chatgpt.py
harvested_at: 2026-08-25T07:40:38
vault_chat_note: ""
---

> Отчёт Deep Research забран из ChatGPT verbatim (widget_state.report_message).
> Заметка диалога в волте обрывается на строке запуска — тела отчёта там нет.

## Запрос

```text
@Deep research Нужно провести глубокое исследование, Дип-ресearch, по поводу того, какие самые проблемы люди обычно задают, когда начинают работать в Cloud Code или в Codex. И даже, тоже у меня есть группа студентов, которые достаточно взрослые, они ни разу не использовали Cloud Code. И мне нужно и Codex не использовали. И даже Grok Code не использовали. В общем, никогда не использовали никакие coding платформы. Нужно дать им какую-то мини-инструкцию, какие самые команды нужно ввести, чтобы было супер удобно и комфортно. То есть, сделать, у нас черт. Нужен первый некий волшебный пинок и первые несколько команд, которые помогут начать что-то делать. Соответственно, мне нужно, чтобы ты провел глубокое исследование, дал мне рекомендации, чтобы я дал своим читателям, студентам, с чего лучше начать. И это начало должно их так впечатлить, что они готовы будут продолжать дальше залипать в этих coding платформах типа Grok, Codex и Cloud Code.
```

## Отчёт
# Deep Research: The Best “First 30 Minutes” With Claude Code, Codex, and Grok Build

## Executive conclusion

For a group of adults who have **never used a coding agent, perhaps barely used a terminal, and may not think of themselves as programmers**, the first lesson should **not** be “learn the terminal,” “install Node,” “understand Git,” or “memorize slash commands.”

The first lesson should produce this emotional sequence:

> **“I described something in normal English → it created real files → I opened them → it actually worked → I asked for a change → it changed → I inspected what it did → I discovered I can undo it.”**

That sequence teaches the real mental model of coding agents far better than a conventional programming introduction.

After reviewing the current official documentation for Claude Code, OpenAI Codex, and xAI's current coding agent, Grok Build, plus recurring reports in official issue trackers, the highest-impact beginner problems are remarkably similar across platforms: launching from the wrong directory, installation/PATH/shell problems, authentication confusion, permission anxiety, vague requests, inability to verify whether the agent is correct, fear of destructive edits, and long sessions polluted by irrelevant context. OpenAI itself notes that many apparent “quality” problems are actually setup problems such as the wrong working directory, missing write access, or missing tools. Anthropic makes verification and context management two of its central best-practice recommendations. citeturn18view0turn20view4turn20view5

There is an important methodological caveat: I did **not** find a vendor-published, normalized dataset saying, for example, “23.7% of first-time Codex users ask about PATH.” So the ranking below should be understood as a **cross-platform recurring-problem hierarchy**, derived from official onboarding/troubleshooting documentation and public issue reports, rather than a statistically precise frequency table. The official OpenAI Codex issue tracker, for example, contains concrete reports around PATH/tool discovery, Windows/WSL/devcontainer behavior, browser authentication in headless environments, and context pressure. citeturn9search3turn1search1turn1search15turn1search28

My strongest recommendation for your students is therefore:

**Do not teach three coding agents at once. Teach one universal workflow first.**

The workflow is:

**Folder → Ask → Plan → Build → Verify → Inspect → Iterate → Recover.**

Once students understand that loop, switching between Claude Code, Codex, and Grok Build becomes mostly a matter of learning different command names.

For a terminal-first introductory class, **Claude Code is currently particularly well suited to the first lesson** because Anthropic explicitly publishes a terminal guide for people who have never used a terminal, includes `/powerup` interactive lessons, has plan mode, granular permissions, automatic checkpoints, `/rewind`, and explicit verification guidance. That is a pedagogical recommendation based on the current feature set, not a claim that Claude is universally the “best” coding model. citeturn19search1turn20view0turn19search17turn20view4

Codex is an equally sensible starting point when your students already use ChatGPT: its current CLI literally greets new users with `/init`, `/status`, `/permissions`, `/model`, and `/review`, and OpenAI's beginner best-practices guide formalizes an excellent four-part prompt structure: **Goal, Context, Constraints, Done when**. citeturn18view1turn18view0

Grok should now be taught as **Grok Build**, not “Grok Code.” xAI launched `grok-code-fast-1` as a coding model on August 28, 2025, while the terminal coding agent **Grok Build** launched in May 2026 and was open-sourced on July 15, 2026. Its `grok` CLI now has plan mode, rewind, context compaction, session management, terminal diagnostics, settings, and permission modes. citeturn15search2turn15search9turn15search16turn16search1

## What the three products actually are

One naming clarification matters before you prepare teaching materials.

When you wrote “Cloud Code,” I believe you mean **Claude Code** from Anthropic. Google also has a product literally called **Cloud Code**, but that is Google's IDE tooling for Google Cloud development, Kubernetes, Cloud Run, and related workflows; it is not the direct analogue of Codex or Claude Code that your description implies. citeturn0search3turn18view3

The three comparable coding-agent environments for your course are therefore:

| Platform | Command students launch | What it does | Persistent project guidance |
|---|---|---|---|
| **Claude Code** | `claude` | Reads a codebase, edits files, executes commands, works from terminal/IDE/desktop/web | `CLAUDE.md`, scaffolded with `/init` citeturn18view3turn20view2 |
| **OpenAI Codex** | `codex` | Inspects, edits, runs project tools, reviews changes, works from CLI/IDE/ChatGPT desktop/cloud | `AGENTS.md`, scaffolded with `/init` citeturn18view1turn18view0 |
| **Grok Build** | `grok` | Full-screen terminal coding agent that reads/edits files, executes shell commands and manages sessions | Supports project configuration and agent rules; `grok inspect` shows what it discovered citeturn18view5turn16search2turn16search6 |

That table is already one useful teaching insight:

**The product is not the model.**

A beginner does not need to understand model names, context-window sizes, reasoning-effort settings, MCP, subagents, skills, hooks, worktrees, or API pricing during the first lesson. All three products deliberately expose layers of those capabilities, but none is necessary for the first successful experience. OpenAI's own Codex best-practice guide recommends starting with the default permissions and adding configuration gradually; Claude's beginner documentation similarly provides a simple terminal-first path before introducing advanced features. citeturn18view0turn19search1

I would give students only three concepts initially:

**Working directory** means: *“This is the project the agent is looking at.”*

**Prompt** means: *“This is my specification, not a magic incantation.”*

**Diff / verification / undo** means: *“I remain the owner of the computer and can inspect whether the agent did the right thing.”*

Almost everything sophisticated comes later.

## Where beginners actually get stuck

The practical order below is my ranking by **cross-platform recurrence plus classroom damage**. A problem near the top is not necessarily more common in absolute global usage; it is more likely to derail a first class.

| Beginner question | What is usually really happening | Best first response |
|---|---|---|
| **“I installed it. Why does `claude` / `codex` / `grok` not work?”** | Shell or PATH issue, old terminal session, wrong Windows shell, or installation mismatch | Close/reopen terminal; verify the correct shell; retry the launch command |
| **“Why doesn't it understand my project?”** | Agent was launched from the wrong folder | Stop, `cd` into the project folder, launch again |
| **“Why does it keep asking me for permission?”** | This is intentional security behavior | Keep defaults during lesson one; explain approvals rather than disabling them |
| **“What am I supposed to type?”** | Beginner is treating the agent like an unfamiliar command-line program instead of a collaborator | Give Goal + Context + Constraints + Done-when |
| **“It made something, but how do I know it works?”** | No verification criterion was provided | Ask it to run a test/build/check or provide observable evidence |
| **“It changed too much. How do I undo this?”** | Student has not yet learned checkpoints/diff/Git | Teach recovery in the first session, not in week three |
| **“It was smart at first and now it seems confused.”** | Conversation context contains too much obsolete material | New task → new chat; otherwise compact the current one |
| **“Why can't it install/run X?”** | Dependency, network, sandbox, package-manager, or toolchain problem | Avoid dependency-heavy projects in the first exercise |
| **“The terminal is behaving strangely.”** | Terminal-specific key handling/TUI behavior, especially in full-screen terminal apps | Use built-in help/terminal diagnostics; do not debug keyboard protocols in lesson one |

Anthropic's Windows setup documentation explicitly distinguishes PowerShell from CMD because using the installer intended for the other shell creates characteristic errors; its troubleshooting documentation also covers path and environment issues. citeturn14search3turn19search7

OpenAI explicitly warns that many Codex “quality” failures are actually environmental: wrong working directory, missing write access, incorrect defaults, or missing tools. Codex also defaults to bounded filesystem access and no network access in its local agent security model, which explains why “the model knows how to do this, so why can't it?” is frequently really a sandbox/environment question. citeturn18view0turn17search11

Permissions deserve special treatment because beginners tend to make one of two opposite mistakes: they either become terrified by every approval prompt or immediately search for the equivalent of “YOLO mode.” Claude's default mode allows reads while prompting before more consequential operations; Anthropic says bypass-permissions mode should be used only in isolated environments such as containers or VMs. Codex tells new users to retain default permissions until they understand the workflow. Grok's default is `Ask`, while its `Always-approve`/`--yolo` mode deliberately skips many approval prompts. citeturn19search17turn19search21turn18view0turn16search0

**I would explicitly forbid these in the first class:**

```text
Claude: dangerous bypass-permission modes
Grok:   --yolo / --always-approve
Codex:  unnecessary full-computer/full-access permission profiles
```

The impressive experience you want is **“the machine can do things and I remain in control,”** not **“the machine can do anything.”** That distinction becomes psychologically important the moment the agent runs its first shell command. The vendors themselves design their default sandbox and approval systems around exactly that separation. citeturn19search17turn17search11turn16search0

The next large beginner failure is prompting. OpenAI recommends explicitly supplying **Goal, Context, Constraints, and Done when**; it also recommends plan mode when a task is difficult or ambiguous. Claude's best-practice guide recommends a closely related workflow: explore first, plan, implement, and provide an objective check that the agent can run. citeturn18view0turn20view4

The verification problem is more important than clever prompting. Anthropic's formulation is particularly useful for teaching: without a test, build result, screenshot, linter, fixture comparison, or other observable signal, the agent can only decide that the work *looks* finished. Giving it a check closes the feedback loop. citeturn20view4

And finally there is context pollution. Claude's documentation explicitly says performance can degrade as context fills, recommends `/clear` between unrelated tasks and `/compact` when retaining a conversation, and gives users checkpoint/rewind tools. Codex provides `/compact`; Grok provides `/context`, `/compact`, `/new`, `/resume`, `/fork`, and `/rewind`. citeturn20view5turn18view2turn16search1

That leads to a very simple beginner rule:

> **One coherent objective = one conversation.**

Do not let students spend an entire day in one giant coding-agent chat.

## The first lesson should optimize for “wow,” not technical realism

I would **not** begin with React, Next.js, npm, Python virtual environments, databases, Docker, API keys, authentication, deployment, MCP servers, or GitHub.

Those are excellent later exercises. They are terrible first exercises.

The reason is not that the agents cannot handle them. The problem is that every additional system introduces another failure domain: package manager, PATH, dependency version, network permission, credentials, ports, build tooling, external service state, and OS differences. OpenAI specifically calls missing tools and environment configuration common sources of apparent Codex quality problems, while its local agent security model can restrict network access. citeturn18view0turn17search11

My recommendation is a **zero-dependency static mini-application**:

> **“My Command Center” — a polished personal dashboard built from `index.html`, `style.css`, and `app.js`.**

It is ideal for the first session because the student immediately sees something concrete while the coding agent demonstrates several capabilities simultaneously:

The agent interprets natural-language requirements, creates multiple files, designs an interface, writes logic, maintains state, fixes bugs, modifies an existing artifact, and explains its work. Because there is no framework or package installation, almost all of the activity remains inside the project directory. This is my pedagogical inference from the environment and sandbox failure modes described above, rather than a vendor-prescribed tutorial. citeturn18view0turn17search11

The application should contain enough behavior to make it obvious that the agent did more than generate a screenshot:

- editable user name;
- current date/time;
- three editable priority tasks;
- a progress indicator;
- light/dark mode;
- one subtle animation;
- `localStorage`, so information survives refresh;
- responsive behavior on a narrow browser window.

Do **not** explain `localStorage` before the agent makes it work. Let the student discover:

> “Wait. I changed the name, refreshed, and it remembered it?”

That is a much stronger introduction to software than ten slides defining variables and functions.

For the first class, I would also preinstall and authenticate the coding agent whenever possible. Installation is necessary knowledge eventually, but installation failures have almost zero demonstration value. Anthropic even publishes a dedicated guide specifically because first-time terminal users require help just opening a shell and starting Claude Code. citeturn19search1

The instructor's preflight should therefore be:

```text
Student can open a terminal.
Student has one coding agent installed.
Student can sign in.
Typing claude / codex / grok successfully opens the agent.
Student has a clean disposable project folder.
No production repositories.
No credentials.
No important personal files in that project folder.
```

For reference, the current native Claude Code installers are:

```bash
# macOS / Linux / WSL
curl -fsSL https://claude.ai/install.sh | bash

# then
claude
```

```powershell
# Windows PowerShell
irm https://claude.ai/install.ps1 | iex

claude
```

Anthropic currently recommends the native installer and also supports Homebrew and WinGet. citeturn18view3

The current Codex quickstart exposes this standalone installer for macOS/Linux:

```bash
curl -fsSL https://chatgpt.com/codex/install.sh | sh

codex
```

The first run asks the user to sign in, and OpenAI's official first suggested prompt is essentially “Tell me about this project.” Codex is also currently available through the ChatGPT desktop app, including native Windows workflows, so for Windows-heavy adult classes I would strongly consider preinstalling the desktop surface rather than turning the first lesson into a Node/npm or shell-installation lesson. citeturn18view1turn13search2turn13search4

Grok Build currently publishes binaries for macOS, Linux, and Windows:

```bash
# macOS / Linux / Git Bash
curl -fsSL https://x.ai/cli/install.sh | bash

grok --version
grok
```

```powershell
# Windows PowerShell
irm https://x.ai/cli/install.ps1 | iex

grok --version
grok
```

These commands are published in xAI's official open-source Grok Build repository. citeturn18view5

## The copy-paste “magic first session”

This is the handout I would actually give your students.

First, create one completely disposable folder:

```bash
mkdir ai-first-project
cd ai-first-project
```

Then launch **only the platform used by the class**:

```bash
claude
```

or:

```bash
codex
```

or:

```bash
grok
```

All three agents fundamentally operate against a working project/directory, and Codex's own quickstart explicitly tells the user to open a project directory before running `codex`. Claude likewise works from the directory in which it is launched, and Grok exposes `--cwd` when you need to specify one explicitly. citeturn18view1turn18view3turn16search2

Before asking for code, give them one tiny orientation command.

For **Claude Code**:

```text
/powerup
```

Anthropic added `/powerup` specifically as short interactive lessons with animated demonstrations of Claude Code features, and the command remains in the current command reference. This is almost tailor-made for your “first magical push” requirement. citeturn19search10turn20view0

For **Codex**:

```text
/status
```

Then type:

```text
/
```

The current Codex opening screen itself suggests `/init`, `/status`, `/permissions`, `/model`, and `/review`, and typing `/` opens the slash-command interface. citeturn18view1turn18view2

For **Grok Build**:

```text
/help
```

If the terminal rendering or keyboard/clipboard behavior seems strange:

```text
/terminal-setup
```

Grok's current TUI command reference includes `/help`, `/settings`, `/terminal-setup`, `/plan`, `/view-plan`, `/context`, `/compact`, `/resume`, and `/rewind`. citeturn16search1turn16search7

Then everybody receives **exactly the same natural-language request**, regardless of platform:

```text
I am a complete beginner. Work only inside this project folder.

GOAL
Build a polished single-page app called "My Command Center" that I can
open locally in a web browser.

CONTEXT
This folder is intentionally empty. This is my first coding-agent project.

CONSTRAINTS
- Use only HTML, CSS, and vanilla JavaScript.
- No npm.
- No frameworks.
- No package installation.
- No external APIs.
- No accounts or API keys.
- No external assets.
- Create files only inside this folder.
- Keep the code easy for a beginner to inspect.

The app should contain:
- a personalized greeting with an editable name;
- current date and time;
- three editable priority tasks;
- a progress indicator;
- light/dark theme;
- one subtle, tasteful animation;
- responsive layout;
- keyboard-accessible controls;
- localStorage so editable data and theme survive a page reload.

Before editing anything, tell me your plan in no more than 5 lines.
Then implement it.

DONE WHEN
- all necessary files exist;
- index.html can be opened directly in a browser;
- all interactions work;
- values survive a page reload;
- you inspect your own changes;
- you tell me exactly what you verified and anything you could not verify.
```

This prompt is deliberately structured around the same information OpenAI recommends—goal, context, constraints, and completion criteria—and adds Anthropic's key principle of giving the agent explicit verification requirements. citeturn18view0turn20view4

Do **not** lecture about that structure yet.

Let them watch the agent work.

Then open `index.html`.

Now the second prompt:

```text
The app works.

Now make it feel substantially more premium without adding any dependency.
Improve visual hierarchy, typography, spacing, empty states, micro-interactions,
and mobile layout.

Do not rewrite things that do not need rewriting.

When finished:
1. tell me which files changed;
2. summarize the visible improvements;
3. verify that the original functionality still works.
```

This teaches the most important behavioral pattern with coding agents:

> **You do not need to get the specification perfect in one giant prompt. You steer the artifact iteratively.**

Then give them the fun one:

```text
You are the product designer now.

Propose ONE feature that would create the biggest "wow" improvement while
keeping this app dependency-free and understandable to a beginner.

Explain the idea in one sentence.
Then implement it.
Then verify it works.
```

At this point the student has experienced **delegation**, not autocomplete.

Now teach critical thinking:

```text
Act as a skeptical senior engineer reviewing the app you just built.

Find 5 concrete bugs, edge cases, accessibility problems, or UX weaknesses.
Do not invent problems just to reach five.

Rank the real findings by importance.
Fix the top 3.
Then verify that the fixes did not break existing behavior.
```

This is important because the student's mental model changes from:

> “AI writes code.”

to:

> “AI can build, criticize, test and iterate—but I define the objective and acceptance criteria.”

OpenAI explicitly recommends asking Codex not merely to make the change but to test, validate, and review it; Claude's best practices similarly emphasize providing checks and evidence rather than accepting an assertion of success. citeturn18view0turn20view4

Finally, deliberately teach recovery. This dramatically reduces fear.

With **Claude Code**, ask for one obviously undesirable change:

```text
For an experiment, make the entire design aggressively neon green and oversized.
```

Look at it.

Then:

```text
/rewind
```

Claude Code creates checkpoints for prompts and can restore conversation state, code state, or both from `/rewind`; Anthropic explicitly notes that these checkpoints are not a replacement for Git because external/Bash changes are not necessarily captured. citeturn20view3turn20view4

With **Grok Build**, use the same experiment followed by:

```text
/rewind
```

Grok's current command palette includes `/rewind` for returning to a previous turn. citeturn16search1

With **Codex**, teach:

```text
/diff
```

OpenAI documents `/diff` as showing the Git diff, including untracked files, so the student can see precisely what Codex changed. Codex's desktop interface additionally exposes a visual diff pane with controls for reviewing and reverting changes, and OpenAI recommends Git checkpoints before and after tasks. citeturn18view2turn18view0turn18view1

That recovery demonstration is, in my view, one of the highest-value moments of the lesson.

It answers the unspoken adult-beginner fear:

> **“What happens when the robot ruins everything?”**

The answer is:

> **“You inspect, reject, revert, and continue.”**

## The only commands I would teach on day one

Trying to memorize 20 commands destroys the magic. Teach roughly five or six per product and explain that typing `/` reveals the rest.

### Claude Code

| Command | Beginner translation |
|---|---|
| `claude` | Start |
| `/powerup` | Show me what this thing can do |
| `/init` | Learn this project and create persistent project instructions |
| `/plan …` | Think before touching code |
| `/permissions` | Show/change what you are allowed to do |
| `/review` | Criticize the changes |
| `/rewind` | Take me back |
| `/compact` | Keep the useful context, throw away conversational clutter |
| `/doctor` | Something about the setup seems broken |

Anthropic currently recommends `/init` early in a repository to generate `CLAUDE.md`, followed by refining that project memory; its command reference explicitly positions `/plan` for larger changes and `/rewind` for recovery. `/doctor` is a bundled setup/checkup skill. citeturn20view2turn20view3turn14search6

I would not require students to remember the entire table. The real day-one set is:

```text
/powerup
/init
/plan
/review
/rewind
```

Add `/permissions` when explaining safety.

### OpenAI Codex

| Command | Beginner translation |
|---|---|
| `codex` | Start |
| `/status` | Where am I and what configuration am I using? |
| `/permissions` | What are you allowed to do? |
| `/init` | Create this project's `AGENTS.md` |
| `/plan` | Think through the task before implementation |
| `/diff` | What exactly changed? |
| `/review` | Try to find problems in the changes |
| `/compact` | Compress a long conversation without starting from zero |
| `/clear` | This is a new problem; give me a clean conversation |

These are all part of the current Codex CLI workflow. `/init` scaffolds `AGENTS.md`; `/diff` exposes the changes; `/compact` summarizes a long chat; `/review` can review uncommitted changes, commits, or branch differences. citeturn18view0turn18view1turn18view2

The day-one set I would put on a slide is:

```text
/status
/init
/plan
/diff
/review
```

And underneath:

> **Type `/` when you forget.**

### Grok Build

| Command | Beginner translation |
|---|---|
| `grok` | Start |
| `/help` | Show commands and shortcuts |
| `/plan …` | Explore and propose a plan before editing |
| `/view-plan` | Show that plan again |
| `/rewind` | Go back |
| `/context` | How much conversational context am I using? |
| `/compact` | Compress an overgrown conversation |
| `/resume` | Continue a saved session |
| `/settings` | Configure the UI/agent |
| `/terminal-setup` | Diagnose terminal/clipboard behavior |

Grok's plan mode prevents ordinary agent edit tools from modifying project files until the user approves the plan, although xAI cautions that the shell remains separately governed by permission controls; `/view-plan` reopens the plan review. citeturn16search4

The day-one set:

```text
/help
/plan
/view-plan
/rewind
/compact
```

And the rescue command:

```text
/terminal-setup
```

xAI documents Grok's default permission mode as **Ask**. I would leave it there for new students. Do not make `/always-approve` or `--yolo` a beginner convenience trick. citeturn16search0

There is also one useful Grok command **outside** the interactive session:

```bash
grok inspect
```

It shows the rules, skills, plugins, hooks, MCP configuration, and related project configuration Grok discovered for the current directory. That becomes useful later when the student asks “Why is Grok behaving differently in this project?” citeturn16search2turn16search6

The larger cross-platform lesson is that **slash commands are not the transferable skill**.

This is:

```text
Understand → Plan → Change → Verify → Review → Recover
```

Claude, Codex, and Grok all expose mechanisms supporting essentially that loop. citeturn20view4turn18view0turn16search1

## The prompt pattern students should actually memorize

I would put only this on the wall:

```text
GOAL
What outcome do I want?

CONTEXT
What should the agent know or inspect?

CONSTRAINTS
What must it do or not do?

DONE WHEN
What evidence proves the task is complete?
```

This is almost directly OpenAI's current Codex recommendation, and it generalizes cleanly to Claude and Grok. citeturn18view0

A weak beginner prompt is:

```text
Make my website better.
```

A strong one is:

```text
GOAL
Improve the homepage so a first-time visitor understands the product
within 5 seconds.

CONTEXT
Inspect index.html, style.css, and the current navigation.
Do not change the product's actual features.

CONSTRAINTS
Keep the existing colors and logo.
No new dependencies.
Do not touch the backend.

DONE WHEN
The hero has one clear headline, one supporting sentence, and one primary CTA.
The mobile layout must still work.
Review the diff and verify there are no obvious regressions.
```

The latter gives the agent both **freedom** and **boundaries**.

That is what beginners initially misunderstand: the goal is not to describe every line of code. The goal is to specify the **outcome, relevant information, boundaries, and test for success**. OpenAI's current guide explicitly says Codex can often solve difficult problems even from imperfect prompts, but providing these four ingredients improves reliability and scoping. citeturn18view0

For fuzzy ideas, one additional prompt is extremely powerful:

```text
I have a rough idea but I don't know enough to specify it properly.

Before writing code, interview me.
Ask at most 5 high-value questions, one batch at a time.
Challenge unclear assumptions.
Then turn my answers into a concrete implementation plan.
Do not implement until I approve the plan.
```

OpenAI explicitly recommends asking Codex to interview the user when the user has an idea but cannot yet express the requirements clearly. citeturn18view0

That technique is especially appropriate for your adult students because it removes the most intimidating assumption:

> “I need to know programming terminology before I can ask the coding agent to build something.”

They do not.

They need to know what problem they are trying to solve.

Then, once the agent has produced something, teach this universal follow-up:

```text
Do not tell me that it works.

Show me the evidence that it works.
Run every verification you reasonably can.
For anything you cannot verify automatically, give me a short manual checklist.
```

Anthropic's best-practice documentation strongly recommends this evidence-oriented approach: tests, build outputs, linters, screenshots, or other observable pass/fail signals are what let an agent iterate instead of simply declaring completion. citeturn20view4

And one more prompt should become habitual:

```text
Before changing anything else, explain the current state to me
as if I am technically curious but new to this codebase.

Tell me:
- what the important files are;
- how the pieces connect;
- how to run it;
- what you think is fragile;
- what you would avoid changing casually.

Do not modify files.
```

That directly converts the coding agent into a personal codebase tutor. OpenAI's own Codex first-task suggestion is to ask it to explain the project, while Anthropic's common-workflow documentation similarly treats understanding unfamiliar code as a core use case. citeturn18view1turn14search4

## How I would structure your course so they keep “sticking” to coding agents

The biggest pedagogical mistake would be spending the first class comparing model benchmarks or explaining architecture.

Your first class should optimize for **agency**.

A practical progression is:

| Stage | Student experience | Hidden concept being learned |
|---|---|---|
| **Create** | “It built my idea.” | Agent can modify a workspace |
| **Personalize** | “I can change it by talking.” | Iterative specification |
| **Critique** | “It can find its own weaknesses.” | Review |
| **Verify** | “It can prove things, not merely claim them.” | Tests and observable evidence |
| **Break & recover** | “I can't permanently ruin everything.” | Diff/checkpoints/version control |
| **Plan** | “I can make it think before acting.” | Separation of exploration and implementation |
| **Persist rules** | “I don't need to repeat myself.” | `CLAUDE.md` / `AGENTS.md` |
| **Resume later** | “This can become a real working environment.” | Session/context management |

The persistence step is where the student transitions from “cool chatbot” to “configured colleague.” Claude's `/init` creates a `CLAUDE.md` project guide; Codex's `/init` scaffolds an `AGENTS.md`, which OpenAI recommends for repository layout, build/test commands, conventions, constraints, and definitions of done. citeturn20view2turn18view0

A fantastic exercise at the end of the first or second lesson is therefore:

```text
Based on everything you learned about this project and how I prefer to work,
create/update the project's agent instructions.

Keep them concise.

Include:
- what this project is;
- the important files;
- how to verify changes;
- no external dependencies unless I explicitly approve them;
- explain major architectural changes before implementing them;
- never claim success without verification.

Do not store temporary conversation details.
Store only rules that should still matter next week.
```

For Codex, that belongs in `AGENTS.md`; for Claude, in `CLAUDE.md`. OpenAI specifically cautions that a short, accurate `AGENTS.md` is preferable to a huge vague file, while Anthropic similarly supports project-level persistent instructions through `CLAUDE.md`. citeturn18view0turn14search9

I would **not** introduce MCP, custom subagents, plugins, hooks, scheduled jobs, worktrees, autonomous modes, or multi-agent orchestration until students can independently answer these six questions:

| Question | Desired student answer |
|---|---|
| What directory is my agent working in? | “This project folder.” |
| What do I want changed? | “I can state a concrete goal.” |
| What should it not change? | “I can state constraints.” |
| How will I know it worked? | “I can define verification.” |
| What changed? | “I can inspect the diff/review.” |
| What if it goes wrong? | “I can rewind/revert/use a checkpoint.” |

Once those six answers are instinctive, advanced autonomy becomes useful rather than dangerous.

For the **first platform**, my course-design preference as of **August 18, 2026** would be:

**Claude Code** when you specifically want a terminal-first “coding agent” experience. The explicit first-time-terminal guide, `/powerup`, plan mode, checkpoints, `/rewind`, and `/doctor` make the learning curve unusually teachable. citeturn19search1turn20view0turn20view3turn14search6

**Codex** when your audience already lives in ChatGPT or you want a gentler bridge through a desktop/IDE experience. The current Codex UX exposes its basic commands directly, and the Goal/Context/Constraints/Done-when framework is particularly good instructional material. citeturn18view1turn18view0

**Grok Build** is an excellent second agent to demonstrate that the concepts transfer across vendors. It is also the newest of the three terminal-agent products in its present form—launched in May 2026 and open-sourced in July—and its current TUI already provides plan, rewind, context, session, settings, and permission controls. citeturn15search9turn15search16turn16search1

The final slide I would show your students is therefore not a giant command reference.

It would be this:

```text
YOU DO NOT NEED TO KNOW HOW TO CODE EVERYTHING.

You need to know how to:

1. Point the agent at the right folder.
2. Describe the outcome.
3. State the constraints.
4. Define what "done" means.
5. Let it work.
6. Inspect the result.
7. Make it verify itself.
8. Undo bad directions.
9. Repeat.
```

And directly underneath:

```text
When stuck, don't ask:
"What command am I supposed to know?"

Ask:
"What do I want to happen next?"
```

That, much more than memorizing `/init` or `/compact`, is the mental transition that turns an absolute beginner from a passive AI user into someone who can actually **build with an agent**.
