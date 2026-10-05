# mlmd: initial user experience

From hearing of mlmd until the first phase (1a, the Vision interview, or its code-and-spec-first
equivalent) is under way. Focus: the Claude Code desktop app on Windows; CLI and web follow later.

## Master rule: small, sliding contexts

The user's context and the model's stay small. A session slides from topic to topic; when its
context grows too large for the user to keep in mind, mlmd commits the documents and offers the
next session as a card (one click). A deep dive is a detour
in its own sessions; mlmd keeps the path, shows it in the status line, and brings the user back to
where they left. (Frank, 2026-10-05, agreed with Alex; BR-38, D-18)

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
  - once imported, mlmd's documents are the only master copy.
  (Frank, 2026-10-05, agreed with Alex)

### 5a. Summary after an import
- Three parts: **decided today** (decision, document and section, who); **changed from your
  source** (superseded, narrowed or reopened); **imported unchanged** (one line per document with
  counts and source, no content repeated). (Frank, 2026-10-05, agreed with Alex)

### 5. First output
- Processing is shown as a summary at the end of each topic: what was written to which document
  and section, and who decided it. (Frank, 2026-10-05)

### 6. Next session
- A session can stop after any answer. The next session opens with a status line (phase, progress,
  next question) and continues there (BR-26). (Frank, 2026-10-05)

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
- None for this journey.
