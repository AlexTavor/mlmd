# Building software with a coding agent

*Draft, 2026-09-29, revised 2026-09-30 and 2026-10-04. Replaces the Superpowers plan
(`~/PersonalKB/drafts/superpowers-layer-plan.md`). Covers requirements through release. The
document forms follow robotics-lms (`research/robotics-lms-process-2026-09-30.md`). The Vision and
the PRD per MVP follow RUP's Vision and iteration plans. The evidence for the build phases is in
`research/build-cycle-evidence-2026-09-30.md`.*

For developers who know how to build software and have not built much with an agent.

## The shape of the process

mlmd is the whole process. It breaks into four phases:

1. **Requirements:** what the product is and what it does.
2. **Functional design and plan:** how the product divides into logical components, and the plan
   for building them.
3. **Technical design:** the HLD and LLDs of what is built next.
4. **Implementation:** building it, checking it and merging it.

Phases 3 and 4 run in **cycles**, one per logical component (or group of
components) the plan says to build next. (Frank, 2026-10-08.)

The documents of each phase:

| Phase | Documents |
| --- | --- |
| 1 Requirements | `docs/vision.md`, `docs/prd/mvp-N.md`, `docs/behaviors.md`, `features/*.feature`, the medium questions in `docs/open-questions.md` |
| 2 Functional design and plan | `docs/architecture.md`, `docs/stack.md`, `docs/spikes/<name>.md`, `.pdd/plan.json` |
| 3 Technical design (per cycle) | `docs/hld/<cycle>.md`, `docs/lld/W<id>-<name>.md` |
| 4 Implementation (per cycle) | `docs/cycles/<cycle>.md`; at the end of an MVP, its verdict in `docs/prd/mvp-N.md` |

The standing documents are kept current in every phase. Each document's purpose line, and how it
is worked on, is under its phase below.

## Words

- **Operator:** the person running the process with the agent. They answer, decide, use what's
  built and approve the stops. This document calls them \"you\".
- **Phase:** one of the four parts of the process: requirements, functional design and plan,
  technical design, implementation (see The shape of the process).
- **Cycle:** one run of phases 3 and 4 for a logical component, or group of components, that the
  plan builds next. It has a goal and exit criteria. `.pdd/plan.json` calls cycles `batches`, dod's
  word, until dod is renamed.
- **MVP:** a bound on what is built next, with its own PRD and verdict. Humans draw it to keep
  their thinking bounded; a later iteration may go any way (see After an MVP).
- **Work item:** one unit of work in the plan, done on its own branch.
- **Your items:** plan items only you can close: an MVP's verdict, a stop the trust level asks for.
- **Session:** one new technical session of the LLM, with a new context. Nothing else is called a
  session. `/clear` ends one and starts the next in the same conversation (the desktop app's
  sidebar entry), so a conversation can hold several sessions.
- **Fresh session:** a session that did not write the work it is given, started from the documents.
- **The gates:** the checks that run before main moves.

## Using mlmd

mlmd is the Claude Code plugin that runs this process. Parts of it are still to be built (see
Tooling this process needs).

- **Install, once per machine:** `/plugin marketplace add AlexTavor/mlmd`, then
  `/plugin install mlmd@mlmd`. The first session after that installs, in the background, what mlmd
  needs: uv if it's missing, and dod, pinned to a version, into the plugin's data folder. mlmd
  installs nothing that needs admin rights or runs at login. For those it shows you the command.
- **Start, once per project:** in an empty folder, `/mlmd:start`. It sets up the repository, the
  documents, the plan, CLAUDE.md with every trust box checked, the permission rules and the hooks,
  adds the project to dod, and begins the Vision interview. Instead of the commands, you can attach
  `mlmd.md` to a prompt: it installs the plugin and starts the same way. mlmd asks for the folder
  (an empty one or an existing repository; it runs `git init` if needed), then which start applies:
  - **idea or law first:** the Vision interview;
  - **code and specs first:** mlmd asks which code and specs to read, and lets you pick from the
    candidates it finds when it isn't sure which you mean. It reads them, showing progress if that
    takes more than about 20 seconds, lists the documents it will fill and why, writes what the
    sources mean into them, then asks only about gaps and contradictions. It runs you through every
    document from the Vision on, however far the existing project got: each is filled from what
    exists and asks only what is missing. Its topics are the sections of the document, in order; a topic with nothing to ask still gets its summary. Each topic's summary lists what was
    decided today, what changed from the sources, and what was imported unchanged. From then on
    mlmd's documents are the only master copy; the sources are inputs and are not kept in sync;
  - **joining a running project:** mlmd asks your role, records it in the register, shows where the
    project stands and why, and asks you any questions marked for your role. Your GitHub access
    sets what you can push. Teams tell each other about joiners through GitHub.

  Before the first question, one screen shows the four phases, where you are, and the current
  document with its purpose line.
- **Overview:** in the desktop app mlmd keeps a pinned overview page, republished at each commit:
  what needs your attention first, then the four phases with where you are, then the current
  document's topics. In the CLI, `/mlmd:status` shows the same.
- **The loop:** every session opens with its goal and a status line: what's done, what's ready, what
  waits for you, and the plan view's address. Then either:
  - start a new worktree session: the desktop app's worktree option, or `claude --worktree`. mlmd
    gives it the next ready item, the session names the item, and you say go; or
  - in a conversation in the project folder, `/clear` to start a new session, then `/mlmd:next`.

  For parallel work, start another session. Each one takes a different ready item.
- **Other commands:** `/mlmd:status` shows the plan in the chat. `/mlmd:trust` shows and changes the
  trust boxes, editing CLAUDE.md and the permission rules together. `/mlmd:release` tags and
  deploys a release.
