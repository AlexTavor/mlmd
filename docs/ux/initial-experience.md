# mlmd: initial user experience

From hearing of mlmd until the first phase (1a, the Vision interview, or its code-and-spec-first
equivalent) is under way. Focus: the Claude Code desktop app on Windows; CLI and web follow later.

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

### 5. First output
- Processing is shown as a summary at the end of each topic: what was written to which document
  and section, and who decided it. (Frank, 2026-10-05)

### 6. Next session
- A session can stop after any answer. The next session opens with a status line (phase, progress,
  next question) and continues there (BR-26). (Frank, 2026-10-05)

## Open questions
- P6 (proposed, awaiting Frank): BR-10 addition, a self-registered joiner's role counts for approvals only after the technical expert reviews it.
- Does a joiner (BA, Uitvoering) also get the per-topic summary, or something lighter?
- How does the technical expert hear of a self-registered joiner to review them?
