# mlmd: flow as the operator experiences it

How it feels to move through creating a product with mlmd, from the first document to the next
increment. Not the process mechanics: those are Alex's (see For Alex below). Continues
`initial-experience.md`, which ends when work on the first document is under way.

Status: working draft by Claude, 2026-10-10. Frank's decisions are under Decisions; the rest is
not agreed yet. The map is read from
process.md at `8d013a1`, not from a run: the UX test reached only the start of the behaviors
(`ux-test/findings.md`). Persona: one technical expert, desktop app, idea-first start, every trust
box checked.

## Journey map

Session counts are estimates from process.md's rules (a new session per document, a fresh session
per review, one session per item and one per attack).

| # | Step | Sees | Does | Waits for | Feels |
| --- | --- | --- | --- | --- | --- |
| 1 | Vision interview (2–3 sessions) | Questions about their product; topic summaries; "mlmd decided, please confirm"; a foundation check after every topic | Answers, confirms | Little | Engaged, heard. By the 4th foundation check, it repeats |
| 2 | Vision review (fresh session) | A card, then nothing, then findings one at a time | Clicks, waits, settles findings | Minutes for the review | "I thought this was done." A second round |
| 3 | MVP document, PRD | Scope questions; the hypothesis asked again | Decides scope | Little | Engaged; real choices |
| 4 | behaviors.md + review | Dozens of behaviors; asked to read the agent's decisions and every rule | Reads a lot, approves | The review | Reading load; approves without really reading |
| 5 | features/*.feature | The agent writing scenarios | Little | The agent | "Why am I here?" |
| 6 | Medium questions | Questions with defaults, at an unclear moment | Rules on defaults | Little | Interrupted, if they come mid-phase-2 |
| 7 | architecture.md + review | A proposed architecture; "walk B7 through it", once per behavior | Walks dozens of behaviors | The review | A slog; or skims and loses confidence |
| 8 | stack.md | Choices | Decides | Little | Quick, satisfying |
| 9 | PoCs | The agent running things, maybe on their device or data | Provides access, reads verdicts | The verdict | Suspense; a setback if a verdict reopens the architecture |
| 10 | The plan | The dod graph: the whole build for the first time | Reads it | Little | Relief at the overview, or "that's a lot" |
| | | *About 13–18 sessions so far. Nothing runs yet.* | | | |
| 11 | Cycle design (LLDs, review) | LLDs being written, then findings | Accepts several LLDs | Writing and review | Reading load again |
| 12 | Items | Per item: card, "go", code, attack, merge question | Clicks, says go, approves merges | Each implementation and attack | Button-pressing; idle while waiting, or juggling parallel conversations |
| 13 | Cycle end | Mutation and reading-pass results; new items in the plan; "use what the cycle built" | Tries something | Mutation run | The plan grows; nothing to try when the component has no visible face |
| 14 | Next cycle | Back to design | As 11 | As 11 | Fine if they know how many cycles remain |
| 15 | Release | Security-relevant code to read; tag; "use it, record what it showed" | Reads, tags, uses, writes | Deploy | The payoff, presented as a chore. No moment |
| 16 | MVP verdict | The hypothesis and its refuting result | Judges | Real users, possibly weeks | Weighty |
| 17 | Next increment | "Go from the earliest document it changes"; the phase strip back at Requirements; the hypothesis asked again | Works out where to start | — | "What now?" Going backwards. Nagged, if the "no" was deliberate |

## Experience problems

- **F1 The long road to first code.** Phases 1 and 2 take some 13–18 sessions of documents before
  anything runs (steps 1–10). The readers are developers who build. This is where momentum is
  most likely to die, before a cycle is ever reached.
- **F2 Sessions with no role for the operator.** Some documents are written by the agent while the
  operator watches: features (5), the LLDs (11), step definitions. Each still costs a card, a cold
  start and a goal line.
- **F3 "Done" isn't done.** Every document ends in a fresh-session review: a wait of minutes, then a
  second round of questions on work the operator thought was finished (2, 4, 7, 11). The foundation
  check after every topic of every document becomes noise.
- **F4 Reading load the operator can't carry.** All behaviors, every rule, every agent decision,
  every LLD, the security code at release. Too much to read turns approval into a rubber stamp
  (4, 7, 11, 15).
- **F5 From author to button-presser.** In phase 4 the operator says go, approves merges and waits.
  Many stops with little to judge breed approval fatigue, the opposite of what the stops are for.
  Nothing says what to do while an item is being built (12).
- **F6 Nothing to try.** "Use what the cycle built" when the cycle built a data layer (13).
- **F7 Progress that slides back, and no "how much is left".** A PoC reopens the architecture,
  survivors become items, the plan grows; nothing shows cycles or items remaining (9, 13, 14).
- **F8 No payoff, then a blank page.** A release is the moment the product exists, and it reads as
  a checklist. After it, the next increment starts from "the earliest document it changes", the
  phase strip resets, and the hypothesis question returns (15, 17).
- **F9 Three overviews, no nudge.** Status line, pinned overview and dod plan view: which one to
  look at? With parallel conversations, nothing tells the operator that an item waits for them.

## Decisions

- **F1 rejected** (Frank, 2026-10-10). The operator is primed that code comes late, as a side
  effect of good requirements engineering. The wait for code doesn't matter: the tests show what
  the code will do.
- **F2 stays as designed** (Frank, 2026-10-10). Sliding context windows are the best known way to
  keep contexts small. Frank is open to a better way. Whether the plan fires agent-only sessions
  off by itself goes to Alex (For Alex, 7).
- **F3 is how mlmd works, not a problem** (Frank, 2026-10-10). "Done" means good enough to be the
  next Current, in the Agile sense, not finished. Documents are living, and they are mlmd's prime
  citizens, so a review that reopens one is expected.
- **F1, follow-up** (Frank, 2026-10-10): the orientation screen says that code comes late, so the
  priming doesn't depend on reading the README. Written into `initial-experience.md`.
- **F3, follow-up** (Frank, 2026-10-10): the status line shows a ready document as "current", not
  ✓. Written into `initial-experience.md`.
- **F4 goes to Alex** (Frank, 2026-10-10): which rubber stamps can the process lose (For Alex, 8).
- **F6 goes to Alex** (Frank, 2026-10-10): what "use what the cycle built" means when a cycle
  builds nothing the operator can open (For Alex, 5). Claude's proposal for him: a named thing to
  open, or else the cycle's scenarios run and shown in plain language.
- **F7, how much is left: no progress count** (Frank, 2026-10-10). "It is a job, you go at it until
  it works." Knowing how much will still come doesn't matter to the operator.
- **F7, going backwards** (Frank, 2026-10-10): when work reopens a current document, the
  overview's "your focus" says so in one line with the reason. Written into
  `initial-experience.md` §6.

## For Alex

Mechanics gaps noticed while mapping. Not Frank's questions.

1. The PRD's Ask writes the PRD and then the behaviors in one go, but a session never crosses a
   document (BR-22).
2. The medium questions run "before phase 2, alongside it, or after it": nothing says when mlmd
   asks them.
3. Architecture: does the operator walk every behavior, or does the agent walk them and the
   operator check the cross-part ones?
4. process.md has documents change "in the same commit" as the work that finds the change (a PoC
   verdict, an attack); BR-18 has approved product and design documents reopened only through a
   change request. Which applies when a PoC changes a behavior?
5. The cycle-end item when a cycle builds nothing the operator can open.
6. How an operator learns that an item waits for them while they work in another conversation.
7. Does the plan start the sessions where the operator has no role (writing the features, the
   LLDs, the step definitions) by itself, one at a time, without the operator clicking a card?
   (Frank, 2026-10-10.)
8. Which of the operator's approvals can the process drop? Too much to read turns approval into a
   rubber stamp: every behavior, every rule, every agent decision, every LLD, the security code at
   release (F4). (Frank, 2026-10-10.)