- **What you do:** answer interviews, decide what comes to you as a question, use what an item or a
  cycle names for you to try, approve the stops your trust level keeps, and give each MVP its
  verdict.
- **What you don't do:** write status, keep track of the phase, make branches or worktrees, run
  merges, or remember to run reviews.
- **Learning as you go:** the first time a step runs in a project, the session says in one line why
  the step exists.

## Sessions

Keep each session small, and slide from topic to topic within a document. A topic is a section of
a document being filled (in vision.md: problem, users and core; features and MVPs; constraints;
what to keep possible; standing documents), a spike, a work item or a review. A session never
crosses into the next document: when work on a document ends, the next one starts in a new
session, even if the context is still small. Approval, commit and merge happen at that boundary,
so the next document starts from the approved documents alone. The review of a document, in a
fresh session, belongs to that document's work. Within a document, a session moves on to a new
session when its context grows too large for you to keep in mind, before a large new topic, or for
a deep dive. A session ends by writing its output files, committing them and merging them into
main, so the next session starts from main. The next session starts by reading only those files,
not the previous conversation. Too much in one session is lost to every later one, and fills both
your context and the agent's.

mlmd watches the session's context size:
- **100K tokens more than it started with:** mlmd suggests a new session, for efficiency. If you
  agree, it wraps up with you, commits, merges into main, and offers the next session.
- **Before a large new topic:** mlmd suggests a new session even below 100K tokens, so the topic
  doesn't build on a context full of the previous one.
- **500K tokens:** mlmd urges you more strongly to continue in a new session, and offers to help
  split the remaining work into sessions.
- **Suggestions ignored:** mlmd explains what a very large context costs: every prompt resends the
  whole context, so each one costs more and takes longer, and the model keeps track of early
  details less well.

When a session moves on, mlmd offers the next session as a card in the desktop app (in the CLI it
names the command), with that session's first prompt filled in. One click starts it. The new
session starts from committed work only, so mlmd commits before it offers the card.

A deep dive, such as a spike, a prototype, or going deeper into one part, is a detour that gets
its own sessions. mlmd keeps the path as a stack in the plan, for example
`vision.md › constraints › deep dive: retention`, and each status line shows it. When the deep dive ends,
the next card returns you to the step you left, at the question where you stopped. Deep dives can
nest.

A session works in the worktree of the item it is on (see Git), and starts with the plan view open
(see The plan).

## Documents

The agent writes the documents and keeps them current. They are how one session's findings reach
the next.

Documents change in any phase. When work turns up a change to a behavior, a rule, a term or an
assumption, the document changes in the same commit as that work. Most product decisions will
arrive after the requirements phase, while later work is being designed.

### Product documents

Three documents describe the product. Each answers one question.

**`docs/vision.md`: where the whole product is going.** Written in phase 1, and revised when an
MVP's verdict changes the direction. It holds:
- the problem, the users, the core;
- every feature, in a line or a short paragraph, marked with the MVP it belongs to, or with none
  yet;
- the constraints and quality levels that hold across the product: devices, speed, privacy,
  languages;
- the later features the architecture must keep possible, each with what it needs from the
  architecture.

It has no behaviors and no scenarios. A feature gets its detail when an MVP takes it on.

**`docs/behaviors.md`: what the product does.** One living spec of every behavior that is built or
being built. Behaviors are numbered across the whole product (B1, B2), and an id is never reused.
Each behavior has:
- a paragraph stating its contract: inputs, outputs, what stays true;
- a **Failure** line: what can go wrong, and what happens then;
- an **Edges** line: boundary, empty, zero and maximum cases;
- the MVP that introduced it.

A later MVP that changes a behavior edits it here, marked with who changed it and when. Git keeps
the old text.

**`docs/prd/mvp-N.md`: what this MVP is for.** One per MVP, and short:
- what the MVP is for, and what result would be a no;
- the behaviors it adds or changes, by id, one line each;
- its decisions, each marked with who made it and when. Its open questions stay in
  open-questions.md, with this MVP under "needed by";
- after it ships, its verdict. From then on it is a record and isn't edited.

**What the Vision may be used for.** The architecture may cite the Vision to keep a later feature
possible, when that costs little now and would cost a lot later. Each such choice is a decision in
architecture.md, with its cost. Nothing is built for a feature outside the current MVP: no code,
no abstraction, no scenario. Storing all data under an organization key in a product with one
customer, because a later feature serves several, is the first kind. A screen for choosing an
organization is the second.

### Standing documents

Phase 1 creates all of these, including the ones that start empty.

**`CLAUDE.md`** (project root). Claude Code loads this file into every session automatically. No
other file is loaded unless something asks for it. It holds:
- what the project is, in a few lines;
- the reading order for the other documents: vision.md, behaviors.md and the current MVP's PRD;
  open-questions.md before proposing anything; assumptions.md before relying on anything the
  documents state as fact; architecture.md before changing structure. Earlier MVPs' PRDs are
  records: read them for why something was decided, not for what the product does;
- the instruction to update the documents whenever the session finds a decision, rule, term,
  trap, assumption or issue;
- **Claims:** nothing is stated as fact unless it was read or run in the current session. A name,
  a file name or a search hit is a lead, not a fact. The agent reports what it guessed as
  confidently as what it checked, and this rule is what makes it check;
- the **Trust** section (see Trust);
- three imports, each on its own line: `@docs/rules.md`, `@docs/glossary.md`, `@docs/footguns.md`.

An import pulls its file into every session, so rules and terms apply without anyone remembering
to read them. A rule in a file that no session loads is not followed. Imports cost context in
every session, so if one of these files grows long, split it by area.

