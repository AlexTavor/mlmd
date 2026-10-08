# mlmd: initial user experience

From hearing of mlmd until the first phase (1a, the Vision interview, or its code-and-spec-first
equivalent) is under way. Focus: the Claude Code desktop app on Windows; CLI and web follow later.

## Master rule: small, sliding contexts

The user's context and the model's stay small. A session slides from topic to topic; when its
context grows too large for the user to keep in mind, mlmd commits the documents and offers the
next session as a card (one click). A deep dive is a detour
in its own sessions; mlmd keeps the path, shows it in the status line, and brings the user back to
where they left. At 100K tokens over the session's start
mlmd suggests a new session; at 500K it urges one and helps split the work; if ignored, it explains
what large contexts cost. (Frank, 2026-10-05, agreed with Alex; BR-38, D-18, D-19)

The sliding happens within a phase. A session never crosses into the next phase: each phase starts in
a new session, even when the context is still small. (Alex, 2026-10-06, confirmed by Frank; BR-22,
D-21)

## Users

- **Technical expert** (required): programming experience and a good understanding of AI. Starts the
  project. (Frank, 2026-10-05)
- **Business Analyst** and **Uitvoering** (UWV staff who serve its clients): optional, often join
  mid-process, work in the same GitHub repo from the desktop app. (Frank, 2026-10-05; BR-10, BR-37)

## Entry points

| Entry | After 10 minutes the user feels |
| --- | --- |
| Idea or law first | The interview asks sharp questions about *their* product, and they know which goal they're working towards and why. |
| Code and spec first | mlmd extracts the business meaning of their code/specs into the right documents, and they know which documents those will be and why. |
| Joining mid-process | Heard and understood; they see their input being processed into the right context. |

(Frank, 2026-10-05; BR-5, BR-36)

## Journey

### 1. Discover
- Word of mouth; a colleague sends `mlmd.md` (renamed from BOOT.md, BR-1). Only the file:
  opening it in Claude Code explains the rest (BR-29). (Frank, 2026-10-05)
- A mid-process joiner gets the repo link plus "open the desktop app in this folder". (Frank, 2026-10-05)

### 2. Install and 3. Start (desktop, Windows)
- The user attaches `mlmd.md` to a prompt; the plugin installs itself. mlmd asks for an empty
  folder or an existing repo and runs `git init` itself if needed, so the session works in the
  project folder and one-click cards can appear. (Frank, 2026-10-05)
- mlmd asks which entry applies: 1 Idea or law, 2 Code and specs, 3 Joining. (Frank, 2026-10-05)
- Phases are introduced by their canonical purpose line from process.md (Words, Phase), word for
  word, with the number as an anchor: "1a · Decide the whole product: what it is and which MVP each
  feature belongs to". (Frank, 2026-10-08)
- The introduction names the phase's main document, from the same table: "1a · Decide the whole
  product: what it is and which MVP each feature belongs to · `docs/vision.md`". (Frank,
  2026-10-08; the document per phase chosen by Claude, see the open question)
- Before the first question: one screen with the phases, where the user is, and one line on why
  this phase exists (BR-29). (Frank, 2026-10-05)
- A joiner is asked their role on first start and mlmd records it in the register; the technical
  expert can review it later. (Frank, 2026-10-05)

### 4. First questions
- Default: one question at a time with a progress count; the user may ask for numbered batches
  instead. (Frank, 2026-10-05)
- Code and spec first: mlmd reads the code and specs, lists the documents it will fill and why,
  then asks only about gaps and contradictions. (Frank, 2026-10-05)
- Code and spec first, after the first UX test (`ux-test/findings.md`):
  - if mlmd isn't sure which sources the user means, it shows the candidates and the user picks;
  - it shows progress when reading takes more than about 20 seconds;
  - it writes what the sources mean, then asks about gaps: the process is iterative, and for an
    existing system interpreting first is right ("write what users mean");
  - it runs the user through every phase from 1a, even when the project is further along, since
    mlmd targets enterprise applications;
  - topics are the sections of the documents the phase fills, in the order it builds them (for 1a:
    problem, users and core; features and MVPs; constraints; what to keep possible; standing
    documents). Each gap question belongs to its section's topic. The count is per topic and
    overall ("constraints, 1 of 2; question 4 of 5"). A topic with no gaps gets a one-line summary;
  - a gap that decides what gets written at all (scope) is asked before writing;
  - once imported, mlmd's documents are the only master copy.
  - an import that was started before is resumed, not redone: mlmd detects the documents and
    decisions already written, shows what is done and where it stopped, and continues from there.
    Settled questions are never asked again. (Frank, 2026-10-07; UX test)
  (Frank, 2026-10-05, agreed with Alex)