**`docs/rules.md`**. The standing rules every session follows, whatever its task. Numbered (R1,
R2). Each rule has:
- **Says:** the rule.
- **From:** why it exists: a decision, or what went wrong without it.
- **Tier**, one of three:
  - **Gated:** a script fails when the rule is broken. It names the script.
  - **Reviewed:** a person checks it. It says so, and names when the check happens.
  - **Registered:** a file must exist and be current. It names the file. The check is that the
    file exists, not the judgement inside it.

Phase 1 rules come from your constraints ("no data leaves the device", "no new dependency without
a decision"). Phase 2 adds the architecture's (module boundaries, such as "the simulation imports
nothing from the view"). The product's own rules, such as a game's rules or business rules, don't
go here. They are behaviors in behaviors.md.

**`docs/glossary.md`**. Every term with a specific meaning in this project: one meaning each, and
the words not to use for it ("member; not user, customer or account"). A term means exactly this
in every document, identifier, test name, commit message and conversation. The agent drifts
between synonyms, and readers take two words for two things. Terms go in during the phase 1
interview as they come up. Phase 2 adds the names of the architecture's parts.

When a term's meaning changes, or it is replaced, it moves to a **Retired** section. Retired terms
are barred from new prose, identifiers and tests.

**`docs/footguns.md`**. Traps: something that looks like it does one thing and does another. They
can be in a library, an API, the platform, the domain, the test tools, and later the project's own
code. Example: a model whose documentation recommends a runtime that is CPU-only on Apple Silicon,
and 25 times slower there. One table:

| # | Looks like | Actually | Anchor | Verified |
| --- | --- | --- | --- | --- |

The Anchor is a link or a `file:line`. Verified is the date it was last checked. An entry is
removed when the trap is gone, not when it is understood. In phase 1 footguns come from research
and the cheapest prototype, so on a new project the file often starts empty. Spikes are the main
source.

**`docs/assumptions.md`**. Beliefs the design stands on that nobody proved. One table:

| # | Assumption | Falsified if | Detector / verify by | Fragility | Fallback |
| --- | --- | --- | --- | --- | --- |

Fragility is **high** when the design bends if the assumption is wrong, and **low** when a setting
changes. A high-fragility assumption gets a spike, or a decision that makes it not matter, before
anything is built on it (see spikes, in phase 2). A check that is waived is written into its entry, with what
stands in for it. Otherwise the entry reads as work still to do. Phase 1 adds beliefs about users,
the environment and the data. Phase 2 adds beliefs about scale, speed and how dependencies behave.

**`docs/open-questions.md`**. Everything not decided. The product documents and architecture.md
hold decisions only. Each question has:
- its **level** (see Ambiguity levels);
- its **state**. **Proposed:** a default exists, and work proceeds on it until you rule.
  **Deferred:** a decision not to decide yet, with the trigger that reopens it;
- **needed by:** the MVP or the work item that can't go further without an answer.

A settled question moves into the document it belongs in, and is deleted here.

**`UNHANDLED_ISSUES.md`** (project root). Anything found that needs fixing and is outside the
current task. The commit that fixes an issue deletes its entry. Create it together with the line
`UNHANDLED_ISSUES.md merge=union` in `.gitattributes`, so entries appended on different branches
merge without conflicts.

How an issue differs from a footgun: an issue gets fixed and then it's gone. A footgun stays,
because its cause can't be removed (it's in a dependency or the domain) or isn't worth removing.

**`.pdd/plan.json`** and **`.pdd/constitution.md`**: the plan and its rules (see The plan).

## The plan

**`.pdd/plan.json`** is the project's one plan, from the first session to the last. It uses PDD's
vocabulary, so dod can draw it:

```json
{
  "title": "…",
  "batches": [
    {"id": "1-2", "name": "…", "goal": "…", "exit_criteria": "…"}
  ],
  "tracks": ["DATA", "UI"],
  "items": [
    {"id": "W7", "title": "…", "batch": "1-2", "track": "DATA", "depends_on": ["W5"],
     "size": "M", "risk": "low", "delivers": ["B3", "B4"], "note": "…"}
  ]
}
```

- **Cycles** (`batches` in the file) are numbered by their MVP: `1-2` is MVP 1's second cycle.
  The first entry of each MVP is not a cycle: it holds that MVP's phase 1 and 2 documents (from
  the PRD to the plan, and the Vision for MVP 1), so progress shows from the first session.
- **Items** are numbered W1, W2 across the project. An id is never reused: a cut item's number
  stays unused.
- **`depends_on`** means "cannot correctly start until", not the order the plan happens to list
  things in. Leave out a dependency that another one already implies.
- **Sizes** are S, M, L or XL, never times. The agent's time estimates have no basis, and sizes are
  enough to compare items.
- **`delivers`** names the behaviors an item delivers. Every behavior of the current MVP is
  delivered by some item.
- **Your items** carry `"operator": true`: an MVP's verdict, and the stops the trust level asks for.
- **One final item.** Exactly one item has nothing depending on it: the current MVP's verdict. The
  next MVP's first items depend on it.
- **Plan edits reach main in merges of their own.** Work branches never edit plan.json. A plan
  changed on a branch shows only on that branch: robotics-lms and unshatter both ended up keeping a
  second copy of the plan in step by hand.

**Status comes from git.** Nobody writes it, except for one case:
- **done:** a merge into main names the item in a `Done:` trailer, the last paragraph of its
  commit message: `Done: W7`, or `Done: W7, W8` for a merge that finishes more than one. Only
  merges on main's first-parent line count, the ones that moved main;
- **in progress:** a branch named for the item exists;
- **waiting for you:** one of your items whose dependencies are all done;
- **blocked:** the one status written by hand, in the item's `status`, with a note saying what it
  waits for;
- **pending:** everything else.

A merge that leaves the trailer out leaves its items undone, and no check can catch it, as none
knows which items a merge finishes. So the merge script writes the trailer (see Git), and the
status after a merge shows the item done.

Hand-kept status went stale in tiny twice. robotics-lms moved status at two events only, the LLD
written and the merge, and still needed a copy ritual between main and its worktrees. Both of its
events are git events. In plague, four merged items still read pending for up to three hours on
2026-10-04, until a later merge set them by hand (plague `e4c5c26`). plague has read status from
`Done:` trailers since (`tools/plan.ts`, merged in `e2da089`).

**`.pdd/constitution.md`** holds these rules. dod also uses the file to recognize the project.

**A structure check runs with the gates:**
- no cycles;
- no dependency on an item that doesn't exist;
- exactly one final item;
- every behavior of the current MVP delivered by an item;
- an LLD for every build item that has started;
- every item a `Done:` trailer names is in the plan.

**The plan view.** dod draws the plan as a dependency graph. It lists the items ready to start, the
critical path and how many items can run at once. It reads the plan and the status from main in
git, so it is right after every merge without anyone updating it.
- **Setup:** `/mlmd:start` adds the repository to dod. mlmd installs dod itself (see Using
  mlmd).
- **Every session:** a SessionStart hook makes sure the plan view is running and shows you a status
  line before you type anything: done, ready, waiting for you, and the view's address. In the
  Claude desktop app, the session opens the view in the browser pane. A session that finishes
  something (a merge, a stop reached) names the items that changed.
- **dod's always-on background agent** (macOS only) isn't needed. The hook starts the plan view
  when a session starts, and the view keeps running after the session ends. This hasn't been tried
  outside macOS.

## Trust

The project's CLAUDE.md has a **Trust** section with five boxes. A checked box is a stop that waits
for you:

- [ ] **LLD:** you accept an item's LLD before its code is written.
- [ ] **Cycle end:** you use a finished cycle before the next one starts.
- [ ] **Merge:** an item merges into main on your word.
- [ ] **Push:** main is pushed to origin on your word.
- [ ] **Release:** a release is tagged and deployed on your word.

The more boxes are checked, the less the agent is trusted. A new project starts with every box
checked. Uncheck a box when its stops have stopped finding anything. At the far end, unshatter has
one stop left: your word that a piece of work is good, after which the agent merges, pushes and
deploys it.

How each box is enforced:
- **Merge, push and release:** a PreToolUse hook in the project's `.claude/settings.json` reads
  every command before it runs, and knows which boxes are checked. For a checked box, it makes
  Claude Code ask you before any command that merges, pushes or releases, however the command is
  written. When it can't read a command, it asks. For an unchecked box, it lets those commands
  through. Ask rules for the same commands (`Bash(git push:*)` and the scripts) stay as a second
  layer. Rules alone aren't enough: a rule matches only commands that start with its text, and in
  the spike a plain `git push` and `git push -u origin` ran with no question
  (`docs/spikes/trust-enforcement.md`). Bypass mode doesn't skip these questions.
- **What the hook can't see:** it reads the command's text, so a push run from inside another
  script gets past it. The boxes stop the agent's mistakes, not an agent that hides what it
  runs.
- **Cycle end:** one of your items at the end of each cycle, which the next cycle's first items
  depend on. The plan view shows it as waiting for you.
- **LLD:** the session stops after the cycle's design review and waits for your word, which each
  LLD records. Nothing mechanical enforces this box.

**A stop whatever the boxes say:** any irreversible operation on real data, such as a migration
against production data, deleting stored data, or changing who can access what. The script that
does it asks for typed confirmation, not a keypress: one cutover script read a closed input as a
yes. The boxes trade review for speed on work that can be undone. Trusting the agent more doesn't
make an irreversible mistake cheaper.

## Git

- **One repository per project** (see Setup).
- **Branches and worktrees from phase 1 on.** All work happens on a branch, in a worktree of its
  own, with one session per worktree. No session checks out main. The folder the repository was
  created in doesn't stay on main either: after the first commit, detach it
  (`git switch --detach main`). While any checkout holds main, `git push . HEAD:main` is refused.
  Items that don't depend on each other run in parallel sessions, and the plan view shows which are
  ready.