- A count that grows with a follow-up question says so: "question 4 of 4 (1 added)". (Frank, 2026-10-07; UX test)
- Each question ends with one line saying what the answer decides. (Frank, 2026-10-07; UX test)

### 5a. Summary after an import
- Three parts: **decided today** (decision, document and section, who); **changed from your
  source** (superseded, narrowed or reopened); **imported unchanged** (one line per document with
  counts and source, no content repeated). (Frank, 2026-10-05, agreed with Alex)
- Before the summary, mlmd checks each "imported unchanged" count against the file it wrote. (Frank, 2026-10-07; UX test)

### 5b. Foundation pass and standing documents
- On a code-and-specs start, before phase work, mlmd drafts the glossary, footguns and rules from
  the existing material, then offers: 1 go through it together, 2 get mlmd's impression of it
  (BR-39). (Frank, 2026-10-08; UX test on mtg-judge)
- At the end of each topic mlmd reports on the standing documents and makes a call, for example:

  ```
  Foundation check
    glossary  23 terms, solid. Card-rule terms come straight from the CR.
    footguns  4 drafted; thin. ADR-0023/24 show boot-prompt traps, likely more.
    rules     11 from AGENT-RULES.md; 3 have no enforcement yet.
  My call: footguns and rules need another pass before phase work.
    1 Go deeper (rules is your domain: enforcement)   2 Move on anyway
  ```

  Only the operator's answer moves the work on (BR-40). (Frank, 2026-10-08; UX test on mtg-judge)

### 5. First output
- Processing is shown as a summary at the end of each topic: what was written to which document
  and section, and who decided it. (Frank, 2026-10-05)
- Every summary ends with a separate block, "mlmd decided, please confirm": one numbered line per
  decision mlmd made itself. The user replies "ok" or, for example, "2: no". (Frank, 2026-10-06)
- A feature that depends on a later phase (a spike, for example) is marked so in the Vision's
  feature line and under "needed by" in open-questions.md. (Frank, 2026-10-07; UX test)

### 6. Next session
- A session can stop after any answer. The next session opens with a status line (phase, progress,
  next question) and continues there (BR-26). (Frank, 2026-10-05)
- Every session opens with its goal in one or two sentences (what it will achieve and why now),
  then the status line, then the first question. It doesn't list the outputs or what it needs from
  the user; those follow from the conversation. (Frank, 2026-10-06; UX test 3)
- Overview, so the user never feels lost (Frank, 2026-10-07; UX test):
  - the status line has two lines: the phase map (`1a ✓ › 1b ● › 2 Architecture › …`) and the
    current phase's topics with counts, ending with "your focus: …". The user can ask "where are
    we?" for it any time;
  - in the desktop app, a pinned overview artifact: "your focus now" first (open questions,
    decisions to confirm, the next session), then the phase strip, then the current phase's topics.
    mlmd republishes it at each commit and shows when it was updated. In the CLI, `/mlmd:status`
    shows the same.

## Decisions on joiners
- A joiner's self-registered role counts immediately; no review step. Their GitHub push access
  bounds what they can do. (Frank, 2026-10-05; proposal P6 rejected)
- The technical expert may mark questions for a specific role; a joiner with that role is asked
  those. Otherwise a joiner gets the same overview and summaries as everyone. (Frank, 2026-10-05)
- Teams notify each other of new joiners and updates through GitHub, not through mlmd.
  (Frank, 2026-10-05)
- The process.md changes this implies (mlmd.md, code-and-spec-first in mlmd, one-at-a-time
  questions with topic summaries) are made jointly: Alex and Frank own the process together (D-16). (Frank, 2026-10-05)

## Open questions
- Main document per phase, chosen by Claude: 1b is the PRD rather than behaviors.md (the PRD
  names what the MVP adds); 6 is the item's LLD although the code is the real output; 7 has no
  document of its own, so it names the plan, where findings become items. Frank to confirm.
- None for this journey.