- **mlmd's WorktreeCreate hook makes every worktree.** Claude Code calls it whenever a session
  starts in a new worktree (`claude --worktree`, a background session, and, still to be checked,
  the desktop app's worktree option), and it replaces Claude Code's own creation. It:
  - claims the next ready item. Creating the item's branch is the claim, since git refuses a
    second branch with the same name;
  - bases the branch on local main, not on origin. When the push box is checked, local main runs
    ahead of origin between pushes, and a branch cut from origin would miss the latest merges.
    The hook's input carries no base branch, whatever the hooks documentation lists, so the hook
    chooses main itself;
  - installs dependencies and copies in the git-ignored files the project needs to run
    (environment files). Without them the gates fail, and the failure reads as a problem in the
    code;
  - places the worktree beside the repository, not inside it. Tools that walk the repository pick
    up a worktree nested in it: robotics-lms had to exclude the desktop app's `.claude/worktrees/`
    from its lint and its mutation runs. If the desktop app's features need their worktrees under
    `.claude/worktrees/`, they go there instead, and architecture.md writes each tool's exclusion.
- **Moving to the next item.** In a session that already worked on an item, `/mlmd:next` leaves
  that worktree (ExitWorktree, keeping it), then enters the next item's worktree by its name,
  which goes through the WorktreeCreate hook. The hook creates the worktree, or prints the path of
  the one that already exists. This works after `/clear` and asks nothing. Entering by path
  doesn't work: a move from one worktree beside the repository to another is refused, and on
  Claude Code 2.1.284 entering by path asks you every time (`docs/spikes/worktree-switching.md`).
- **Names:** the branch is the item's id and a short name (`w7-catalog`), and the merge commit
  names the item in its `Done:` trailer. The plan view reads both: the branch for in progress, the
  trailer for done.
- **Merging.** The merge script does these steps:
  1. In the item's worktree, build the merge on a detached HEAD at main:
     `git switch --detach main && git -c rerere.enabled=true merge --no-ff <branch>`, with a
     message ending in the `Done:` trailer, the item's id taken from the branch's name. Resolve
     any conflicts there. rerere records each resolution, so a retry replays it.
  2. Run the gates on the merged tree.
  3. Move main with `git push . HEAD:main`. Git allows only a fast-forward, so if main moved in the
     meantime, go back to step 1 on the new main.
  4. Switch back to the branch. Remove the worktree once the item is done.
- **The gates** run in one pre-push hook, whenever the push targets `refs/heads/main`: types, lint,
  tests, the Gherkin scenarios, the plan's structure check, and the project's gated rules.
  `git push . HEAD:main` runs the pre-push hook too, so one hook covers local merges and pushes to
  origin. That includes a merge committed by hand after a conflict, which a merge hook once
  skipped 5 times in 8. A Claude Code PreToolUse hook refuses `--no-verify`.
- **Releases are tags on main.** Main is not the deployed tree: a deploy comes from a tag. That is
  what lets merges go ahead without you while releases still wait (see Trust).

## Tools

Each step names the one thing to run, and invokes it by name. Nothing depends on a skill starting
from its description: in one project's records, 1,472 reminders to apply a practice led the agent
to open that practice 10 times. There are three kinds:
- **Guidance, in the working session:** skills and templates. The interview, the LLD template, the
  merge procedure, handoff.
- **Checks, in a fresh session:** workflows. design-review for the design documents, a cycle's
  together, and the adversarial review before merge. Whatever judges the agent's work runs outside the session that
  made it.
- **Enforcement and setup:** hooks and scripts. The gates in the pre-push hook, the refusal of
  `--no-verify`, the WorktreeCreate hook, the status line and plan view at session start, the
  first-session install, and the trust hook with its permission rules.

design-review's kinds: the Vision, and each MVP's PRD with its behaviors, use `prd` (`gdd` for a
game). architecture.md uses `hld`. A cycle's designs, reviewed together, use `hld` when the cycle has
an HLD and `lld` when it doesn't. An item's review of its own uses `lld`.

They ship as one Claude Code plugin, mlmd: skills, workflows (in the plugin's `workflows/` folder,
run as `/mlmd:<workflow>`), hooks and templates. engineering-discipline's skills are not part of it:
their practices are already in the documents, the review questions and the gates.

## Ambiguity levels

Every open question has one of three levels:

- **Large:** answering it could change the architecture.
- **Medium:** changes behavior, but not the architecture.
- **Small:** an implementation detail. It doesn't go in the documents.

## Setup

Setting up is not a phase. Before anything is written:
- a folder and a git repository for this project and nothing else. Every document and all the
  code live there, and the project's sessions start there. Claude Code keeps CLAUDE.md, its memory
  and its session history per folder, so a project that shares a folder with other work shares all
  three;
- `.pdd/plan.json`, with the documents of phases 1 and 2 as its first items, and
  `.pdd/constitution.md`. The repository is added to dod;
- CLAUDE.md, with every trust box checked, and the trust hook and permission rules that go with
  them;
- the pre-push hook, starting with the plan's structure check. The plan adds the rest of the gates
  (see `.pdd/plan.json` under phase 2).

`/mlmd:start` does all of this.

## How each document is worked on

The next step is always to work on the next document of the current phase. A phase can have
several documents in progress at once, and a document is worked on and iterated, not finished
once. Work that produces no document of its own (code, an attack, mutation testing, a reading
pass, a release) is defined by a document and records its result in one.

Each document below has:
- **For:** its purpose line. It is canonical: mlmd introduces the document with this line, word
  for word, together with the phase it belongs to.
- **Needs:** the documents it is written from.
- **Ready when:** what makes it good enough for the documents that need it. Standing documents
  are never ready; they are current or not.

Work on each document starts in a new session, and a document can take several (see Sessions).
The fresh-session review of a document belongs to that document's work.

## Phase 1: Requirements

The whole product first (the Vision), then one MVP (its PRD, behaviors and scenarios), then the
questions that change behavior but not structure.

### `docs/vision.md`

- **For:** Decide the whole product: what it is and which MVP each feature belongs to.
- **Needs:** you; existing code and specs, if the project starts from them.
- **Ask:** "Interview me about the whole product until you can write vision.md. Ask one question
  at a time with a progress count, or numbered questions a few per round if I ask for that. At the
  end of each topic, summarize what was written to which document and section, and who decided.
  Mark every decision you write with who made it and when: me, or you so work could continue. Add
  each term to the glossary as it comes up. Anything not decided goes into open-questions.md with
  its level, never into the documents as settled."
- **The interview:** answer, or by number in rounds. You can stop after any answer: the next
  session's status line shows where the interview stands and continues there. Say "explain" when
  a question is unclear. When you and the agent use a word differently, settle it in the glossary
  before going on.
- **Cheapest prototype (optional):** some large questions can only be answered by trying something:
  whether it's fun, whether an interaction feels right, whether the output is useful. For each such
  question, build the cheapest thing that answers it, in whatever tool is fastest. Before building
  it, write down what result would be a no. Each prototype has its own document,
  `docs/prototypes/<name>.md`, which stands alone: the question, what would be a no, what was
  built, how it was judged, the verdict, and where what was built can still be found (a commit or
  a link). A decision that follows from a prototype cites it. The code is thrown away. Say so when
  asking for it; otherwise the agent will reuse it as a base.
- **Review:** design-review of the Vision, in a fresh session, looking for:
  - a feature marked with no MVP, and not marked "none yet" either;
  - a term used with two meanings, or two terms for one thing;
  - a belief the product depends on that nobody chose. Each one goes into assumptions.md;
  - a large question about a later feature that is neither answered nor listed among what the
    architecture must keep possible.

  The review reads the documents under review; it reads the sources (an imported PRD, for example)
  only when a check needs them. Its findings are settled like gap questions, one at a time, and
  recorded with their status in `docs/reviews/<document>.md`.
- **You:** answer, decide, reject. Read every decision marked as the agent's: each topic summary
  ends with a block "mlmd decided, please confirm", one numbered line each, and you reply "ok" or,
  for example, "2: no".
- **Ready when:**
  - every feature is marked with its MVP, or with none yet;
  - every large question about a later feature is answered, or listed in the Vision among what the
    architecture must keep possible;
  - every decision is marked with who made it;
  - the review's findings are settled;
  - the standing documents exist in the form described under Standing documents, and you have seen
    each one's summary. The standing-documents topic is never skipped, also not on an import.

### `docs/prd/mvp-N.md`

- **For:** Decide one MVP: what it's for and what result would be a no.
- **Needs:** vision.md.
- **Ask:** "Take these features from the Vision for MVP N. Write the PRD for this MVP, then their
  behaviors into behaviors.md. Interview me about anything not settled."
- **Keep it small:** the sessions, not the MVP. The MVP is a full product for what it is for. Push
  back on features that don't serve that purpose; everything else stays in the Vision for a later
  MVP. Overview comes from splitting the work into cycles and small sessions, not into smaller
  MVPs.
- **Scope first:** a gap that decides which behaviors get written at all (what the MVP is for, what
  it leaves out) is asked before writing. Other gaps are asked after.
- A cheapest prototype (see vision.md) can answer a large question here too.
- **Ready when:** what it's for and what would be a no are stated; the behaviors it adds or
  changes are listed by id; every decision is marked with who made it; no large question about
  this MVP is open, and any new large question about a later feature is answered or added to the
  Vision's list of what the architecture must keep possible.

### `docs/behaviors.md`

- **For:** State what the product does: contract, failure, edges.
- **Needs:** the PRD.
- **Review:** design-review of the PRD and the behaviors it adds or changes, together, in a fresh
  session, looking for:
  - a behavior without its Failure or Edges line;
  - a term used with two meanings, or two terms for one thing;
  - a belief the design depends on that nobody chose. Each one goes into assumptions.md;
  - a behavior or scenario for a feature outside this MVP.
- **You:** answer, decide, reject. Read every decision marked as the agent's, and every rule.
- **Ready when:** every behavior this MVP adds or changes has its contract, Failure and Edges;
  every term the documents use with a specific meaning is in the glossary; the review's findings
  are settled.

### `features/*.feature`

- **For:** Make each behavior testable.
- **Needs:** behaviors.md.
- **Ready when:** every behavior this MVP adds or changes has scenarios in Gherkin, each tagged
  with its behavior's id (`@B7`), covering the Failure and Edges lines as well as the normal case.

**Why Gherkin:** it doesn't depend on the stack, runners exist for most languages (Cucumber for
JS/Java, behave for Python, godog for Go, Reqnroll for .NET), and developers already know it. The
scenarios can run once the step definitions bind them to the system (see stack.md).

### `docs/open-questions.md`, the medium questions

- **For:** Settle the questions that change behavior but not structure.
- **Needs:** the documents above.
- Answer the medium questions. Each one has a default that work proceeds on until you rule.
  Because they don't affect the architecture, this can run before phase 2, alongside it, or after
  it.
- **Ready when:** no medium question blocks an item the plan is about to start.

## Phase 2: Functional design and plan

How the product divides into logical components, what it is built with, which beliefs are proved
before anything rests on them, and the plan.

### `docs/architecture.md`

- **For:** Divide the product into logical components and walk every behavior through them.
- **Needs:** behaviors.md, and the Vision's list of what to keep possible.

The agent is lead architect: it proposes an architecture that covers every behavior in
behaviors.md, and keeps possible the later features the Vision lists for it.

- **You:** walk the behaviors through the architecture one at a time: "B7: which parts take part,
  in what order, what state changes?" If a behavior can't be walked through, the architecture is
  incomplete. The walk-throughs of behaviors that cross more than one part go into
  architecture.md as its key flows.
- **Later features:** each one the Vision lists for the architecture gets a decision: what keeps it
  possible and what that costs now, or a decision not to, with what adding it later would cost.
- **Assumptions:** every belief the architecture depends on that nobody proved goes into
  assumptions.md, with its fragility.
- **Review:** design-review of architecture.md, kind `hld`, in a fresh session.
- **Ready when:** every behavior is walked through; the review is settled; it holds the parts, the
  key flows, the decisions (each marked with who made it and when), and a **Deferred** section for
  technical options set aside. With it: rules for the module boundaries it depends on, each with
  its tier, and glossary entries for its parts.

### `docs/stack.md`

- **For:** Pick what supports the requirements and the architecture.
- **Needs:** architecture.md. Any constraint known up front (platform, language, hosting) is in the
  Vision already.
- **Step definitions:** choose the test runner and write the step definitions that bind the Gherkin
  steps to the system. Watch for steps that assert nothing: if a scenario's steps only log or
  return, it passes against any implementation. A fresh session checks them by asking "which wrong
  implementation passes these?"
- **Ready when:** every choice is a decision with who made it; every belief the stack depends on
  is in assumptions.md; the project's install command is written, which the WorktreeCreate hook
  runs in every new worktree.

### `docs/spikes/<name>.md`

- **For:** Prove a risky assumption before anything is built on it.
- **Needs:** a high-fragility entry in assumptions.md.

Every high-fragility assumption gets a spike, or a decision that makes it not matter, before
anything is built on it. It could be a mechanism, a part of the stack, an integration. Each spike
does the minimum needed to answer its question.

- **Before starting:** state the question, what result would be a no, and where the spike has to
  run: the device, the data or the real service the question is about. A result from anywhere else
  answers a different question. If that place isn't at hand, say so now and name what will stand
  in. A spike planned inside the work it gates tends to be skipped once that work is built.
- **Ready when:** it holds the question, where it ran, what was done, and the verdict, and the
  assumption's entry has the verdict. The spike code is thrown away unless the verdict says to keep
  it. Any trap the spike found goes in footguns.md.
- **Waived:** a spike that won't run is written into its assumption, with what stands in for it.
- **Feedback:** a verdict that changes the architecture reopens architecture.md. One that changes a
  behavior reopens behaviors.md. Either way, the documents change in the same commit as the
  verdict.
- **A likely spike:** can the Gherkin steps drive this stack? This is hard for real-time systems and
  games, where state is continuous and timing matters.

### `.pdd/plan.json`, the build

- **For:** Plan the build: cycles of work items, with their dependencies.
- **Needs:** the documents above; for MVP 1, the spikes' verdicts.
- **Ask:** "Plan MVP N's build in .pdd/plan.json: work items in cycles, each with what it cannot
  correctly start until, its size, its risk and the behaviors it delivers. Give each cycle a goal
  and exit criteria that can be checked. End with the MVP's verdict as the one final item."
- **Size items for review:** an item is small enough when its LLD can be read in one sitting, and a
  cycle when its designs can be reviewed together in one. A file that will obviously grow past a
  few hundred lines is a reason to split the item now, not after the code exists.
- **Your items:** the MVP's verdict, and a stop at the end of each cycle while that trust box is
  checked.
- **The gates:** before the first build item, the pre-push hook runs types, lint, tests, the Gherkin
  scenarios, the plan's structure check and the gated rules. Try each check once on the real
  failure it exists for. A check counts once it has been seen to fail on a real case, not only on a
  planted example: in one project a boundary check passed a module that broke its rule, until a
  review noticed.
- **You:** read the plan in dod: the cycles, what each item delivers, what can run in parallel.
- **Ready when:** every behavior of the MVP is delivered by an item, the structure check passes, and
  each gate has been seen to fail on a real case.

## Cycles: phases 3 and 4

Phases 3 and 4 run once per cycle: one logical component, or group of components, that the plan
builds next. A cycle's designs are written and reviewed together, then its items are built one by
one, then the cycle is checked.

## Phase 3: Technical design

### `docs/hld/<cycle>.md`

- **For:** Design a cycle that changes the architecture.
- **Needs:** architecture.md and the plan.

Only for a cycle that adds or changes architecture: new parts, new stored data, new flows between
parts. Any other cycle goes straight to its LLDs, written against architecture.md.

- **It holds:**
  - what the cycle builds and why;
  - its decisions, each marked with who made it and when;
  - what can go wrong, with a guard for each;
  - what it costs;
  - its key flows;
  - what the LLDs must decide;
  - its changes to the glossary, the rules and the assumptions;
  - what it defers.
- **Review:** together with the cycle's LLDs, in the cycle's one design review (see the LLD). A
  finding that would overturn one of your decisions comes to you as a question.
- **Ready when:** the cycle review's findings are settled. Where the cycle changes the architecture,
  architecture.md changes in the same commit.

### `docs/lld/W<id>-<name>.md`

- **For:** Design one work item.
- **Needs:** the cycle's HLD if it has one, otherwise architecture.md; the behaviors it delivers.

A cycle's LLDs are written first, all of them in one session, after its HLD if it has one, and
reviewed together.

- **From the template:**
  - a header: the behaviors it delivers, what it depends on, what you'll open to see it work, and
    whether its code gets the adversarial review, with the reason;
  - **Files:** every file it adds or changes, with what the file is responsible for and what it
    exports. A file missing from this table should not appear in the diff;
  - **Contracts:** the types that cross a module boundary, and what happens to invalid input. It
    fails or reaches the user, never quietly;
  - **Tests:** each test file, what it pins, and the behavior and rule ids it covers;
  - **Not doing:** what a reader would expect here and won't find, and where it happens instead.
- **Design review, one per cycle,** in a fresh session, once all the cycle's LLDs are written: its
  HLD, if it has one, and its LLDs, put into one file, since design-review reads one document. An
  item the plan sizes XL, or rates high risk, also gets a review of its own. The findings are fixed
  in the documents before any of the cycle's code. If the LLD box is checked, the session then
  waits for your word.
- **Ready when:** the cycle's design review is settled, and your word is recorded if the LLD box is
  checked.

**Why one design review per cycle:** a reviewer reads the code and the standing documents before
the document it is given, and that reading, not the document, sets most of the cost. In one
project, 74 reviews of single designs cost 189k to 480k tokens each, median 324k. Among the last of
them, the shortest document, 1,703 words, cost 392k, and one review of a batch's five documents
together, 6,369 words, cost 396k. That review raised 6 findings across the five, where a single
review raises a median of 5, so the items with the most room for a defect, XL or high risk, keep a
review of their own (`research/review-cost-2026-10-04.md`).

## Phase 4: Implementation

### `docs/cycles/<cycle>.md`, the cycle report

- **For:** Build, attack and merge each item, then check the cycle: mutation testing and a reading
  pass.
- **Needs:** the cycle's LLDs, reviewed.

The code is the work; the report is where its results are written. For each item:
1. **Implementation**, in the item's worktree, in its own session. The gates run between edits, and
   each verified step is a commit.
2. **Adversarial review before merge**, in a fresh session. It attacks:
   - the tests, always: "which wrong implementation passes these?";
   - the code, when the item reads outside input, writes stored data or changes what stored data
     means, or does something that can't be undone.

   What it finds is fixed before the merge, and recorded in the report.
3. **Merge**, by the merge procedure, as the trust level says. The item is done when its merge is
   on main.

Then, at the end of the cycle:
- **You:** use what the cycle built. If the cycle-end box is checked, the next cycle waits for your
  item. What you find becomes items in the plan.
- **Mutation testing** on what the cycle changed in the modules that hold data or make decisions:
  the mutants on the files the cycle changed, and on the modules whose tests it changed, each run
  against the whole suite. A tool (Stryker, for TypeScript) makes small changes to the code and
  reruns the tests, and a change no test notices is a gap. Read every survivor the same day. Each
  one is a missing test, dead code, or a change that makes no difference, and each is closed. A run
  over the whole codebase re-judges code no cycle changed and takes hours; it runs only when you
  ask for one.
- **A reading pass** for failures that leave no trace: swallowed errors, work started and never
  finished, success reported for nothing written.
- **Once no session can read the whole codebase:** a check for duplicated code. The agent writes a
  new helper where one already exists.
- **Ready when:** every item is merged; the attack findings, survivors and reading-pass findings
  are settled or turned into plan items; the cycle's exit criteria are met.

**Why these reviews:** in one project, reviews by fresh sessions found 11 of its 15 most
consequential defects, for about 15% of its tokens. Nine of the ten largest code defects they found
were in those three kinds of code. And agent-written tests can pass without testing anything: one
set of removal tests passed on code that removed nothing.

**Why mutation testing and a reading pass:** a mutation run found a fault in code the current model
wrote: an unreadable save was marked writable, so one refused read would have written a new save
over the player's. A reading pass found two hangs that no check could see. Only what the cycle
changed: in one project, two runs over the whole codebase took over two hours each, for 532 and
then 604 mutants, and caught every one both times; between them the batch had changed the files of
135 of the 604, none in its simulation (`research/mutation-scope-2026-10-05.md`).

### `docs/prd/mvp-N.md`, the verdict

- **For:** Judge the MVP and release it.
- **Needs:** the plan's final item, ready.
- **The verdict:** you use the MVP against what it was for, and against the result its PRD said
  would be a no. The verdict goes into its PRD and closes the MVP's final item.
- **Release:** a tag on main, deployed from the tag. If the release box is checked, you first read
  the code that handles outside input, permissions and stored data. A platform built by prompting
  alone, with an earlier model, went to production accepting any Google account as admin and
  running HTML stored in its content. Reading found both.
- **After release,** what users report becomes items.
- **Ready when:** the verdict is written; a release is tagged, on your word if the box is checked.

## After an MVP

An MVP bounds thinking about a product that does not exist yet; it is not a fixed sequence. A
later iteration goes any way a usual feature-changing iteration goes. It reopens the earliest
document it changes, through a change request, and continues from there. A common path:
1. Revise the Vision if the last verdict changes the direction.
2. The new MVP's PRD, the behaviors it adds or changes, and their scenarios; then its medium
   questions.
3. architecture.md as a check: walk the new and changed behaviors through it. Change it only where
   one can't be walked through, and record the decision.
4. Spikes for new high-fragility assumptions.
5. The plan, then its cycles.

## Tooling this process needs

Parts of this process rely on tools that don't exist yet:
- **dod:**
  - `cycles` and `cycle` as names for its `phases` and `phase` (the plan says `batches` today);
  - status derived from git, from the `Done:` trailers and the branches;
  - the plan read from main instead of from a checkout;
  - your items shown as waiting for you;
  - adding a project in one step. Today a project is added by hand to dod's PDD provider config.
- **mlmd, the plugin:**
  - commands: `/mlmd:start`, `/mlmd:next`, `/mlmd:status`, `/mlmd:trust`, `/mlmd:release`;
  - skills: the interview, the LLD template, the merge procedure, handoff;
  - workflows: design-review and adversarial review, which exist today as copies installed from
    the author's workflows repository;
  - hooks: the first-session install, the status line and plan view at session start,
    WorktreeCreate, the trust hook, the refusal of `--no-verify`;
  - templates for the documents.
- **Scripts:** the merge script, the plan's structure check, and the pre-push hook that runs the
  gates.
- **Spikes, once for mlmd, and again when Claude Code changes.** Each has its document in
  `docs/spikes/`:
  - **trust enforcement, done:** ask rules alone miss most ways of writing a command, and a
    PreToolUse hook holds in every mode (see Trust). Auto mode with the real model is still to
    run;
  - **worktree switching, done:** ExitWorktree, then EnterWorktree by name through the
    WorktreeCreate hook (see Git). `/clear` typed by hand is still to check;
  - **the desktop app's worktree checkbox, in progress:** the app calls the WorktreeCreate hook,
    but the hook's process couldn't reach a repository in `~/Documents`. Still open: whether it
    works outside `~/Documents`, and whether the app's diff, base-branch sync and archive work with
    a worktree the hook made.
